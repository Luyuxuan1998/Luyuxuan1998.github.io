---
layout: post
title: "Three.js 使用记录"
comments: true
categories: Web前端

---
## Three.js介绍
Three.js 是一款运行在浏览器中的 3D 引擎，你可以用它创建各种三维场景，包括了摄影机、光影、材质等各种对象。

---
## 项目需求
1. 一款增强现实的飞行引导界面，背景透明，显示Prepar3D摄像机拍摄的内容（前向摄像头视角）。
2. 需要有从近到远的引导框，提示用户穿框飞行，以保证飞行方向、飞行高度正确。
3. 需要有穿过这些框的引导线。

---
## 需求分析（思路）
1. 始终以摄像机所在位置为（0，0，0）点，通过移动场景中位置物体的位置显示动画效果；
2. 使用Three.js加载GLTF格式的3D引导框模型，作为可以移动的物体显示在Three.js场景中；
3. 使用项目后端发送的引导框经纬度等信息，从WGS-84转换为屏幕直角坐标系XYZ，控制引导框相对于Three.js场景摄像机的位置、角度等；
4. 使用项目后端发送的飞机经纬度等信息，从WGS-84转换为屏幕直角坐标系XYZ，控制Three.js场景摄像机的角度；
5. 使用引导框的XYZ信息，得到每个引导框对应的引导点的XYZ信息，连接从近及远的引导点，显示引导线。

---
## 实现（代码Demo）

```
<!-- 定义 -->
<!-- Three.js的长度单位为米，旋转单位为角度 -->
<!-- 屏幕直角坐标系：X轴：右侧为X轴正方向，Y轴：上方为Y轴正方向，Z轴：靠近为Z轴正方向 -->
<!-- 空间直角坐标系：右手规则 -->
<!-- 始终以摄像机所在位置为（0，0，0）点，通过移动场景中位置物体的位置显示动画效果 -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>WGS-84 To Screen</title>
</head>
<body style="margin: 0;">
  <div>
    <script src="./three.js-master/build/three.js"></script>
    <script src="./three.js-master/examples/js/loaders/GLTFLoader.js"></script>
    <!-- <script type="module">
    </script> -->
    <script>
      // 创建一个场景；
      const scene = new THREE.Scene();
      // 创建一个透视摄像机，第一个参数为FOV，单位为角度；第二个参数是长宽比；第三个参数是近截面，第四个参数是远截面
      const camera = new THREE.PerspectiveCamera( 120, 16/9, 0.1, 1000 );
      // 创建一个WebGL渲染器(透明背景)；
      const renderer = new THREE.WebGLRenderer();//{ alpha: true }
      renderer.setSize( window.innerWidth, window.innerHeight );

      // 加载GLTF格式的3D模型，添加到场景
      var model = new THREE.Object3D();
      var loader = new THREE.GLTFLoader();
      loader.load('scene.gltf', function ( gltf ) {
        model = gltf.scene;
        model.name = 'model';
        scene.add(model);
      })
      
      // 通过model.name获取到场景中的3D模型，修改其属性
      var object = scene.getObjectByName(model.name);
      object.position.set(0,0,0);

      camera.position.set(0, 0, 5);
      // 将渲染器的dom元素添加到HTML文档的body中（渲染的内容是一个canvas元素）
      document.body.appendChild( renderer.domElement );
      
      // 初始化摄像机位置(经纬高换算)
      var camera_x = 0;
      var camera_y = 0;
      var camera_z = 0;
      // 初始化物体位置(经纬高换算)
      var object_x = 0;
      var object_y = 0;
      var object_z = 0;

      // WGS-84坐标系转换为屏幕直角坐标系
      // 算法参考自：https://blog.csdn.net/qq_42764468/article/details/99658677
      function WGS84ToScreenXYZ(Longitude, Latitude, Altitude)
      {
        var f = 1 / 298.257223563;            // WGS84 椭球扁率
        var a = 6378137;                      // WGS84 长半轴
        var b = a * ( 1 - f );                // WGS84 短半轴
        var e = Math.sqrt(Math.pow(a, 2) - Math.pow(b, 2)) / a; // 椭球第一偏心率
        var W = Math.sqrt(1 - Math.pow(e, 2) * Math.pow(Math.sin(Latitude * Math.PI / 180), 2));  // 第一辅助系数
        var N = a / W;                        // 椭球面卯酉圈的曲率半径
        var WGS84_x = (N + Altitude) * Math.cos(Latitude * Math.PI / 180) * Math.cos(Longitude * Math.PI / 180);  // 空间直角坐标系x
        var WGS84_y = (N + Altitude) * Math.cos(Latitude * Math.PI / 180) * Math.sin(Longitude * Math.PI / 180);  // 空间直角坐标系y
        var WGS84_z = (N * (1 - Math.pow(e, 2)) + Altitude) * Math.sin(Latitude * Math.PI / 180);                 // 空间直角坐标系z

        // 空间直角坐标系转为屏幕直角坐标系并返回
        return{
          screen_x: WGS84_y,
          screen_y: WGS84_z,
          screen_z: WGS84_x
        }
      }

      // 通过WebSocket获取物体的经纬高，Pitch, Bank, Heading信息
      var Lon = 0;
      var Lat = 0;
      var Alt = 0;
      var Pitch = 0;
      var Bank = 0;
      var Heading = 0;

      // 通过WebSocket获取飞机的经纬高，Pitch, Bank, Heading信息
      var camera_Lon = 0;
      var camera_Lat = 0;
      var camera_Alt = 0;
      var camera_Pitch = 0;
      var camera_Bank = 0;
      var camera_Heading = 0;

      // 通过经纬高获得物体在屏幕空间的位置
      var obj = WGS84ToScreenXYZ(Lon, Lat, Alt);
      object_x = obj.screen_x;
      object_y = obj.screen_y;
      object_z = obj.screen_z;

      // 通过经纬高获得摄像机在屏幕空间的位置
      var camera_obj = WGS84ToScreenXYZ(camera_Lon, camera_Lat, camera_Alt);
      camera_x = camera_obj.screen_x;
      camera_y = camera_obj.screen_y;
      camera_z = camera_obj.screen_z;

      // 计算插值，获得物体相对于屏幕空间坐标原点的位置
      function diff_xyz(x, y, z) {
        return {
          diff_x: x - camera_x,
          diff_y: y - camera_y,
          diff_z: z - camera_z
        }
      }
      var diff = diff_xyz(object_x, object_y, object_z)

      var object_position_x = diff.diff_x;
      var object_position_y = diff.diff_y;
      var object_position_z = diff.diff_z;

      var object_rotation_x = Pitch;
      var object_rotation_y = Heading;
      var object_rotation_z = Bank;

      var HighWayWigth = 5;
      // 使用三维向量确定两个点，再用三维线段连接这两个点
      const Point1 = new THREE.Vector3( diff.diff_x, diff.diff_y - HighWayWigth/2 , diff.diff_z);
      const Point2 = new THREE.Vector3( diff.diff_x, diff.diff_y - HighWayWigth/2 , diff.diff_z + 10);
      const Line = new THREE.Line3(Point1, Point2);
      scene.add(Line);

      var camera_rotation_x = camera_Pitch;
      var camera_rotation_y = camera_Heading;
      var camera_rotation_z = camera_Bank;

      object.position.set(object_position_x, object_position_y, object_position_z);   // 设置物体的位置
      object.rotation.set(object_rotation_x, object_rotation_y, object_rotation_y);    // 设置物体的角度
      
      camera.rotation.set(camera_rotation_x, camera_rotation_y, camera_rotation_z); // 设置摄像机的角度
      
      // 动画测试
      function animate() {
        requestAnimationFrame( animate );
        renderer.render( scene, camera );
      }
      animate();
    </script>
  </div>
</body>
</html>
```

---
## 注意
由于谷歌浏览器等不支持直接读取本地文件，会出现域访问问题。
所以此时需要使用一个web服务器（如Nginx），通过web服务访问本地文件即可。

---
