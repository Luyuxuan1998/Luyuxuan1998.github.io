---
layout: post
title: "ASP.NET Core MVC学习笔记"
comments: true
categories: ASP.NET

---

# ASP.NET Core MVC学习笔记

ASP.NET Core MVC 是使用“模型-视图-控制器”设计模式构建 Web 应用和 API 的丰富框架。

## 概述

### MVC模式介绍

模型-视图-控制器 (MVC) 体系结构模式将应用程序分成 3 个主要组件组：模型、视图和控制器。

此模式有助于实现[关注点分离](https://docs.microsoft.com/zh-cn/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)。 

使用此模式，用户请求被路由到控制器，后者负责使用模型来执行用户操作和/或检索查询结果。 控制器选择要显示给用户的视图，并为其提供所需的任何模型数据。

![MVC](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview/_static/mvc.png?view=aspnetcore-5.0)

#### 模型组件的职责

MVC 应用程序的模型 (M) 表示**应用程序和任何应由其执行的业务逻辑或操作的状态**。 业务逻辑应与保持应用程序状态的任何实现逻辑一起封装在模型中。 **强类型视图**通常使用 ViewModel 类型，旨在包含要在该视图上显示的数据。 控制器从模型创建并填充 ViewModel 实例。

#### 视图组件的职责

视图 (V) 负责**通过用户界面展示内容**。 它们使用[ Razor 视图引擎](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#razor-view-engine)在 HTML 标记中嵌入 .net 代码。 视图中应该有最小逻辑，并且其中的任何逻辑都必须与展示内容相关。 如果发现需要在视图文件中执行大量逻辑以显示复杂模型中的数据，请考虑使用 [View Component](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/view-components?view=aspnetcore-5.0)、ViewModel 或视图模板来简化视图。

#### 控制器组件的职责

控制器 (C) 是**处理用户交互、使用模型并最终选择要呈现的视图的组件**。 在 MVC 应用程序中，视图仅显示信息；控制器处理并响应用户输入和交互。 在 MVC 模式中，控制器是初始入口点，负责选择要使用的模型类型和要呈现的视图（因此得名 - 它控制应用如何响应给定请求）。

### ASP.NET Core MVC介绍

ASP.NET Core MVC 框架是轻量级、开源、高度可测试的演示框架，并针对 ASP.NET Core 进行了优化。

ASP.NET Core MVC 提供一种基于模式的方式，用于生成可彻底分开管理事务的动态网站。 它提供对标记的完全控制，支持 TDD 友好开发并使用最新的 Web 标准。

#### 功能

ASP.NET Core MVC 包括以下功能：

- [路由](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#routing)
- [模型绑定](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#model-binding)
- [模型验证](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#model-validation)
- [依赖关系注入](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0)
- [筛选器](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#filters)
- [Areas](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#areas)
- [Web API](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#web-apis)
- [Testability](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#testability)
- [Razor 查看引擎](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#razor-view-engine)
- [强类型视图](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#strongly-typed-views)
- [标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#tag-helpers)
- [查看组件](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/overview?view=aspnetcore-5.0#view-components)

#### 路由

ASP.NET Core MVC 建立在 [ASP.NET Core 的路由](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/routing?view=aspnetcore-5.0)之上，是一个功能强大的 URL 映射组件，可用于生成具有易于理解和可搜索 URL 的应用程序。 它可让你定义适用于搜索引擎优化 (SEO) 和链接生成的应用程序 URL 命名模式，而不考虑如何组织 Web 服务器上的文件。 可以使用支持路由值约束、默认值和可选值的方便路由模板语法来定义路由。

使用 *基于约定的路由*，可以全局定义应用程序接受的 URL 格式，并说明每种格式如何映射到给定控制器上的特定操作方法。 接收传入请求时，路由引擎分析 URL 并将其匹配到定义的 URL 格式之一，然后调用关联的控制器操作方法。

```C#
routes.MapRoute(name: "Default", template: "{controller=Home}/{action=Index}/{id?}");
```

借助属性路由，可以通过用定义应用程序路由的属性修饰控制器和操作来指定路由信息。 这意味着路由定义位于与之相关联的控制器和操作旁。

```C#
[Route("api/[controller]")]
public class ProductsController : Controller
{
    [HttpGet("{id}")]
    public IActionResult GetProduct(int id)
    {
      ...
    }
}
```

#### 模型绑定

ASP.NET Core MVC [模型绑定](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)将客户端请求数据（窗体值、路由数据、查询字符串参数、HTTP 头）转换到控制器可以处理的对象中。 因此，控制器逻辑不必找出传入的请求数据；它只需具备作为其操作方法的参数的数据。

```C#
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null) { ... }
```

#### 模型验证

ASP.NET Core MVC 通过使用数据注释验证属性修饰模型对象来支持[验证](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/validation?view=aspnetcore-5.0)。 验证属性在值发布到服务器前在客户端上进行检查，并在调用控制器操作前在服务器上进行检查。

```C#
using System.ComponentModel.DataAnnotations;
public class LoginViewModel
{
    [Required]
    [EmailAddress]
    public string Email { get; set; }

    [Required]
    [DataType(DataType.Password)]
    public string Password { get; set; }

    [Display(Name = "Remember me?")]
    public bool RememberMe { get; set; }
}
```

控制器操作：

```C#
public async Task<IActionResult> Login(LoginViewModel model, string returnUrl = null)
{
    if (ModelState.IsValid)
    {
      // work with the model
    }
    // At this point, something failed, redisplay form
    return View(model);
}
```

框架处理客户端和服务器上的验证请求数据。 在模型类型上指定的验证逻辑作为非介入式注释添加到呈现的视图，并使用 [jQuery 验证](https://jqueryvalidation.org/)在浏览器中强制执行。

#### 依赖关系注入

ASP.NET Core 内置有对[依赖关系注入 (DI)](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0) 的支持。 在 ASP.NET Core MVC 中，[控制器](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/dependency-injection?view=aspnetcore-5.0)可通过其构造函数请求所需服务，使其能够遵循 [Explicit Dependencies Principle](https://docs.microsoft.com/zh-cn/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)（显式依赖关系原则）。

应用还可通过 `@inject` 指令使用[视图文件中的依赖关系注入](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/dependency-injection?view=aspnetcore-5.0)：

```CSHTML
@inject SomeService ServiceName

<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ServiceName.GetTitle</title>
</head>
<body>
    <h1>@ServiceName.GetTitle</h1>
</body>
</html>
```

#### 筛选器

[筛选器](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/filters?view=aspnetcore-5.0)帮助开发者封装横切关注点，例如异常处理或授权。 筛选器允许操作方法运行自定义预处理和后处理逻辑，并且可以配置为在给定请求的执行管道内的特定点上运行。 筛选器可以作为属性应用于控制器或操作（也可以全局运行）。 此框架中包括多个筛选器（例如 `Authorize`）。 `[Authorize]` 是用于创建 MVC 授权筛选器的属性。

```C#
[Authorize]
public class AccountController : Controller
```

#### Areas

[区域](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/areas?view=aspnetcore-5.0)提供将大型 ASP.NET Core MVC Web 应用分区为较小功能分组的方法。 区域是应用程序内的一个 MVC 结构。 在 MVC 项目中，模型、控制器和视图等逻辑组件保存在不同的文件夹中，MVC 使用命名约定来创建这些组件之间的关系。 对于大型应用，将应用分区为独立的高级功能区域可能更有利。 例如，具有多个业务单位的电子商务应用程序，如结帐、计费和搜索等。其中每个单位都有自己的逻辑组件视图、控制器和模型。

#### Web API

除了作为生成网站的强大平台，ASP.NET Core MVC 还对生成 Web API 提供强大的支持。 可以生成可连接大量客户端（包括浏览器和移动设备）的服务。

该框架包括对 HTTP 内容协商的支持，后者有允许[设置数据格式](https://docs.microsoft.com/zh-cn/aspnet/core/web-api/advanced/formatting?view=aspnetcore-5.0)为 JSON 或 XML 的内置支持。 编写[自定义格式化程序](https://docs.microsoft.com/zh-cn/aspnet/core/web-api/advanced/custom-formatters?view=aspnetcore-5.0)以添加对自己格式的支持。

使用链接生成启用对超媒体的支持。 轻松启用对[跨域资源共享 (CORS)](https://www.w3.org/TR/cors/) 的支持，以便 Web API 可以跨多个 Web 应用程序共享。

#### Testability

框架对界面和依赖项注入的使用非常适用于单元测试，并且该框架还包括使得[集成测试](https://docs.microsoft.com/zh-cn/aspnet/core/test/integration-tests?view=aspnetcore-5.0)快速轻松的功能（例如 TestHost 和实体框架的 InMemory 提供程序）。 详细了解[如何测试控制器逻辑](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/testing?view=aspnetcore-5.0)。

#### Razor查看引擎

[ASP.NET CORE MVC 视图](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/overview?view=aspnetcore-5.0)使用[ Razor 视图引擎](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0)呈现视图。 Razor 是一种紧凑、富于表现力且流畅的模板标记语言，用于使用 embedded c # 代码定义视图。 Razor 用于在服务器上动态生成 web 内容。 可以完全混合服务器代码与客户端内容和代码。

```CSHTML
<ul>
    @for (int i = 0; i < 5; i++) {
        <li>List item @i</li>
    }
</ul>
```

使用 Razor 视图引擎可以定义 [布局](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/layout?view=aspnetcore-5.0)、 [分部视图](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/partial?view=aspnetcore-5.0) 和可替换部分。

#### 强类型视图

Razor MVC 中的视图可以基于模型进行强类型化。 控制器可以将强类型化的模型传递给视图，使视图具备类型检查和 IntelliSense 支持。

例如，以下视图呈现类型为 `IEnumerable<Product>` 的模型：

```CSHTML
@model IEnumerable<Product>
<ul>
    @foreach (Product p in Model)
    {
        <li>@p.Name</li>
    }
</ul>
```

#### 标记帮助程序

[标记帮助](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0) 程序使服务器端代码可以在文件中参与创建和呈现 HTML 元素 Razor 。 可以使用标记帮助程序定义自定义标记（例如 `<environment>`），或者修改现有标记的行为（例如 `<label>`）。 标记帮助程序基于元素名称及其属性绑定到特定的元素。 它们提供了服务器端呈现的优势，同时仍然保留了 HTML 编辑体验。

有多种常见任务（例如创建表单、链接，加载资产等）的内置标记帮助程序，公共 GitHub 存储库和 NuGet 包中甚至还有更多可用标记帮助程序。 标记帮助程序使用 C# 创建，基于元素名称、属性名称或父标记以 HTML 元素为目标。 例如，内置 LinkTagHelper 可以用来创建指向 `AccountsController` 的 `Login` 操作的链接：

```CSHTML
<p>
    Thank you for confirming your email.
    Please <a asp-controller="Account" asp-action="Login">Click here to Log in</a>.
</p>
```

可以使用 `EnvironmentTagHelper` 在视图中包括基于运行时环境（例如开发、暂存或生产）的不同脚本（例如原始或缩减脚本）：

```CSHTML
<environment names="Development">
    <script src="~/lib/jquery/dist/jquery.js"></script>
</environment>
<environment names="Staging,Production">
    <script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-2.1.4.min.js"
            asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
            asp-fallback-test="window.jQuery">
    </script>
</environment>
```

标记帮助程序提供 HTML 友好的开发体验，并提供丰富的 IntelliSense 环境用于创建 HTML 和 Razor 标记。 大多数内置标记帮助程序以现有 HTML 元素为目标，为该元素提供服务器端属性。

#### 视图组件

通过[视图组件](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/view-components?view=aspnetcore-5.0)可以包装呈现逻辑并在整个应用程序中重用它。 这些组件类似于[分部视图](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/partial?view=aspnetcore-5.0)，但具有关联逻辑。

## 教程：在 ASP.NET Core 中开始使用 Razor Pages

在本教程中，你将了解：

- 创建 Razor 页面 Web 应用。
- 运行应用。
- 检查项目文件。

在本教程结束时，你将有一个工作的 Razor Pages Web 应用。在后续教程中，你可以在其基础上进行增强。

![Home 或 Index 页](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/razor-pages/razor-pages-start/_static/5/home5.png?view=aspnetcore-5.0)

### 先决条件

- [Visual Studio](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-5.0&tabs=visual-studio#tabpanel_1_visual-studio)
- [Visual Studio Code](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-5.0&tabs=visual-studio#tabpanel_1_visual-studio-code)
- [Visual Studio for Mac](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-5.0&tabs=visual-studio#tabpanel_1_visual-studio-mac)

- 具有“ASP.NET 和 Web 开发”工作负载的 [Visual Studio 2019 16.8 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
- [.NET 5.0 SDK 或更高版本](https://dotnet.microsoft.com/download/dotnet/5.0)

### 链接

[教程链接]: https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/razor-pages/razor-pages-start?view=aspnetcore-5.0&amp;tabs=visual-studio

### Pages 文件夹

包含 Razor 页面和支持文件。 每个 Razor 页面都是一对文件：

- 一个 .cshtml 文件，其中包含使用 Razor 语法的 C# 代码的 HTML 标记。
- 一个 .cshtml.cs 文件，其中包含处理页面事件的 C# 代码。

支持文件的名称以下划线开头。 例如，_Layout.cshtml 文件可配置所有页面通用的 UI 元素。 此文件设置页面顶部的导航菜单和页面底部的版权声明。 有关详细信息，请参阅 [ASP.NET Core 中的布局](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/layout?view=aspnetcore-5.0)。

### wwwroot 文件夹

包含静态资产，如 HTML 文件、JavaScript 文件和 CSS 文件。 有关详细信息，请参阅 [ASP.NET Core 中的静态文件](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/static-files?view=aspnetcore-5.0)。

### appsettings.json

包含配置数据，如连接字符串。 有关详细信息，请参阅 [ASP.NET Core 中的配置](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0)。

### Program.cs

包含应用的入口点。 有关详细信息，请参阅 [ASP.NET Core 中的 .NET 通用主机](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-5.0)。

### Startup.cs

包含配置应用行为的代码。 有关详细信息，请参阅 [ASP.NET Core 中的应用启动](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/startup?view=aspnetcore-5.0)。

## ASP.NET Core MVC

[ASP.NET Core MVC入门]: https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/start-mvc?view=aspnetcore-5.0&amp;tabs=visual-studio

这是本系列教程的第一个教程，介绍具有控制器和视图的 ASP.NET Core MVC Web 开发。

在本系列结束时，你将拥有一个管理和显示电影数据的应用。 您将学习如何：

- 创建 Web 应用。
- 添加和构架模型。
- 使用数据库。
- 添加搜索和验证。

[查看或下载示例代码](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/first-mvc-app/start-mvc/sample)（[如何下载](https://docs.microsoft.com/zh-cn/aspnet/core/introduction-to-aspnet-core?view=aspnetcore-5.0#how-to-download-a-sample)）。

### 入门

#### 先决条件

- 具有“ASP.NET 和 Web 开发”工作负载的 [Visual Studio 2019 16.8 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)
- [.NET 5.0 SDK 或更高版本](https://dotnet.microsoft.com/download/dotnet/5.0)

#### 创建 Web 应用

- 启动 Visual Studio 并选择“创建新项目”。
- 在“新建项目”对话框中，选择“ASP.NET Core Web 应用程序”>“下一步”。
- 在“配置新项目”对话框中，为“项目名称”输入 `MvcMovie`。 务必要将项目命名为“MvcMovie”。 复制代码时，大小写需要匹配每个 `namespace` 匹配项。
- 选择“创建”。
- 在“创建新的 ASP.NET Core Web 应用程序”对话框中，选择：
  - 下拉列表中的“.NET Core”和“ASP.NET Core 5.0”。
  - ASP.NET Core Web 应用程序（模型-视图-控制器）。
  - **Create**。

![创建新的 ASP.NET Core Web 应用呈现 ](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/start-mvc/_static/mvcvs19v16.9.png?view=aspnetcore-5.0)

#### 运行应用

- 选择 Ctrl+F5 以在不使用调试程序的情况下运行应用。

  Visual Studio 会显示以下对话框：

  ![此项目配置为使用 SSL。 要避免浏览器中出现 SSL 警告，可以选择信任 IIS Express 生成的自签名证书。 要信任 IIS Express SSL 证书吗？](https://docs.microsoft.com/zh-cn/aspnet/core/getting-started/_static/trustcert.png?view=aspnetcore-5.0)

  如果信任 IIS Express SSL 证书，请选择“是”。

  将显示以下对话框：

  ![安全警告对话](https://docs.microsoft.com/zh-cn/aspnet/core/getting-started/_static/cert.png?view=aspnetcore-5.0)

  如果你同意信任开发证书，请选择“是”。

  有关信任 Firefox 浏览器的信息，请参阅 [Firefox SEC_ERROR_INADEQUATE_KEY_USAGE 证书错误](https://docs.microsoft.com/zh-cn/aspnet/core/security/enforcing-ssl?view=aspnetcore-5.0#trust-ff)。

  Visual Studio：

  - 启动 [IIS Express](https://docs.microsoft.com/zh-cn/iis/extensions/introduction-to-iis-express/iis-express-overview)。
  - 运行应用。

  地址栏显示 `localhost:port#`，而不是显示 `example.com`。 本地计算机的标准主机名为 `localhost`。 当 Visual Studio 创建 Web 项目时，对 Web 服务器使用的是随机端口。

在不进行调试的情况下，通过选择 Ctrl+F5 启动应用，可以：

- 更改代码。
- 保存文件。
- 快速刷新浏览器并查看代码更改。

可以从“调试”菜单项中以调试或非调试模式启动应用：

![调试菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/start-mvc/_static/debug_menu50.png?view=aspnetcore-5.0)

可以通过选择“IIS Express”按钮来调试应用

![IIS Express](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/start-mvc/_static/iis_express50.png?view=aspnetcore-5.0)

下图显示该应用：

![Home 或索引页](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/start-mvc/_static/home50-vs.png?view=aspnetcore-5.0)

### 将控制器添加到 ASP.NET Core MVC 应用

模型-视图-控制器 (MVC) 体系结构模式将应用分成 3 个主要组件：模型 (M)、视图 (V) 和控制器 (C) 。 MVC 模式有助于创建比传统单片应用更易于测试和更新的应用。

基于 MVC 的应用包含：

- 模型 (M)：表示应用数据的类。 模型类使用验证逻辑来对该数据强制实施业务规则。 通常，模型对象检索模型状态并将其存储在数据库中。 本教程中，`Movie` 模型将从数据库中检索电影数据，并将其提供给视图或对其进行更新。 更新后的数据将写入到数据库。
- 视图 (V)：视图是显示应用用户界面 (UI) 的组件。 此 UI 通常会显示模型数据。
- 控制器：可执行以下操作的类：
  - 处理浏览器请求。
  - 检索模型数据。
  - 调用返回响应的视图模板。

在 MVC 应用中，视图仅显示信息。 控制器处理用户输入和交互并对其进行响应。 例如，控制器处理 URL 段和查询字符串值，并将这些值传递给模型。 该模型可使用这些值查询数据库。 例如：

- `https://localhost:5001/Home/Privacy`：指定 `Home` 控制器和 `Privacy` 操作。
- `https://localhost:5001/Movies/Edit/5`：是使用 `Movies` 控制器和 `Edit` 操作编辑 ID=5 的电影的请求，本教程稍后将对此进行详细介绍。

本教程的后续部分中将介绍路由数据。

MVC 体系结构模式将应用分成 3 组主要组件：模型、视图和控制器。 此模式有助于实现关注点分离：UI 逻辑位于视图中。 输入逻辑位于控制器中。 业务逻辑位于模型中。 这种隔离有助于控制构建应用时的复杂程度，因为它可用于一次处理一个实现特性，而不影响其他特性的代码。 例如，处理视图代码时不必依赖业务逻辑代码。

本教程系列介绍并演示了这些概念，同时生成一个电影应用。 MVC 项目包含“控制器”和“视图”文件夹 。

#### 添加控制器

在“解决方案资源管理器”中，右键单击“控制器”>“添加”>“控制器” 。

![在“解决方案资源管理器”中，右键单击“控制器”>“添加”>“控制器”](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-controller/_static/add_controllercopyvs19v16.9.png?view=aspnetcore-5.0)

在“添加基架”对话框中，选择“MVC 控制器 - 空” 。

![添加 MVC 控制器并为其命名](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-controller/_static/accopyvs19v16.9.png?view=aspnetcore-5.0)

在“添加新项 - MvcMovie”对话框中，输入 HelloWorldController.cs，然后选择“添加” 。

将“Controllers/HelloWorldController.cs”的内容替换为以下内容：

```C#
using Microsoft.AspNetCore.Mvc;
using System.Text.Encodings.Web;

namespace MvcMovie.Controllers
{
    public class HelloWorldController : Controller
    {
        // 
        // GET: /HelloWorld/

        public string Index()
        {
            return "This is my default action...";
        }

        // 
        // GET: /HelloWorld/Welcome/ 

        public string Welcome()
        {
            return "This is the Welcome action method...";
        }
    }
}
```

控制器中的每个 `public` 方法均可作为 HTTP 终结点调用。 上述示例中，两种方法均返回一个字符串。 请注意每个方法前面的注释。

一个 HTTP 终结点：

- 是 Web 应用程序中可定向的 URL，如 `https://localhost:5001/HelloWorld`。
- 结合以下内容：
  - 所用的协议：`HTTPS`。
  - Web 服务器的网络位置，包括 TCP 端口：`localhost:5001`。
  - 目标 URI：`HelloWorld`。

第一条注释指出这是一个 [HTTP GET](https://developer.mozilla.org/docs/Web/HTTP/Methods/GET) 方法，它通过向基 URL 追加 `/HelloWorld/` 进行调用。

第二条注释指定一个 [HTTP GET](https://developer.mozilla.org/docs/Web/HTTP/Methods) 方法，它通过向 URL 追加 `/HelloWorld/Welcome/` 进行调用。 本教程稍后将使用基架引擎生成 `HTTP POST` 方法，用于更新数据。

在不使用调试程序的情况下运行应用。

将“HelloWorld”追加到地址栏中的路径。 `Index` 方法返回一个字符串。

![显示“这是我的默认操作”应用响应的浏览器窗口](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-controller/_static/hell1.png?view=aspnetcore-5.0)

MVC 根据入站 URL 调用控制器类以及其中的操作方法。 MVC 所用的默认 [URL 路由逻辑](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/routing?view=aspnetcore-5.0)使用如下格式来确定调用的代码：

```
/[Controller]/[ActionName]/[Parameters]
```

在 Startup.cs 文件的 `Configure` 方法中设置路由格式。

```C#
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

如果浏览到应用且不提供任何 URL 段，它将默认为上面突出显示的模板行中指定的“Home”控制器和“Index”方法。 在前面的 URL 段中：

- 第一个 URL 段决定要运行的控制器类。 因此 `localhost:5001/HelloWorld` 映射到 HelloWorld 控制器类。
- 该 URL 段的第二部分决定类上的操作方法。 因此 `localhost:5001/HelloWorld/Index` 触发 `HelloWorldController` 类的 `Index` 方法来运行。 请注意，只需浏览到 `localhost:5001/HelloWorld`，而 `Index` 方法默认调用。 `Index` 是默认方法，如果未显式指定方法名称，则将在控制器上调用它。
- URL 段的第三部分 (`id`) 针对的是路由数据。 本教程的后续部分中将介绍路由数据。

浏览到 `https://localhost:{PORT}/HelloWorld/Welcome`。 将 `{PORT}` 替换为端口号。

`Welcome` 方法将运行并返回字符串 `This is the Welcome action method...`。 对于此 URL，采用 `HelloWorld` 控制器和 `Welcome` 操作方法。 目前尚未使用 URL 的 `[Parameters]` 部分。

![显示“这是 Welcome 操作方法”应用程序响应的浏览器窗口](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-controller/_static/welcome.png?view=aspnetcore-5.0)

修改代码，将一些参数信息从 URL 传递到控制器。 例如 `/HelloWorld/Welcome?name=Rick&numtimes=4`。==利用URL传参==

更改 `Welcome` 方法以包括以下代码中显示的两个参数：

```C#
// GET: /HelloWorld/Welcome/ 
// Requires using System.Text.Encodings.Web;
public string Welcome(string name, int numTimes = 1)
{
    return HtmlEncoder.Default.Encode($"Hello {name}, NumTimes is: {numTimes}");
}
```

前面的代码：

- 使用 C# 可选参数功能指示，未为 `numTimes` 参数传递值时该参数默认为 1。
- ==使用 `HtmlEncoder.Default.Encode` 防止恶意输入（例如通过 JavaScript）损害应用。==
- 在 `$"Hello {name}, NumTimes is: {numTimes}"` 中使用[内插字符串](https://docs.microsoft.com/zh-cn/dotnet/articles/csharp/language-reference/keywords/interpolated-strings)。

运行应用并浏览到 `https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4`。 将 `{PORT}` 替换为端口号。

在 URL 中对 `name` 和 `numtimes` 使用其他值。 MVC [模型绑定](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)系统可将命名参数从查询字符串自动映射到方法中的参数。 有关详细信息，请参阅[模型绑定](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)。

![显示 Hello Rick 的应用程序响应的浏览器窗口，NumTimes 为 : 4](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-controller/_static/rick4.png?view=aspnetcore-5.0)

在上图中：

- 未使用 `Parameters` URL 段。
- 在[查询字符串](https://wikipedia.org/wiki/Query_string)中传递 `name` 和 `numTimes` 参数。
- ==上述 URL 中的 `?`（问号）为分隔符，后接查询字符串。==
- ==`&` 字符将字段/值对分隔开。==

将 `Welcome` 方法替换为以下代码：

C#复制

```csharp
public string Welcome(string name, int ID = 1)
{
    return HtmlEncoder.Default.Encode($"Hello {name}, ID: {ID}");
}
```

运行应用并输入以下 URL：`https://localhost:{PORT}/HelloWorld/Welcome/3?name=Rick`

在上述 URL 中：

- ==第三个 URL 段与路由参数 `id` 相匹配。==
- `Welcome` 方法包含 `MapControllerRoute` 方法中匹配 URL 模板的参数 `id`。
- 尾随的 `?` 启动[查询字符串](https://wikipedia.org/wiki/Query_string)。

==如果`https://localhost:{PORT}/HelloWorld/Welcome/?name=Rick`，则ID为默认值1==

```C#
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

	### 将视图添加到 ASP.NET Core MVC 应用

在此部分中，你将修改 `HelloWorldController` 类以使用 [Razor](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/razor?view=aspnetcore-5.0) 视图文件。 这顺利封装了为客户端生成 HTML 响应的过程。

==视图模板==是使用 Razor 创建的。 基于 Razor 的视图模板：

- ==具有 .cshtml 文件扩展名。==
- 提供一种巧妙的方法来使用 C# 创建 HTML 输出。

当前，`Index` 方法返回一个字符串，其中包含控制器类中的消息。 在 `HelloWorldController` 类中，将 `Index` 方法替换为以下代码：

```C#
public IActionResult Index()
{
    return View();
}
```

前面的代码：

- 调用该控制器的 [View](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.controller.view) 方法。
- 使用视图模板生成 HTML 响应。

控制器方法：

- 称为“操作方法”。 例如，上述代码中的 `Index` 操作方法。
- 通常返回 [IActionResult](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.iactionresult) 或从 [ActionResult](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.actionresult) 派生的类，而不是 `string` 这样的类型。

#### 添加视图

右键单击“视图”文件夹，然后单击“添加”>“新文件夹”，并将文件夹命名为“HelloWorld”。

右键单击“Views/HelloWorld”文件夹，然后单击“添加”>“新项”。

在“添加新项 - MvcMovie”对话框中：

- 在右上角的搜索框中，输入“视图”
- 选择“Razor 视图”
- 保持“名称”框的值：Index.cshtml。
- 选择“添加”

![“添加新项”对话框](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-view/_static/add_view50.png?view=aspnetcore-5.0)

使用以下内容替换 Razor 视图文件 Views/HelloWorld/Index.cshtml 的内容：

CSHTML复制

```cshtml
@{
    ViewData["Title"] = "Index";
}

<h2>Index</h2>

<p>Hello from our View Template!</p>
```

导航到 `https://localhost:{PORT}/HelloWorld`：

- `HelloWorldController` 中的 `Index` 方法运行 `return View();` 语句，指定此方法应使用视图模板文件来呈现对浏览器的响应。
- 由于未指定视图模板文件名称，因此 MVC 默认使用默认视图文件。 如果未指定视图文件名称，则返回默认视图。 默认视图与操作方法的名称相同，在本例中为 `Index`。 使用视图模板 /Views/HelloWorld/Index.cshtml。
- 下图显示了视图中硬编码的 字符串“Hello from our View Template!”：

![浏览器窗口](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-view/_static/hell_template.png?view=aspnetcore-5.0)

#### 更改视图和布局页面

选择菜单链接“MvcMovie”、“Home”和“Privacy” 。 每页显示相同的菜单布局。 菜单布局是在 Views/Shared/_Layout.cshtml 文件中实现的。

打开 Views/Shared/_Layout.cshtml 文件。

[布局](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/layout?view=aspnetcore-5.0)模板允许：

- 在一个位置指定站点的 HTML 容器布局。
- 在该站点的多个页面上应用 HTML 容器布局。

查找 `@RenderBody()` 行。 `RenderBody` 是显示创建的所有特定于视图的页面的占位符，已包装在布局页面中。 例如，如果选择“Privacy”链接，Views/Home/Privacy.cshtml 视图将在 `RenderBody` 方法中呈现 。

#### 更改布局文件中的标题、页脚和菜单链接

将 Views/Shared/_Layout.cshtml 文件的内容替换为以下标记。 突出显示所作更改：

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>@ViewData["Title"] - Movie App</title>
    <link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.min.css" />
    <link rel="stylesheet" href="~/css/site.css" />
</head>
<body>
    <header>
        <nav class="navbar navbar-expand-sm navbar-toggleable-sm navbar-light bg-white border-bottom box-shadow mb-3">
            <div class="container">
                <a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>
                <button class="navbar-toggler" type="button" data-toggle="collapse" data-target=".navbar-collapse" aria-controls="navbarSupportedContent"
                        aria-expanded="false" aria-label="Toggle navigation">
                    <span class="navbar-toggler-icon"></span>
                </button>
                <div class="navbar-collapse collapse d-sm-inline-flex justify-content-between">
                    <ul class="navbar-nav flex-grow-1">
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Index">Home</a>
                        </li>
                        <li class="nav-item">
                            <a class="nav-link text-dark" asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
                        </li>
                    </ul>
                </div>
            </div>
        </nav>
    </header>
    <div class="container">
        <main role="main" class="pb-3">
            @RenderBody()
        </main>
    </div>

    <footer class="border-top footer text-muted">
        <div class="container">
            &copy; 2020 - Movie App - <a asp-area="" asp-controller="Home" asp-action="Privacy">Privacy</a>
        </div>
    </footer>
    <script src="~/lib/jquery/dist/jquery.min.js"></script>
    <script src="~/lib/bootstrap/dist/js/bootstrap.bundle.min.js"></script>
    <script src="~/js/site.js" asp-append-version="true"></script>
    @await RenderSectionAsync("Scripts", required: false)
</body>
</html>
```

上述标记进行以下更改：

- `MvcMovie` 更改为 `Movie App` 三次。
- 定位点元素 `<a class="navbar-brand" asp-area="" asp-controller="Home" asp-action="Index">MvcMovie</a>` 更改为 `<a class="navbar-brand" asp-controller="Movies" asp-action="Index">Movie App</a>`。

在前面的标记中，省略了 `asp-area=""` [定位点标记帮助程序特性](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/built-in/anchor-tag-helper?view=aspnetcore-5.0)和特性值，因为此应用未使用[区域](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/areas?view=aspnetcore-5.0)。

**说明**：`Movies` 控制器尚未实现。 此时，`Movie App` 链接不起作用。

保存更改并选择“Privacy”链接。 请注意浏览器选项卡上的标题现在显示的是“Privacy 策略 - 电影应用”，而不是“Privacy 策略 - Mvc 电影” ：

![Privacy 选项卡](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-view/_static/privacy50.png?view=aspnetcore-5.0)

选择 Home 链接。

请注意，标题和定位点文本显示“电影应用”。 在布局模板中进行了一次更改，网站上的所有页面都反映新的链接文本和新标题。

检查 Views/_ViewStart.cshtml 文件：

```cshtml
@{
    Layout = "_Layout";
}
```

Views/_ViewStart.cshtml 文件将 Views/Shared/_Layout.cshtml 文件引入到每个视图中 。 可以使用 `Layout` 属性设置不同的布局视图，或将它设置为 `null`，这样将不会使用任何布局文件。

打开 Views/HelloWorld/Index.cshtml 视图文件。

更改标题和 `<h2>` 元素，如以下突出显示：

```cshtml
@{
    ViewData["Title"] = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>
```

标题和 `<h2>` 元素略有不同，因此可以清楚地看出代码的哪一部分更改了显示。

上述代码中的 `ViewData["Title"] = "Movie List";` 将 `ViewData` 字典的 `Title` 属性设置为“Movie List”。 `Title` 属性在布局页面中的 `<title>` HTML 元素中使用：

```cshtml
<title>@ViewData["Title"] - Movie App</title>
```

保存更改并导航到 `https://localhost:{PORT}/HelloWorld`。

请注意，以下内容已更改：

- 浏览器标题。
- 主标题。
- 二级标题。

如果浏览器中没有任何更改，则可能是正在查看的缓存内容。 在浏览器中按 Ctrl + F5 强制加载来自服务器的响应。 浏览器标题是使用我们在 Index.cshtml 视图模板中设置的 `ViewData["Title"]` 以及在布局文件中添加的额外“ - Movie App”创建的。

Index.cshtml 视图模板中的内容与 Views/Shared/_Layout.cshtml 视图模板合并 。 单个 HTML 响应将发送到浏览器。 凭借布局模板可以轻松地对应用中所有页面进行更改。 若要了解更多信息，请参阅[布局](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/layout?view=aspnetcore-5.0)。

![电影列表视图](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-view/_static/hell50.png?view=aspnetcore-5.0)

但是，“数据”的一小部分（即“Hello from our View Template!” 消息）是硬编码的。 MVC 应用程序有一个“V”（视图）和一个“C”（控制器），但还没有“M”（模型）。

#### 将数据从控制器传递给视图

控制器操作会被调用以响应传入的 URL 请求。 控制器类是编写处理传入浏览器请求的代码的地方。 控制器从数据源检索数据，并决定将哪些类型的响应发送回浏览器。 可以从控制器使用视图模板来生成并格式化对浏览器的 HTML 响应。

控制器负责提供所需的数据，使视图模板能够呈现响应。

视图模板不应：

- 执行业务逻辑
- 直接与数据库交互。

视图模板应仅使用由控制器提供给它的数据。 保持此“分离关注点”有助于保持代码：

- 干净。
- 可测试。
- 可维护。

目前，`HelloWorldController` 类中的 `Welcome` 方法采用 `name` 和 `ID` 参数，然后将值直接输出到浏览器。

应将控制器更改为使用视图模板，而不是使控制器将此响应呈现为字符串。 视图模板会生成动态响应，这意味着必须将适当的数据从控制器传递给视图以生成响应。 为此，可以让控制器将视图模板所需的动态数据（参数）放置在 `ViewData` 字典中。 然后，视图模板可以访问动态数据。

在 HelloWorldController.cs 中，更改 `Welcome` 方法以将 `Message` 和 `NumTimes` 值添加到 `ViewData` 字典。

`ViewData` 字典是动态对象，这意味着任何类型都可以使用。 在添加某些内容之前，`ViewData` 对象没有已定义的属性。 [MVC 模型绑定系统](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)自动将命名参数 `name` 和 `numTimes` 从查询字符串映射到方法中的参数。 完整的 `HelloWorldController`：

```csharp
using Microsoft.AspNetCore.Mvc;
using System.Text.Encodings.Web;

namespace MvcMovie.Controllers
{
    public class HelloWorldController : Controller
    {
        public IActionResult Index()
        {
            return View();
        }

        public IActionResult Welcome(string name, int numTimes = 1)
        {
            ViewData["Message"] = "Hello " + name;
            ViewData["NumTimes"] = numTimes;

            return View();
        }
    }
}
```

`ViewData` 字典对象包含将传递给视图的数据。

创建一个名为 Views/HelloWorld/Welcome.cshtml 的 Welcome 视图模板。

在 Welcome.cshtml 视图模板中创建一个循环，显示“Hello” `NumTimes`。 使用以下内容替换 Views/HelloWorld/Welcome.cshtml 的内容：

CSHTML复制

```cshtml
@{
    ViewData["Title"] = "Welcome";
}

<h2>Welcome</h2>

<ul>
    @for (int i = 0; i < (int)ViewData["NumTimes"]; i++)
    {
        <li>@ViewData["Message"]</li>
    }
</ul>
```

保存更改并浏览到以下 URL：

```
https://localhost:{PORT}/HelloWorld/Welcome?name=Rick&numtimes=4
```

数据取自 URL，并传递给使用 [MVC 模型绑定器](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)的控制器。 控制器将数据打包到 `ViewData` 字典中，并将该对象传递给视图。 然后，视图将数据作为 HTML 呈现给浏览器。

![Privacy 视图显示了 Welcome 标签以及四个“Hello Rick”短语](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-view/_static/rick2_50.png?view=aspnetcore-5.0)

在前面的示例中，我们使用 `ViewData` 字典将数据从控制器传递给视图。 稍后在本教程中，我们将使用视图模型将数据从控制器传递给视图。 传递数据的视图模型方法比 `ViewData` 字典方法更为优先。

在下一个教程中，将创建电影数据库。

### 将模型添加到 ASP.NET Core MVC 应用

在本部分中将添加用于管理数据库中的电影的类。 这些类将是 MVC 应用的“Model”部分 。

可以结合 [Entity Framework Core](https://docs.microsoft.com/zh-cn/ef/core) (EF Core) 使用这些类来处理数据库。 EF Core 是对象关系映射 (ORM) 框架，可以简化需要编写的数据访问代码。

要创建的模型类称为 POCO 类（源自“简单传统 CLR 对象”），因为它们与 EF Core 没有任何依赖关系 。 它们只定义将存储在数据库中的数据的属性。

在本教程中，首先要编写模型类，然后 EF Core 将创建数据库。

#### 添加数据模型类

右键单击 Models 文件夹，然后单击“添加” > “类” 。 将文件命名为 Movie.cs。

使用以下代码更新 Movie.cs 文件：

```csharp
using System;
using System.ComponentModel.DataAnnotations;

namespace MvcMovie.Models
{
    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string Genre { get; set; }
        public decimal Price { get; set; }
    }
}
```

`Movie` 类包含一个 `Id` 字段，数据需要该字段作为主键。

`ReleaseDate` 上的 [DataType](https://docs.microsoft.com/zh-cn/dotnet/api/system.componentmodel.dataannotations.datatype) 特性指定了数据的类型 (`Date`)。 通过此特性：

- 用户无需在数据字段中输入时间信息。
- 仅显示日期，而非时间信息。

[DataAnnotations](https://docs.microsoft.com/zh-cn/dotnet/api/system.componentmodel.dataannotations) 会在后续教程中介绍。

#### 添加NuGet包

从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”(PMC)。

![PMC 菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model/_static/pmc.png?view=aspnetcore-5.0)

在 PMC 中运行以下命令：

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

前面的命令添加 EF Core SQL Server 提供程序。 提供程序包将 EF Core 包作为依赖项进行安装。 在本教程后面的基架步骤中会自动安装其他包。

#### 创建数据库上下文类

要一个数据库上下文类来协调 `Movie` 模型的 EF Core 功能（创建、读取、更新和删除）。 数据库上下文派生自 [Microsoft.EntityFrameworkCore.DbContext](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.entityframeworkcore.dbcontext) 并指定要包含在数据模型中的实体。

创建一个“Data”文件夹。

使用以下代码添加 Data/MvcMovieContext.cs 文件：

```csharp
using Microsoft.EntityFrameworkCore;
using MvcMovie.Models;

namespace MvcMovie.Data
{
    public class MvcMovieContext : DbContext
    {
        public MvcMovieContext (DbContextOptions<MvcMovieContext> options)
            : base(options)
        {
        }

        public DbSet<Movie> Movie { get; set; }
    }
}
```

前面的代码为实体集创建 [DbSet](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.entityframeworkcore.dbset-1) 属性。 在实体框架术语中，实体集通常与数据表相对应。 实体对应表中的行。

#### 注册数据库上下文

ASP.NET Core 通过[依赖关系注入 (DI)](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0) 生成。 在应用程序启动过程中，必须向 DI 注册服务（如 EF Core DB 上下文）。 需要这些服务（如 Razor 页面）的组件通过构造函数参数提供相应服务。 本教程的后续部分介绍了用于获取 DB 上下文实例的构造函数代码。 本部分会将数据库上下文注册到 DI 容器。

将以下 `using` 语句添加到 Startup.cs 顶部：

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

将以下突出显示的代码添加到 `Startup.ConfigureServices`：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    services.AddDbContext<MvcMovieContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("MvcMovieContext")));
}
```

通过调用 [DbContextOptions](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 对象中的一个方法将连接字符串名称传递到上下文。 进行本地开发时，[ASP.NET Core 配置系统](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0)在 *appsettings.json* 文件中读取连接字符串。

#### 添加数据库连接字符串

将连接字符串添加到 *appsettings.json* 文件中：

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "MvcMovieContext": "Server=(localdb)\\mssqllocaldb;Database=MvcMovieContext-1;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
}
```

生成项目以检查编译器错误。

#### 基架电影页面

使用基架工具为电影模型生成“创建”、“读取”、“更新”和“删除”(CRUD) 页面。

在解决方案资源管理器中，右键单击“Controllers”文件夹 >“添加”>“新搭建基架的项目”。

![上述步骤的视图](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model/_static/add_controller21.png?view=aspnetcore-5.0)

在“添加基架”对话框中，选择“包含视图的 MVC 控制器(使用 Entity Framework)”>“添加” 。

![“添加基架”对话框](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model/_static/add_scaffold21.png?view=aspnetcore-5.0)

填写“添加控制器”对话框：

- 模型类：Movie (MvcMovie.Models)
- 数据上下文类：MvcMovieContext (MvcMovie.Data)

![“添加数据”上下文](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model/_static/dc5.png?view=aspnetcore-5.0)

- 视图：将每个选项保持为默认选中状态
- 控制器名称：保留默认的 MoviesController
- 选择“添加”

Visual Studio 将创建：

- 电影控制器 (Controllers/MoviesController.cs)
- “创建”、“删除”、“详细信息”、“编辑”和“索引”页面的 Razor 视图文件 (Views/Movies/*.cshtml)

自动创建这些文件称为“基架”。

你还不能使用基架页面，因为该数据库不存在。 如果运行应用并单击“Movie App”链接，则会出现“无法打开数据库”或“无此类表：Movie”错误消息。

#### 初始迁移

使用 EF Core [迁移](https://docs.microsoft.com/zh-cn/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-5.0)功能来创建数据库。 迁移是可用于创建和更新数据库以匹配数据模型的一组工具。

从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”(PMC)。

在 PMC 中，输入以下命令：

```powershell
Add-Migration InitialCreate
Update-Database
```

- `Add-Migration InitialCreate`：生成 Migrations/{timestamp}_InitialCreate.cs 迁移文件。 `InitialCreate` 参数是迁移名称。 可以使用任何名称，但是按照惯例，会选择可说明迁移的名称。 因为这是首次迁移，所以生成的类包含用于创建数据库架构的代码。 数据库架构基于在 `MvcMovieContext` 类中指定的模型。

- `Update-Database`：将数据库更新到上一个命令创建的最新迁移。 此命令在用于创建数据库的 Migrations/{time-stamp}_InitialCreate.cs 文件中运行 `Up` 方法。

  数据库更新命令生成以下警告：

  > No type was specified for the decimal column 'Price' on entity type 'Movie'. This will cause values to be silently truncated if they do not fit in the default precision and scale. Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.

  你可以忽略该警告，它将后面的教程中得到修复。

有关 EF Core 的 PMC 工具的详细信息，请参阅 [EF Core 工具引用 - Visual Studio 中的 PMC](https://docs.microsoft.com/zh-cn/ef/core/miscellaneous/cli/powershell)。

#### InitialCreate类

检查 Migrations/{timestamp}_InitialCreate.cs 迁移文件：

```csharp
public partial class InitialCreate : Migration
{
    protected override void Up(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.CreateTable(
            name: "Movie",
            columns: table => new
            {
                Id = table.Column<int>(nullable: false)
                    .Annotation("SqlServer:ValueGenerationStrategy", 
                                 SqlServerValueGenerationStrategy.IdentityColumn),
                Title = table.Column<string>(nullable: true),
                ReleaseDate = table.Column<DateTime>(nullable: false),
                Genre = table.Column<string>(nullable: true),
                Price = table.Column<decimal>(nullable: false)
            },
            constraints: table =>
            {
                table.PrimaryKey("PK_Movie", x => x.Id);
            });
    }

    protected override void Down(MigrationBuilder migrationBuilder)
    {
        migrationBuilder.DropTable(
            name: "Movie");
    }
}
```

`Up` 方法创建 Movie 表，并将 `Id` 配置为主键。 `Down` 方法可还原 `Up` 迁移所做的架构更改。

#### 测试应用

运行应用并单击“Movie App”链接。

如果遇到类似于以下情况的异常：

```
SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
```

可能缺失[迁移步骤](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model?view=aspnetcore-5.0&tabs=visual-studio#migration)。

- 测试“创建”页。 输入并提交数据。
- 测试“编辑”、“详细信息”和“删除”页 。

#### 控制器中的依赖项注入

打开 Controllers/MoviesController.cs 文件并检查构造函数：

```csharp
public class MoviesController : Controller
{
    private readonly MvcMovieContext _context;

    public MoviesController(MvcMovieContext context)
    {
        _context = context;
    }
```

构造函数使用[依赖关系注入](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0)将数据库上下文 (`MvcMovieContext`) 注入到控制器中。 数据库上下文将在控制器中的每个 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法中使用。

#### 强类型模型和 @model 关键词

在本教程之前的内容中，已经介绍了控制器如何使用 `ViewData` 字典将数据或对象传递给视图。 `ViewData` 字典是一个动态对象，提供了将信息传递给视图的方便的后期绑定方法。

MVC 还提供将强类型模型对象传递给视图的功能。 此强类型方法启用编译时代码检查。 基架机制通过 `MoviesController` 类和视图使用了此方法（即传递强类型模型）。

检查 Controllers/MoviesController.cs 文件中生成的 `Details` 方法：

```csharp
// GET: Movies/Details/5
public async Task<IActionResult> Details(int? id)
{
    if (id == null)
    {
        return NotFound();
    }

    var movie = await _context.Movie
        .FirstOrDefaultAsync(m => m.Id == id);
    if (movie == null)
    {
        return NotFound();
    }

    return View(movie);
}
```

`id` 参数通常作为路由数据传递。 例如 `https://localhost:5001/movies/details/1` 的设置如下：

- 控制器被设置为 `movies` 控制器（第一个 URL 段）。
- 操作被设置为 `details`（第二个 URL 段）。
- ID 被设置为 1（最后一个 URL 段）。

还可以使用查询字符串传入 `id`，如下所示：

```
https://localhost:5001/movies/details?id=1
```

在未提供 ID 值的情况下，`id` 参数可定义为[可以为 null 的类型](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/nullable-types/index) (`int?`)。

[Lambda 表达式](https://docs.microsoft.com/zh-cn/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)会被传入 `FirstOrDefaultAsync` 以选择与路由数据或查询字符串值相匹配的电影实体。

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

如果找到了电影，`Movie` 模型的实例则会被传递到 `Details` 视图：

```csharp
return View(movie);
```

检查 Views/Movies/Details.cshtml 文件的内容：

```cshtml
@model MvcMovie.Models.Movie

@{
    ViewData["Title"] = "Details";
}

<h1>Details</h1>

<div>
    <h4>Movie</h4>
    <hr />
    <dl class="row">
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Title)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Title)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.ReleaseDate)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.ReleaseDate)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Genre)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Genre)
        </dd>
        <dt class="col-sm-2">
            @Html.DisplayNameFor(model => model.Price)
        </dt>
        <dd class="col-sm-10">
            @Html.DisplayFor(model => model.Price)
        </dd>
    </dl>
</div>
<div>
    <a asp-action="Edit" asp-route-id="@Model.Id">Edit</a> |
    <a asp-action="Index">Back to List</a>
</div>
```

视图文件顶部的 `@model` 语句可指定视图期望的对象类型。 创建影片控制器时，将包含以下 `@model` 语句：

```cshtml
@model MvcMovie.Models.Movie
```

此 `@model` 指令允许访问控制器传递给视图的影片。 `Model` 对象为强类型对象。 例如，在 Details.cshtml 视图中，代码通过强类型的 `Model` 对象将每个电影字段传递给 `DisplayNameFor` 和 `DisplayFor`HTML 帮助程序。 `Create` 和 `Edit` 方法以及视图也传递一个 `Movie` 模型对象。

检查电影控制器中的 Index.cshtml 视图和 `Index` 方法。 请注意代码在调用 `View` 方法时是如何创建 `List` 对象的。 代码将此 `Movies` 列表从 `Index` 操作方法传递给视图：

```csharp
// GET: Movies
public async Task<IActionResult> Index()
{
    return View(await _context.Movie.ToListAsync());
}
```

创建影片控制器后，基架将以下 `@model` 语句包含在 Index.cshtml 文件的顶部：

```cshtml
@model IEnumerable<MvcMovie.Models.Movie>
```

`@model` 指令使你能够使用强类型的 `Model` 对象访问控制器传递给视图的电影列表。 例如，在 Index.cshtml 视图中，代码使用 `foreach` 语句通过强类型 `Model` 对象对电影进行循环遍历：

```cshtml
@model IEnumerable<MvcMovie.Models.Movie>

@{
    ViewData["Title"] = "Index";
}

<h1>Index</h1>

<p>
    <a asp-action="Create">Create New</a>
</p>
<table class="table">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Title)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.ReleaseDate)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Genre)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Price)
            </th>
            <th></th>
        </tr>
    </thead>
    <tbody>
@foreach (var item in Model) {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Title)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.ReleaseDate)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Genre)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Price)
            </td>
            <td>
                <a asp-action="Edit" asp-route-id="@item.Id">Edit</a> |
                <a asp-action="Details" asp-route-id="@item.Id">Details</a> |
                <a asp-action="Delete" asp-route-id="@item.Id">Delete</a>
            </td>
        </tr>
}
    </tbody>
</table>
```

因为 `Model` 对象为强类型（作为 `IEnumerable<Movie>` 对象），因此循环中的每个项都被类型化为 `Movie`。 除其他优点之外，这意味着可对代码进行编译时检查。

#### 其他资源

- [标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0)
- [全球化和本地化](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/localization?view=aspnetcore-5.0)

### 在 ASP.NET Core MVC 应用中使用数据库

`MvcMovieContext` 对象处理连接到数据库并将 `Movie` 对象映射到数据库记录的任务。 在 Startup.cs 文件的 `ConfigureServices` 方法中向[依赖关系注入](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-5.0)容器注册数据库上下文：

```C#
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllersWithViews();

    services.AddDbContext<MvcMovieContext>(options =>
    options.UseSqlServer(Configuration.GetConnectionString("MvcMovieContext")));
}
```

SP.NET Core [配置](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0)系统会读取 `ConnectionString`。 进行本地开发时，它从 *appsettings.json* 文件获取连接字符串：

```json
"ConnectionStrings": {
  "MvcMovieContext": "Server=(localdb)\\mssqllocaldb;Database=MvcMovieContext-2;Trusted_Connection=True;MultipleActiveResultSets=true"
}
```

当应用部署到测试服务器或生产服务器时，环境变量可用于将连接字符串设置为生产 SQL Server。 有关详细信息，请参阅[配置](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/configuration/?view=aspnetcore-5.0)。

#### SQL Server Express LocalDB

LocalDB 是轻型版的 SQL Server Express 数据库引擎，以程序开发为目标。 LocalDB 作为按需启动并在用户模式下运行的轻量级数据库没有复杂的配置。 默认情况下，LocalDB 数据库在 C:/Users/{user} 目录中创建 .mdf 文件 。

- 从“视图”菜单中，打开“SQL Server 对象资源管理器”(SSOX) 。

  ![“视图”菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/ssox.png?view=aspnetcore-5.0)

- 右键单击 `Movie` 表，然后单击“视图设计器”

  ![Movie 表上打开的上下文菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/design.png?view=aspnetcore-5.0)

  ![设计器中打开的 Movie 表](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/dv.png?view=aspnetcore-5.0)

请注意 `ID` 旁边的密钥图标。 默认情况下，EF 将名为 `ID` 的属性设置为主键。

- 右键单击 `Movie` 表，然后单击“查看数据”

  ![Movie 表上打开的上下文菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/ssox2.png?view=aspnetcore-5.0)

  ![显示表数据的打开的 Movie 表](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/vd22.png?view=aspnetcore-5.0)

#### 设定数据库种子

在 Models 文件夹中创建一个名为 `SeedData` 的新类。 将生成的代码替换为以下代码：

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.DependencyInjection;
using MvcMovie.Data;
using System;
using System.Linq;

namespace MvcMovie.Models
{
    public static class SeedData
    {
        public static void Initialize(IServiceProvider serviceProvider)
        {
            using (var context = new MvcMovieContext(
                serviceProvider.GetRequiredService<
                    DbContextOptions<MvcMovieContext>>()))
            {
                // Look for any movies.
                if (context.Movie.Any())
                {
                    return;   // DB has been seeded
                }

                context.Movie.AddRange(
                    new Movie
                    {
                        Title = "When Harry Met Sally",
                        ReleaseDate = DateTime.Parse("1989-2-12"),
                        Genre = "Romantic Comedy",
                        Price = 7.99M
                    },

                    new Movie
                    {
                        Title = "Ghostbusters ",
                        ReleaseDate = DateTime.Parse("1984-3-13"),
                        Genre = "Comedy",
                        Price = 8.99M
                    },

                    new Movie
                    {
                        Title = "Ghostbusters 2",
                        ReleaseDate = DateTime.Parse("1986-2-23"),
                        Genre = "Comedy",
                        Price = 9.99M
                    },

                    new Movie
                    {
                        Title = "Rio Bravo",
                        ReleaseDate = DateTime.Parse("1959-4-15"),
                        Genre = "Western",
                        Price = 3.99M
                    }
                );
                context.SaveChanges();
            }
        }
    }
}
```

如果 DB 中有任何电影，则会返回种子初始值设定项，并且不会添加任何电影。

```csharp
if (context.Movie.Any())
{
    return;   // DB has been seeded.
}
```

#### 添加种子初始值设定项

将 Program.cs 的内容替换为以下代码：

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;
using Microsoft.Extensions.Logging;
using MvcMovie.Data;
using MvcMovie.Models;
using System;

namespace MvcMovie
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = CreateHostBuilder(args).Build();

            using (var scope = host.Services.CreateScope())
            {
                var services = scope.ServiceProvider;

                try
                {
                    SeedData.Initialize(services);
                }
                catch (Exception ex)
                {
                    var logger = services.GetRequiredService<ILogger<Program>>();
                    logger.LogError(ex, "An error occurred seeding the DB.");
                }
            }

            host.Run();

        }

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseStartup<Startup>();
                });
    }
}
```

#### 测试应用

- 删除 DB 中的所有记录。 可以使用浏览器中的删除链接，也可从 SSOX 执行此操作。

- 强制应用初始化（调用 `Startup` 类中的方法），使种子方法能够正常运行。 若要强制进行初始化，必须先停止 IIS Express，然后再重新启动它。 可以使用以下任一方法来执行此操作：

  - 右键单击通知区域中的 IIS Express 系统任务栏图标，然后点击“退出”或“停止站点”

    ![IIS Express 系统任务栏图标](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/iisexicon.png?view=aspnetcore-5.0)

    ![上下文菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/stopiis.png?view=aspnetcore-5.0)

    - 如果是在非调试模式下运行 VS 的，请按 F5 以在调试模式下运行
    - 如果是在调试模式下运行 VS 的，请停止调试程序并按 F5

应用将显示设定为种子的数据。

![在 Microsoft Edge 中打开的显示电影数据的 MVC 电影应用程序](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/m55.png?view=aspnetcore-5.0)

### ASP.NET Core 中的控制器方法和视图

电影应用的开头不错，但展示效果不理想，例如，ReleaseDate 应为两个词。

![索引视图：Release Date 为一个词（没有空格），每个电影发行日期都显示时间中午 12 点](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/working-with-sql/_static/m55.png?view=aspnetcore-5.0)

打开 Models/Movie.cs 文件，并添加以下代码中突出显示的行：

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace MvcMovie.Models
{
    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [Display(Name = "Release Date")]
        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string Genre { get; set; }

        [Column(TypeName = "decimal(18, 2)")]
        public decimal Price { get; set; }
    }
}
```

下一教程将介绍 [DataAnnotations](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6)。 [Display](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.metadata.displaymetadata) 特性指定要显示的字段名称的内容（本例中应为“Release Date”，而不是“ReleaseDate”）。 [DataType](https://docs.microsoft.com/zh-cn/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) 属性指定数据的类型（日期），使字段中存储的时间信息不会显示。

要使 Entity Framework Core 能将 `Price` 正确地映射到数据库中的货币，则必须使用 `[Column(TypeName = "decimal(18, 2)")]` 数据注释。 有关详细信息，请参阅[数据类型](https://docs.microsoft.com/zh-cn/ef/core/modeling/relational/data-types)。

浏览到 `Movies` 控制器，并将鼠标指针悬停在“编辑”链接上以查看目标 URL。

![鼠标悬停在“编辑”链接上的浏览器窗口，显示了 https://localhost:5001/Movies/Edit/5 的链接 URL](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/controller-methods-views/_static/edit7.png?view=aspnetcore-5.0)

“编辑”、“详细信息”和“删除”链接是在 Views/Movies/Index.cshtml 文件中由 Core MVC 定位标记帮助程序生成的 。

```cshtml
        <a asp-action="Edit" asp-route-id="@item.ID">Edit</a> |
        <a asp-action="Details" asp-route-id="@item.ID">Details</a> |
        <a asp-action="Delete" asp-route-id="@item.ID">Delete</a>
    </td>
</tr>
```

[标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0)使服务器端代码可以在 Razor 文件中参与创建和呈现 HTML 元素。 在上面的代码中，`AnchorTagHelper` 从控制器操作方法和路由 ID 动态生成 HTML `href` 特性值。在最喜欢的浏览器中使用“查看源”，或使用开发人员工具来检查生成的标记。 生成的 HTML 的一部分如下所示：

```html
 <td>
    <a href="/Movies/Edit/4"> Edit </a> |
    <a href="/Movies/Details/4"> Details </a> |
    <a href="/Movies/Delete/4"> Delete </a>
</td>
```

重新调用在 Startup.cs 文件中设置的[路由](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/controllers/routing?view=aspnetcore-5.0)的格式：

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

ASP.NET Core 将 `https://localhost:5001/Movies/Edit/4` 转换为对 `Movies` 控制器的 `Edit` 操作方法的请求，参数 `Id` 为 4。 （控制器方法也称为操作方法。）

[标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0)是 ASP.NET Core 中最受欢迎的新功能之一。 有关详细信息，请参阅[其他资源](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/controller-methods-views?view=aspnetcore-5.0#additional-resources)。

打开 `Movies` 控制器并检查两个 `Edit` 操作方法。 以下代码显示了 `HTTP GET Edit` 方法，此方法将提取电影并填充由 Edit.cshtml Razor 文件生成的编辑表单。

```csharp
// GET: Movies/Edit/5
public async Task<IActionResult> Edit(int? id)
{
    if (id == null)
    {
        return NotFound();
    }

    var movie = await _context.Movie.FindAsync(id);
    if (movie == null)
    {
        return NotFound();
    }
    return View(movie);
}
```

以下代码显示 `HTTP POST Edit` 方法，它会处理已发布的电影值：

```csharp
// POST: Movies/Edit/5
// To protect from overposting attacks, please enable the specific properties you want to bind to, for 
// more details see http://go.microsoft.com/fwlink/?LinkId=317598.
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Edit(int id, [Bind("ID,Title,ReleaseDate,Genre,Price")] Movie movie)
{
    if (id != movie.ID)
    {
        return NotFound();
    }

    if (ModelState.IsValid)
    {
        try
        {
            _context.Update(movie);
            await _context.SaveChangesAsync();
        }
        catch (DbUpdateConcurrencyException)
        {
            if (!MovieExists(movie.ID))
            {
                return NotFound();
            }
            else
            {
                throw;
            }
        }
        return RedirectToAction("Index");
    }
    return View(movie);
}
```

`[Bind]` 特性是防止[过度发布](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost)的一种方法。 只应在 `[Bind]` 特性中包含想要更改的属性。 有关详细信息，请参阅[防止控制器过度发布](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)。 [ViewModels](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/) 提供了一种替代方法以防止过度发布。

请注意第二个 `Edit` 操作方法的前面是 `[HttpPost]` 特性。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Edit(int id, [Bind("ID,Title,ReleaseDate,Genre,Price")] Movie movie)
{
    if (id != movie.ID)
    {
        return NotFound();
    }

    if (ModelState.IsValid)
    {
        try
        {
            _context.Update(movie);
            await _context.SaveChangesAsync();
        }
        catch (DbUpdateConcurrencyException)
        {
            if (!MovieExists(movie.ID))
            {
                return NotFound();
            }
            else
            {
                throw;
            }
        }
        return RedirectToAction(nameof(Index));
    }
    return View(movie);
}
```

`HttpPost` 特性指定只能为 `POST` 请求调用此 `Edit` 方法。 可将 `[HttpGet]` 属性应用于第一个编辑方法，但不是必需，因为 `[HttpGet]` 是默认设置。

`ValidateAntiForgeryToken` 特性用于[防止请求伪造](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0)，并与编辑视图文件 (Views/Movies/Edit.cshtml) 中生成的防伪标记相配对。 编辑视图文件使用[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)生成防伪标记。

CSHTML复制

```cshtml
<form asp-action="Edit">
```

[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)会生成隐藏的防伪标记，此标记必须与电影控制器的 `Edit` 方法中 `[ValidateAntiForgeryToken]` 生成的防伪标记相匹配。 有关详细信息，请参阅 [防止跨站点请求伪造 (XSRF/CSRF) Core 中的 ASP.NET 攻击](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0)。

`HttpGet Edit` 方法采用电影 `ID` 参数，使用Entity Framework `FindAsync` 方法查找电影，并将所选电影返回到“编辑”视图。 如果无法找到电影，则返回 `NotFound` (HTTP 404)。

C#复制

```csharp
// GET: Movies/Edit/5
public async Task<IActionResult> Edit(int? id)
{
    if (id == null)
    {
        return NotFound();
    }

    var movie = await _context.Movie.FindAsync(id);
    if (movie == null)
    {
        return NotFound();
    }
    return View(movie);
}
```

当基架系统创建“编辑”视图时，它会检查 `Movie` 类并创建代码为类的每个属性呈现 `<label>` 和 `<input>` 元素。 以下示例显示由 Visual Studio 基架系统生成的“编辑”视图：

```cshtml
@model MvcMovie.Models.Movie

@{
    ViewData["Title"] = "Edit";
}

<h1>Edit</h1>

<h4>Movie</h4>
<hr />
<div class="row">
    <div class="col-md-4">
        <form asp-action="Edit">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <input type="hidden" asp-for="Id" />
            <div class="form-group">
                <label asp-for="Title" class="control-label"></label>
                <input asp-for="Title" class="form-control" />
                <span asp-validation-for="Title" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="ReleaseDate" class="control-label"></label>
                <input asp-for="ReleaseDate" class="form-control" />
                <span asp-validation-for="ReleaseDate" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Genre" class="control-label"></label>
                <input asp-for="Genre" class="form-control" />
                <span asp-validation-for="Genre" class="text-danger"></span>
            </div>
            <div class="form-group">
                <label asp-for="Price" class="control-label"></label>
                <input asp-for="Price" class="form-control" />
                <span asp-validation-for="Price" class="text-danger"></span>
            </div>
            <div class="form-group">
                <input type="submit" value="Save" class="btn btn-primary" />
            </div>
        </form>
    </div>
</div>

<div>
    <a asp-action="Index">Back to List</a>
</div>

@section Scripts {
    @{await Html.RenderPartialAsync("_ValidationScriptsPartial");}
}
```

请注意视图模板在文件顶端有一个 `@model MvcMovie.Models.Movie` 语句。 `@model MvcMovie.Models.Movie` 指定视图期望的视图模板的模型为 `Movie` 类型。

基架的代码使用几个标记帮助程序方法来简化 HTML 标记。 [标签标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)显示字段的名称（“Title”、“ReleaseDate”、“Genre”或“Price”）。 [输入标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)呈现 HTML `<input>` 元素。 [验证标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)显示与该属性相关联的任何验证消息。

运行应用程序并导航到 `/Movies` URL。 点击“编辑”链接。 在浏览器中查看页面的源。 为 `<form>` 元素生成的 HTML 如下所示。

HTML复制

```HTML
<form action="/Movies/Edit/7" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <hr />
        <div class="text-danger" />
        <input type="hidden" data-val="true" data-val-required="The ID field is required." id="ID" name="ID" value="7" />
        <div class="form-group">
            <label class="control-label col-md-2" for="Genre" />
            <div class="col-md-10">
                <input class="form-control" type="text" id="Genre" name="Genre" value="Western" />
                <span class="text-danger field-validation-valid" data-valmsg-for="Genre" data-valmsg-replace="true"></span>
            </div>
        </div>
        <div class="form-group">
            <label class="control-label col-md-2" for="Price" />
            <div class="col-md-10">
                <input class="form-control" type="text" data-val="true" data-val-number="The field Price must be a number." data-val-required="The Price field is required." id="Price" name="Price" value="3.99" />
                <span class="text-danger field-validation-valid" data-valmsg-for="Price" data-valmsg-replace="true"></span>
            </div>
        </div>
        <!-- Markup removed for brevity -->
        <div class="form-group">
            <div class="col-md-offset-2 col-md-10">
                <input type="submit" value="Save" class="btn btn-default" />
            </div>
        </div>
    </div>
    <input name="__RequestVerificationToken" type="hidden" value="CfDJ8Inyxgp63fRFqUePGvuI5jGZsloJu1L7X9le1gy7NCIlSduCRx9jDQClrV9pOTTmqUyXnJBXhmrjcUVDJyDUMm7-MF_9rK8aAZdRdlOri7FmKVkRe_2v5LIHGKFcTjPrWPYnc9AdSbomkiOSaTEg7RU" />
</form>
```

`<input>` 元素位于 `HTML <form>` 元素中，后者的 `action` 特性设置为发布到 `/Movies/Edit/id` URL。 当单击 `Save` 按钮时，表单数据将发布到服务器。 关闭 `</form>` 元素之前的最后一行显示[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)生成的隐藏的 [XSRF](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0) 标记。

#### 处理POST请求

以下列表显示了 `Edit` 操作方法的 `[HttpPost]` 版本。

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Edit(int id, [Bind("ID,Title,ReleaseDate,Genre,Price")] Movie movie)
{
    if (id != movie.ID)
    {
        return NotFound();
    }

    if (ModelState.IsValid)
    {
        try
        {
            _context.Update(movie);
            await _context.SaveChangesAsync();
        }
        catch (DbUpdateConcurrencyException)
        {
            if (!MovieExists(movie.ID))
            {
                return NotFound();
            }
            else
            {
                throw;
            }
        }
        return RedirectToAction(nameof(Index));
    }
    return View(movie);
}
```

`[ValidateAntiForgeryToken]` 特性验证[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)中的防伪标记生成器生成的隐藏的 [XSRF](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0) 标记

[模型绑定](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/model-binding?view=aspnetcore-5.0)系统采用发布的表单值，并创建一个作为 `movie` 参数传递的 `Movie` 对象。 `ModelState.IsValid` 属性验证表单中提交的数据是否可以用于修改（编辑或更新）`Movie` 对象。 如果数据有效，将保存此数据。 通过调用数据库上下文的 `SaveChangesAsync` 方法，将更新（编辑）的电影数据保存到数据库。 保存数据后，代码将用户重定向到 `MoviesController` 类的 `Index` 操作方法，此方法显示电影集合，包括刚才所做的更改。

在表单发布到服务器之前，客户端验证会检查字段上的任何验证规则。 如果有任何验证错误，则将显示错误消息，并且不会发布表单。 如果禁用 JavaScript，则不会进行客户端验证，但服务器将检测无效的发布值，并且表单值将与错误消息一起重新显示。 稍后在本教程中，我们将更详细地研究[模型验证](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/validation?view=aspnetcore-5.0)。 Views/Movies/Edit.cshtml 视图模板中的[验证标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)负责显示相应的错误消息。

![“编辑”视图：不正确的“价格”值 abc 的异常，说明“价格”字段必须是一个数字。 不正确的“发布日期”值 xyz 的异常，请输入有效的日期。](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/controller-methods-views/_static/val.png?view=aspnetcore-5.0)

电影控制器中的所有 `HttpGet` 方法都遵循类似的模式。 它们获取电影对象（对于 `Index`获取的是对象列表）并将对象（模型）传递给视图。 `Create` 方法将空的电影对象传递给 `Create` 视图。 在方法的 `[HttpPost]` 重载中，创建、编辑、删除或以其他方式修改数据的所有方法都执行此操作。 以 `HTTP GET` 方式修改数据是一种安全隐患。 以 `HTTP GET` 方法修改数据也违反了 HTTP 最佳做法和架构 [REST](http://rest.elkstein.org/) 模式，后者指定 GET 请求不应更改应用程序的状态。 换句话说，执行 GET 操作应是没有任何隐患的安全操作，也不会修改持久数据。

#### 其他资源

- [全球化和本地化](https://docs.microsoft.com/zh-cn/aspnet/core/fundamentals/localization?view=aspnetcore-5.0)
- [标记帮助程序简介](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0)
- [创作标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/authoring?view=aspnetcore-5.0)
- [防止跨站点请求伪造 (XSRF/CSRF) Core 中的 ASP.NET 攻击](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0)
- 防止控制器[过度发布](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application)
- [ViewModels](https://rachelappel.com/use-viewmodels-to-manage-data-amp-organize-code-in-asp-net-mvc-applications/)
- [表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)
- [输入标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)
- [标签标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)
- [选择标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)
- [验证标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)

### 将搜索添加到 ASP.NET Core MVC 应用

在本部分中，将向 `Index` 操作方法添加搜索功能，以实现按“类型”或“名称”搜索电影 。

使用以下代码更新 Controllers/MoviesController.cs 中的 `Index` 方法：

```C#
public async Task<IActionResult> Index(string searchString)
{
    var movies = from m in _context.Movie
                 select m;

    if (!String.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    return View(await movies.ToListAsync());
}
```

`Index` 操作方法的第一行创建了 [LINQ](https://docs.microsoft.com/zh-cn/dotnet/standard/using-linq) 查询用于选择电影：

```csharp
var movies = from m in _context.Movie
             select m;
```

此时仅对查询进行了定义，它还不会针对数据库运行。

如果 `searchString` 参数包含一个字符串，电影查询则会被修改为根据搜索字符串的值进行筛选：

C#复制

```csharp
if (!String.IsNullOrEmpty(searchString))
{
    movies = movies.Where(s => s.Title.Contains(searchString));
}
```

上面的 `s => s.Title.Contains()` 代码是 [Lambda 表达式](https://docs.microsoft.com/zh-cn/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)。 Lambda 在基于方法的 [LINQ](https://docs.microsoft.com/zh-cn/dotnet/standard/using-linq) 查询中用作标准查询运算符方法的参数，如 [Where](https://docs.microsoft.com/zh-cn/dotnet/api/system.linq.enumerable.where) 方法或 `Contains`（上述的代码中所使用的）。 在对 LINQ 查询进行定义或通过调用方法（如 `Where`、`Contains` 或 `OrderBy`）进行修改后，此查询不会被执行。 相反，会延迟执行查询。 这意味着表达式的计算会延迟，直到真正循环访问其实现的值或者调用 `ToListAsync` 方法为止。 有关延迟执行查询的详细信息，请参阅[Query Execution](https://docs.microsoft.com/zh-cn/dotnet/framework/data/adonet/ef/language-reference/query-execution)（查询执行）。

注意：[Contains](https://docs.microsoft.com/zh-cn/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 方法在数据库上运行，而不是在上面显示的 C# 代码中运行。 查询是否区分大小写取决于数据库和排序规则。 在 SQL Server 上，[Contains](https://docs.microsoft.com/zh-cn/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) 映射到 [SQL LIKE](https://docs.microsoft.com/zh-cn/sql/t-sql/language-elements/like-transact-sql)，这是不区分大小写的。 在 SQLite 中，由于使用了默认排序规则，因此需要区分大小写。

导航到 `/Movies/Index`。 将查询字符串（如 `?searchString=Ghost`）追加到 URL。 筛选的电影将显示出来。

![索引视图](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/ghost.png?view=aspnetcore-5.0)

如果将 `Index` 方法的签名更改为具有名称为 `id` 的参数，则 `id` 参数将匹配 Startup.cs 中设置的默认路由的可选 `{id}` 占位符。

```csharp
app.UseEndpoints(endpoints =>
{
    endpoints.MapControllerRoute(
        name: "default",
        pattern: "{controller=Home}/{action=Index}/{id?}");
});
```

将参数更改为 `id`，并将出现的所有 `searchString` 更改为 `id`。

之前的 `Index` 方法：

```csharp
public async Task<IActionResult> Index(string searchString)
{
    var movies = from m in _context.Movie
                 select m;

    if (!String.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    return View(await movies.ToListAsync());
}
```

更新后带 `id` 参数的 `Index` 方法：

```csharp
public async Task<IActionResult> Index(string id)
{
    var movies = from m in _context.Movie
                 select m;

    if (!String.IsNullOrEmpty(id))
    {
        movies = movies.Where(s => s.Title.Contains(id));
    }

    return View(await movies.ToListAsync());
}
```

现可将搜索标题作为路由数据（ URL 段）而非查询字符串值进行传递。

![索引视图，显示添加到 URL 的“ghost”一词以及返回的两部电影（Ghostbusters 和 Ghostbusters 2）的电影列表](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/g2.png?view=aspnetcore-5.0)

但是，不能指望用户在每次要搜索电影时都修改 URL。 因此需要添加 UI 元素来帮助他们筛选电影。 若已更改 `Index` 方法的签名，以测试如何传递绑定路由的 `ID` 参数，请改回原样，使其采用名为 `searchString` 的参数：

```csharp
public async Task<IActionResult> Index(string searchString)
{
    var movies = from m in _context.Movie
                 select m;

    if (!String.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    return View(await movies.ToListAsync());
}
```

打开“Views/Movies/Index.cshtml”文件，并添加以下突出显示的 `<form>` 标记：

```cshtml
    ViewData["Title"] = "Index";
}

<h2>Index</h2>

<p>
    <a asp-action="Create">Create New</a>
</p>

<form asp-controller="Movies" asp-action="Index">
    <p>
        Title: <input type="text" name="SearchString" />
        <input type="submit" value="Filter" />
    </p>
</form>

<table class="table">
    <thead>
```

此 HTML `<form>` 标记使用[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)，因此提交表单时，筛选器字符串会发布到电影控制器的 `Index` 操作。 保存更改，然后测试筛选器。

![显示标题筛选器文本框中键入了 ghost 一词的索引视图](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/filter.png?view=aspnetcore-5.0)

如你所料，不存在 `Index` 方法的 `[HttpPost]` 重载。 无需重载，因为该方法不更改应用的状态，仅筛选数据。

可添加以下 `[HttpPost] Index` 方法。

```csharp
[HttpPost]
public string Index(string searchString, bool notUsed)
{
    return "From [HttpPost]Index: filter on " + searchString;
}
```

`notUsed` 参数用于创建 `Index` 方法的重载。 本教程稍后将对此进行探讨。

如果添加此方法，则操作调用程序将与 `[HttpPost] Index` 方法匹配，且将运行 `[HttpPost] Index` 方法，如下图所示。

![显示“来自 HttpPost 索引: 筛选 ghost”应用程序响应的浏览器窗口](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/fo.png?view=aspnetcore-5.0)

但是，即使添加 `Index` 方法的 `[HttpPost]` 版本，其实现方式也受到限制。 假设你想要将特定搜索加入书签，或向朋友发送一个链接，让他们单击链接即可查看筛选出的相同电影列表。 请注意 HTTP POST 请求的 URL 与 GET 请求的 URL (localhost:{PORT}/Movies/Index) 相同，URL 中没有任何搜索信息。 搜索字符串信息作为[表单域值](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)发送给服务器。 可使用浏览器开发人员工具或出色的 [Fiddler 工具](https://www.telerik.com/fiddler)对其进行验证。 下图展示了 Chrome 浏览器开发人员工具：

![Microsoft Edge 中开发人员工具的“网络”选项卡，显示了 ghost 的 searchString 值的请求正文](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/f12_rb.png?view=aspnetcore-5.0)

在请求正文中，可看到搜索参数和 [XSRF](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0) 标记。 请注意，正如之前教程所述，[表单标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0) 会生成一个 [XSRF](https://docs.microsoft.com/zh-cn/aspnet/core/security/anti-request-forgery?view=aspnetcore-5.0) 防伪标记。 不会修改数据，因此无需验证控制器方法中的标记。

搜索参数位于请求正文而非 URL 中，因此无法捕获该搜索信息进行书签设定或与他人共享。 修复方法是指定：请求应为 Views/Movies/Index.cshtml 文件中的 `HTTP GET`。

CSHTML复制

```cshtml
@model IEnumerable<MvcMovie.Models.Movie>

@{
    ViewData["Title"] = "Index";
}

<h1>Index</h1>

<p>
    <a asp-action="Create">Create New</a>
</p>
<form asp-controller="Movies" asp-action="Index" method="get">
    <p>
        Title: <input type="text" name="SearchString" />
        <input type="submit" value="Filter" />
    </p>
</form>

<table class="table">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Title)
```

现在提交搜索后，URL 将包含搜索查询字符串。 即使具备 `HttpPost Index` 方法，搜索也将转到 `HttpGet Index` 操作方法。

![URL 中显示 searchString=ghost 且返回了 Ghostbusters 和 Ghostbusters 2 的浏览器窗口包含 ghost 一词](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/search_get.png?view=aspnetcore-5.0)

以下标记显示对 `form` 标记的更改：

```cshtml
<form asp-controller="Movies" asp-action="Index" method="get">
```

#### 添加按流派搜索

将以下 `MovieGenreViewModel` 类添加到“模型”文件夹：

```csharp
using Microsoft.AspNetCore.Mvc.Rendering;
using System.Collections.Generic;

namespace MvcMovie.Models
{
    public class MovieGenreViewModel
    {
        public List<Movie> Movies { get; set; }
        public SelectList Genres { get; set; }
        public string MovieGenre { get; set; }
        public string SearchString { get; set; }
    }
}
```

“电影流派”视图模型将包含：

- 电影列表。
- 包含流派列表的 `SelectList`。 这使用户能够从列表中选择一种流派。
- 包含所选流派的 `MovieGenre`。
- `SearchString`包含用户在搜索文本框中输入的文本。

将 `MoviesController.cs` 中的 `Index` 方法替换为以下代码：

```csharp
// GET: Movies
public async Task<IActionResult> Index(string movieGenre, string searchString)
{
    // Use LINQ to get list of genres.
    IQueryable<string> genreQuery = from m in _context.Movie
                                    orderby m.Genre
                                    select m.Genre;

    var movies = from m in _context.Movie
                 select m;

    if (!string.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    if (!string.IsNullOrEmpty(movieGenre))
    {
        movies = movies.Where(x => x.Genre == movieGenre);
    }

    var movieGenreVM = new MovieGenreViewModel
    {
        Genres = new SelectList(await genreQuery.Distinct().ToListAsync()),
        Movies = await movies.ToListAsync()
    };

    return View(movieGenreVM);
}
```

以下代码是一种 `LINQ` 查询，可从数据库中检索所有流派。

```csharp
// Use LINQ to get list of genres.
IQueryable<string> genreQuery = from m in _context.Movie
                                orderby m.Genre
                                select m.Genre;
```

通过投影不同的流派创建 `SelectList`（我们不希望选择列表中的流派重复）。

当用户搜索某个项目时，搜索值会保留在搜索框中。

#### 向“索引”视图添加“按流派搜索”

按如下所示更新 Views/Movies/ 中的 `Index.cshtml`：

```cshtml
@model MvcMovie.Models.MovieGenreViewModel

@{
    ViewData["Title"] = "Index";
}

<h1>Index</h1>

<p>
    <a asp-action="Create">Create New</a>
</p>
<form asp-controller="Movies" asp-action="Index" method="get">
    <p>

        <select asp-for="MovieGenre" asp-items="Model.Genres">
            <option value="">All</option>
        </select>

        Title: <input type="text" asp-for="SearchString" />
        <input type="submit" value="Filter" />
    </p>
</form>

<table class="table">
    <thead>
        <tr>
            <th>
                @Html.DisplayNameFor(model => model.Movies[0].Title)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movies[0].ReleaseDate)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movies[0].Genre)
            </th>
            <th>
                @Html.DisplayNameFor(model => model.Movies[0].Price)
            </th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model.Movies)
        {
            <tr>
                <td>
                    @Html.DisplayFor(modelItem => item.Title)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.ReleaseDate)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Genre)
                </td>
                <td>
                    @Html.DisplayFor(modelItem => item.Price)
                </td>
                <td>
                    <a asp-action="Edit" asp-route-id="@item.Id">Edit</a> |
                    <a asp-action="Details" asp-route-id="@item.Id">Details</a> |
                    <a asp-action="Delete" asp-route-id="@item.Id">Delete</a>
                </td>
            </tr>
        }
    </tbody>
</table>
```

检查以下 HTML 帮助程序中使用的 Lambda 表达式：

```
@Html.DisplayNameFor(model => model.Movies[0].Title)
```

在上述代码中，`DisplayNameFor` HTML 帮助程序检查 Lambda 表达式中引用的 `Title` 属性来确定显示名称。 由于只检查但未计算 Lambda 表达式，因此当 `model`、`model.Movies[0]` 或 `model.Movies` 为 `null` 或空时，你不会收到访问冲突。 对 Lambda 表达式求值时（例如，`@Html.DisplayFor(modelItem => item.Title)`），将求得该模型的属性值。

通过按流派搜索、按电影标题搜索以及按流派和电影标题搜索来测试应用：

![显示 https://localhost:5001/Movies?MovieGenre=Comedy&SearchString=2 结果的浏览器窗口](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/search/_static/s2.png?view=aspnetcore-5.0)

### 将新字段添加到 ASP.NET Core MVC 应用

在此部分中，[Entity Framework](https://docs.microsoft.com/zh-cn/ef/core/get-started/aspnetcore/new-db) Code First 迁移用于：

- 将新字段添加到模型。
- 将新字段迁移到数据库。

使用 EF Code First 自动创建数据库时，Code First 将：

- 将表添加到数据库，以跟踪数据库的架构。
- 验证数据库与生成它的模型类是否同步。 如果它们不同步，EF 则会引发异常。 这使查找不一致的数据库/代码问题变得更加轻松。

#### 向电影模型添加分级属性

将 `Rating` 属性添加到 Models/Movie.cs：

```csharp
using System;
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace MvcMovie.Models
{
    public class Movie
    {
        public int Id { get; set; }
        public string Title { get; set; }

        [Display(Name = "Release Date")]
        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string Genre { get; set; }

        [Column(TypeName = "decimal(18, 2)")]
        public decimal Price { get; set; }
        public string Rating { get; set; }
    }
}
```

生成应用`Ctrl+Shift+B`

因为已经添加新字段到 `Movie` 类，所以需要更新属性绑定列表，将此新属性纳入其中。 在 MoviesController.cs 中，更新 `Create` 和 `Edit` 操作方法的 `[Bind]` 属性，以包括 `Rating` 属性：

```C#
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
```

更新视图模板以在浏览器视图中显示、创建和编辑新的 `Rating` 属性。

编辑 /Views/Movies/Index.cshtml 文件并添加 `Rating` 字段：

```C#
<thead>
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Movies[0].Title)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Movies[0].ReleaseDate)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Movies[0].Genre)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Movies[0].Price)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Movies[0].Rating)
        </th>
        <th></th>
    </tr>
</thead>
<tbody>
    @foreach (var item in Model.Movies)
    {
        <tr>
            <td>
                @Html.DisplayFor(modelItem => item.Title)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.ReleaseDate)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Genre)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Price)
            </td>
            <td>
                @Html.DisplayFor(modelItem => item.Rating)
            </td>
            <td>
```

使用 `Rating` 字段更新 /Views/Movies/Create.cshtml。

可以复制/粘贴之前的“窗体组”，并让 intelliSense 帮助更新字段。 IntelliSense 适用于[标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/tag-helpers/intro?view=aspnetcore-5.0)。

![开发人员已在视图的第二个标签元素中键入字母 R 用作 asp-for 的特性值。 出现了 Intellisense 上下文菜单，其中显示了可用字段，包括在列表中自动突出显示的“Rating”。 开发人员单击此字段或在键盘上按 Enter 键时，此值将设置为“Rating”。](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/new-field/_static/cr.png?view=aspnetcore-5.0)

更新剩余模板。

更新 `SeedData` 类，使它提供新列的值。 示例更改如下所示，但可能需要对每个 `new Movie` 做出此更改。

```C#
new Movie
{
    Title = "When Harry Met Sally",
    ReleaseDate = DateTime.Parse("1989-1-11"),
    Genre = "Romantic Comedy",
    Rating = "R",
    Price = 7.99M
},
```

在 DB 更新为包括新字段之前，应用将不会正常工作。 如果它现在运行，将引发以下 `SqlException`：

```
SqlException: Invalid column name 'Rating'.
```

发生此错误是因为更新的 Movie 模型类与现有数据库的 Movie 表架构不同。 （数据库表中没有 `Rating` 列。）

可通过几种方法解决此错误：

1. 让 Entity Framework 自动丢弃，并基于新的模型类架构重新创建数据库。 在测试数据库上进行开发时，此方法在开发周期早期很方便；通过它可以一起快速改进模型和数据库架构。 但其缺点是会丢失数据库中的现有数据 - 因此请勿对生产数据库使用此方法！ 使用初始值设定项，以使用测试数据自动设定数据库种子，这通常是开发应用程序的有效方式。 对于早期开发和使用 SQLite 的情况，这是一个不错的方法。
2. 对现有数据库架构进行显式修改，使它与模型类相匹配。 此方法的优点是可以保留数据。 可以手动或通过创建数据库更改脚本进行此更改。
3. 使用 Code First 迁移更新数据库架构。

对于本教程，请使用 Code First 迁移。

从“工具”菜单中，选择“NuGet 包管理器”>“包管理器控制台”。

![PMC 菜单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/adding-model/_static/pmc.png?view=aspnetcore-5.0)

在 PMC 中，输入以下命令：

PowerShell复制

```powershell
Add-Migration Rating
Update-Database
```

`Add-Migration` 命令会通知迁移框架使用当前 `Movie` DB 架构检查当前 `Movie` 模型，并创建必要的代码，将 DB 迁移到新模型。

名称“Rating”是任意的，用于对迁移文件进行命名。 为迁移文件使用有意义的名称是有帮助的。

如果删除 DB 中的所有记录，初始化方法会设定 DB 种子，并将包括 `Rating` 字段。

### 将验证添加到ASP.NET Core MVC应用

本节内容：

- 向 `Movie` 模型添加了验证逻辑。
- 确保每当用户创建或编辑电影时，都会强制执行验证规则。

#### 坚持DRY原则

MVC 的设计原则之一是 [DRY](https://wikipedia.org/wiki/Don't_repeat_yourself)（“不要自我重复”）。 ASP.NET Core MVC 支持你仅指定一次功能或行为，然后使它应用到整个应用中。 这可以减少所需编写的代码量，并使编写的代码更少出错，更易于测试和维护。

MVC 和 Entity Framework Core Code First 提供的验证支持是 DRY 原则在实际操作中的极佳示例。 可以在一个位置（模型类中）以声明方式指定验证规则，并且在应用中的所有位置强制执行。

#### 将验证规则添加到电影模型

DataAnnotations 命名空间提供一组内置验证特性，可通过声明方式应用于类或属性。 DataAnnotations 还包含 `DataType` 等格式特性，有助于格式设置但不提供任何验证。

更新 `Movie` 类以使用内置的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 验证特性。

```csharp
public class Movie
{
    public int Id { get; set; }

    [StringLength(60, MinimumLength = 3)]
    [Required]
    public string Title { get; set; }

    [Display(Name = "Release Date")]
    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }

    [Range(1, 100)]
    [DataType(DataType.Currency)]
    [Column(TypeName = "decimal(18, 2)")]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z\s]*$")]
    [Required]
    [StringLength(30)]
    public string Genre { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z0-9""'\s-]*$")]
    [StringLength(5)]
    [Required]
    public string Rating { get; set; }
}
```

验证特性指定要对应用这些特性的模型属性强制执行的行为：

- `Required` 和 `MinimumLength` 特性表示属性必须有值；但用户可输入空格来满足此验证。
- `RegularExpression` 特性用于限制可输入的字符。 在上述代码中，即“Genre”（分类）：
  - 只能使用字母。
  - 第一个字母必须为大写。 允许使用空格，但不允许使用数字和特殊字符。
- `RegularExpression`“Rating”（分级）：
  - 要求第一个字符为大写字母。
  - 允许在后续空格中使用特殊字符和数字。 “PG-13”对“分级”有效，但对于“分类”无效。
- `Range` 特性将值限制在指定范围内。
- `StringLength` 特性使你能够设置字符串属性的最大长度，以及可选的最小长度。
- 从本质上来说，需要值类型（如 `decimal`、`int`、`float`、`DateTime`），但不需要 `[Required]` 特性。

让 ASP.NET Core 强制自动执行验证规则有助于提升你的应用的可靠性。 同时它能确保你无法忘记验证某些内容，并防止你无意中将错误数据导入数据库。

#### 验证错误UI

运行应用并导航到电影控制器。

点击“新建”连接添加新电影的链接。 使用无效值填写表单。 当 jQuery 客户端验证检测到错误时，会显示一条错误消息。

![带有多个 jQuery 客户端验证错误的电影视图表单](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/validation/_static/val.png?view=aspnetcore-5.0)

请注意表单如何自动呈现每个包含无效值的字段中相应的验证错误消息。 客户端（使用 JavaScript 和 jQuery）和服务器端（若用户禁用 JavaScript）都必定会遇到这些错误。

明显的好处在于不需要在 `MoviesController` 类或 Create.cshtml 视图中更改单个代码行来启用此验证 UI。 在本教程前面创建的控制器和视图会自动选取验证规则，这些规则是通过在 `Movie` 模型类的属性上使用验证特性所指定的。 使用 `Edit` 操作方法测试验证后，即已应用相同的验证。

存在客户端验证错误时，不会将表单数据发送到服务器。 可通过使用 [Fiddler 工具](https://www.telerik.com/fiddler)或 [F12 开发人员工具](https://docs.microsoft.com/zh-cn/microsoft-edge/devtools-guide)在 `HTTP Post` 方法中设置断点来对此进行验证。

#### 验证工作原理

你可能想知道在不对控制器或视图中的代码进行任何更新的情况下，验证 UI 是如何生成的。 下列代码显示两种 `Create` 方法。

```csharp
// GET: Movies/Create
public IActionResult Create()
{
    return View();
}

// POST: Movies/Create
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Create(
    [Bind("ID,Title,ReleaseDate,Genre,Price, Rating")] Movie movie)
{
    if (ModelState.IsValid)
    {
        _context.Add(movie);
        await _context.SaveChangesAsync();
        return RedirectToAction("Index");
    }
    return View(movie);
}
```

第一个 (HTTP GET) `Create` 操作方法显示初始的“创建”表单。 第二个 (`[HttpPost]`) 版本处理表单发布。 第二个 `Create` 方法（`[HttpPost]` 版本）调用 `ModelState.IsValid` 以检查电影是否有任何验证错误。 调用此方法将评估已应用于对象的任何验证特性。 如果对象有验证错误，则 `Create` 方法会重新显示此表单。 如果没有错误，此方法则将新电影保存在数据库中。 在我们的电影示例中，在检测到客户端上存在验证错误时，表单不会发布到服务器。当存在客户端验证错误时，第二个 `Create` 方法永远不会被调用。 如果在浏览器中禁用 JavaScript，客户端验证将被禁用，而你可以测试 HTTP POST `Create` 方法 `ModelState.IsValid` 检测任何验证错误。

可以在 `[HttpPost] Create` 方法中设置断点，并验证方法从未被调用，客户端验证在检测到存在验证错误时不会提交表单数据。 如果在浏览器中禁用 JavaScript，然后提交错误的表单，将触发断点。 在没有 JavaScript 的情况下仍然可以进行完整的验证。

以下图片显示如何在 FireFox 浏览器中禁用 JavaScript。

![Firefox：在“选项”的“内容”选项卡上，取消选中“启用 Javascript”复选框。](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/validation/_static/ff.png?view=aspnetcore-5.0)

以下图片显示如何在 Chrome 浏览器中禁用 JavaScript。

![Google Chrome：在“内容”设置的 Javascript 部分中，选择“不允许任何网站运行 JavaScript”。](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/validation/_static/chrome.png?view=aspnetcore-5.0)

禁用 JavaScript 后，发布无效数据并单步执行调试程序。

![在对无效数据进行调试时，ModelState.IsValid 上的 Intellisense 显示值为 false。](https://docs.microsoft.com/zh-cn/aspnet/core/tutorials/first-mvc-app/validation/_static/ms.png?view=aspnetcore-5.0)

Create.cshtml 视图模板的一部分在以下标记中显示：

```HTML
<h4>Movie</h4>
<hr />
<div class="row">
    <div class="col-md-4">
        <form asp-action="Create">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <div class="form-group">
                <label asp-for="Title" class="control-label"></label>
                <input asp-for="Title" class="form-control" />
                <span asp-validation-for="Title" class="text-danger"></span>
            </div>           
       
        @*Markup removed for brevity.*@
```

操作方法使用上述标记来显示初始表单，并在发生错误时重新显示此表单。

[输入标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0)使用 [DataAnnotations](https://docs.microsoft.com/zh-cn/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 特性，并在客户端上生成 jQuery 验证所需的 HTML 特性。 [验证标记帮助程序](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/views/working-with-forms?view=aspnetcore-5.0#the-validation-tag-helpers)用于显示验证错误。 有关详细信息，请参阅[验证](https://docs.microsoft.com/zh-cn/aspnet/core/mvc/models/validation?view=aspnetcore-5.0)。

此方法真正好的一点是：无论是控制器还是 `Create` 视图模板都不知道强制实施的实际验证规则或显示的特定错误消息。 仅可在 `Movie` 类中指定验证规则和错误字符串。 这些相同的验证规则自动应用于 `Edit` 视图和可能创建用于编辑模型的任何其他视图模板。

需要更改验证逻辑时，可以通过将验证特性添加到模型在同一个位置实现此操作。（在此示例中为 `Movie` 类）。 无需担心对应用程序的不同部分所强制执行规则的方式不一致 - 所有验证逻辑都将定义在一个位置并用于整个应用程序。 这使代码非常简洁，并且更易于维护和改进。 这意味着对 DRY 原则的完全遵守。

### 使用 DataType 特性

打开 Movie.cs 文件并检查 `Movie` 类。 除了一组内置的验证特性，`System.ComponentModel.DataAnnotations` 命名空间还提供格式特性。 我们已经在发布日期和价格字段中应用了 `DataType` 枚举值。 以下代码显示具有适当 `DataType` 特性的 `ReleaseDate` 和 `Price` 属性。

```csharp
[Display(Name = "Release Date")]
[DataType(DataType.Date)]
public DateTime ReleaseDate { get; set; }

[Range(1, 100)]
[DataType(DataType.Currency)]
public decimal Price { get; set; }
```

`DataType` 属性仅提供相关提示来帮助视图引擎设置数据格式（并提供元素/属性，例如向 URL 提供 `<a>` 和向电子邮件提供 `<a href="mailto:EmailAddress.com">`）。 可以使用 `RegularExpression` 特性验证数据的格式。 `DataType` 属性用于指定比数据库内部类型更具体的数据类型，它们不是验证属性。 在此示例中，我们只想跟踪日期，而不是时间。 `DataType` 枚举提供了多种数据类型，例如日期、时间、电话号码、货币、电子邮件地址等。 应用程序还可通过 `DataType` 特性自动提供类型特定的功能。 例如，可以为 `DataType.EmailAddress` 创建 `mailto:` 链接，并且可以在支持 HTML5 的浏览器中为 `DataType.Date` 提供日期选择器。 `DataType` 特性发出 HTML 5 `data-`（读作 data dash）特性供 HTML 5 浏览器理解。 `DataType` 特性不提供任何验证。

`DataType.Date` 不指定显示日期的格式。 默认情况下，数据字段根据基于服务器的 `CultureInfo` 的默认格式进行显示。

`DisplayFormat` 特性用于显式指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` 设置指定在文本框中显示值以进行编辑时也应用格式。 （你可能不想为某些字段执行此操作 — 例如对于货币值，你可能不希望文本框中的货币符号可编辑。）

可以单独使用 `DisplayFormat` 特性，但通常建议使用 `DataType` 特性。 `DataType` 特性传达数据的语义而不是传达如何在屏幕上呈现数据，并提供 DisplayFormat 不具备的以下优势：

- 浏览器可启用 HTML5 功能（例如显示日历控件、区域设置适用的货币符号、电子邮件链接等）
- 默认情况下，浏览器将根据区域设置采用正确的格式呈现数据。
- `DataType` 特性使 MVC 能够选择正确的字段模板来呈现数据（如果 `DisplayFormat` 由自身使用，则使用的是字符串模板）。

需要禁用 jQuery 日期验证才能使用具有 `DateTime` 的 `Range` 特性。 通常，在模型中编译固定日期是不恰当的，因此不推荐使用 `Range` 特性和 `DateTime`。

以下代码显示组合在一行上的特性：

```csharp
public class Movie
{
    public int Id { get; set; }

    [StringLength(60, MinimumLength = 3)]
    public string Title { get; set; }

    [Display(Name = "Release Date"), DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z\s]*$"), Required, StringLength(30)]
    public string Genre { get; set; }

    [Range(1, 100), DataType(DataType.Currency)]
    [Column(TypeName = "decimal(18, 2)")]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z0-9""'\s-]*$"), StringLength(5)]
    public string Rating { get; set; }
}
```

在本系列的下一部分中，我们将回顾应用，并对自动生成的 `Details` 和 `Delete` 方法进行一些改进。

