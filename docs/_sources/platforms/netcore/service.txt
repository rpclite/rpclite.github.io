创建服务端
=========================================================

.. tip::
  .Net Core中现只支持Host到ASP.NET Core中。
  .Net Core中创建服务除初始化不同其它步骤基本相同


创建服务包括以下步骤：

* 创建Web Application工程
* 添加RpcLite引用
* 创建服务契约接口
* 通过实现服务契约接口创建服务类
* 在Startup.cs中初始化RpcLite

创建Web Application工程
-------------------------

* 打开Visual Studio 2015
* 打开菜单File ‣ New ‣ Project...
* 在左边的菜单中选择 Templates ‣ Visual C# ‣ .Net Core
* 确保目标Framework版本为 .NET Framework 4.5 或更高
* 在项目类型中选择 ASP.NET Core Web Application (.Net Core)
* 填写项目名称HelloRpcLiteServiceCore点Ok
* 在模板类型选择窗口选择Empty点Ok


添加RpcLite引用
-------------------------

.. tip::
  添加引用有两种方式：直接下载dll然后引用、通过NuGet添加，其中通过NuGet添加简单方便，本文以此方式为例。
  通过NuGet添加也有两种方式：图形界面或命令行

*命令行*

* 打开菜单Tools ‣ NuGet Package Manager ‣ Package Manager Console
* 运行 Install-Package RpcLite

*图形界面*

* 在Solution Explorer中右击HelloRpcLite，选择Manage NuGet Packages...
* 在NuGet页面中选择Browse Tab页，然后搜索RpcLite
* 在搜索结果中安装RpcLite

创建服务契约接口
-----------------------------

* 新建类文件IProductService.cs
* 输入以下内容

.. includesamplefile:: HelloRpcLite/src/HelloRpcLiteServiceCore/IProductService.cs
        :language: c#
        :linenos:

通过实现服务契约接口创建服务类
-----------------------------------------

* 新建类文件ProductService.cs
* 输入以下内容

.. includesamplefile:: HelloRpcLite/src/HelloRpcLiteServiceCore/ProductService.cs
        :language: c#
        :linenos:

在Startup.cs中初始化RpcLite
-----------------------------

向Configure函数中添加初始化代码

::

	app.UseRpcLite(builder => builder
			.UseService<ProductService>("ProductService", "api/service/")
			.UseServicePaths("api/")
	);
	
如果WebApplication中未用到MVC(Routing组件)还需要在ConfigureServices中添加对Routing组件的引用
::

	services.AddRouting();

完整代码如下

.. includesamplefile:: HelloRpcLite/src/HelloRpcLiteServiceCore/Startup.cs
        :language: c#
        :linenos:

*说明*

* UseService<ProductService>("ProductService", "api/service/")是添加一个服务
  泛型参数ProductService为服务提供类,
  第一个参数"ProductService"为服务名
  "api/service/"为服务相对于当前WebApplication根的地址，例如WebApplication地址为http://localhost:8080则服务地址为http://localhost:8080/api/service/。若服务部署到虚拟目录下如http://localhost:8080/app1则服务地址为http://localhost:8080/app1/api/service/
* UseServicePaths("api/")指定服务地址的前缀，以此地址开始的所有Url都会被认为是RpcLite服务，UseService中使用的路必需在ServicePaths中。若没有配置此选项则不能正常访问服务。

运行
-----------------------------

| 到此一个RpcLite服务已经创建完成，可运行查看结果。

* F5运行WebApplication，在浏览器中查看地址，假设是http://localhost:5000
* 在浏览器访问http://localhost:5000/api/service/GetDateTimeString，可看到返回的内容是当前日期
* 在浏览器访问http://localhost:5000/api/service/可以看到当前服务的信息，服务名及所有接口名

::

  Service Name: ProductService
  Actions:
  String GetDateTimeString();
