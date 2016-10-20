创建服务端
=========================================================

.. tip::
  Full .Net Framework中现只支持Host到ASP.NET中。


*创建服务包括以下步骤：*

* 创建Web Application工程
* 添加RpcLite引用
* Web.config添加RpcHttpModule
* 创建服务契约接口
* 通过实现服务契约接口创建服务类
* 在Global.asax中初始化RpcLite

创建Web Application工程
-------------------------

* 打开Visual Studio 2015
* 打开菜单File ‣ New ‣ Project...
* 在左边的菜单中选择 Templates ‣ Visual C# ‣ Web
* 在项目类型中选择 ASP.NET Web Application (.Net Framework)
* 确保目标Framework版本为 .NET Framework 4.0 或更高
* 填写项目名称HelloRpcLiteService点Ok


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

Web.config添加RpcHttpModule
-----------------------------

| 默认情况下添加RpcLite包依赖后Web.Config会自动添加，若已自动添加请忽略本小节的操作
| 在集成管道模式和经典管道模式中添加HttpModule方式不同，本文以现在用得最多的集成管道模式说明

在configuration/system.webServer节点下添加
<add name="RpcLite" type="RpcLite.Service.RpcHttpModule, RpcLite.NetFx" />

完整配置如下::

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
    	<system.webServer>
    		<modules>
    			<add name="RpcLite" type="RpcLite.Service.RpcHttpModule, RpcLite.NetFx" />
    		</modules>
    	</system.webServer>
    </configuration>

创建服务契约接口
-----------------------------

* 新建类文件IProductService.cs
* 输入以下内容

::

	namespace HelloRpcLiteService
	{
		public interface IProductService
		{
			string GetDateTimeString();
		}
	}


通过实现服务契约接口创建服务类
-----------------------------------------

* 新建类文件ProductService.cs
* 输入以下内容

::

	using System;

	namespace HelloRpcLiteService
	{
		public class ProductService : IProductService
		{
			public string GetDateTimeString()
			{
				return DateTime.Now.ToString();
			}
		}
	}

在Global.asax中初始化RpcLite
-----------------------------

* 向工程中添加Global.asax
* 在Application_Start函数中添加初始化代码

::

	using System;
	using RpcLite.AspNet;

	namespace HelloRpcLiteService
	{
		public class Global : System.Web.HttpApplication
		{
			protected void Application_Start(object sender, EventArgs e)
			{
				RpcInitializer.Initialize(builder => builder
					.UseService<ProductService>("ProductService", "api/service/")
					.UseServicePaths("api/")
					);
			}
		}
	}


*说明*

* UseService<ProductService>("ProductService", "api/service/")是添加一个服务
  泛型参数ProductService为服务提供类,
  第一个参数"ProductService"为服务名
  "api/service/"为服务相对于当前WebApplication根的地址，例如WebApplication地址为http://localhost:8080则服务地址为http://localhost:8080/api/service/。若服务部署到虚拟目录下如http://localhost:8080/app1则服务地址为http://localhost:8080/app1/api/service/
* UseServicePaths("api/")指定服务地址的前缀，以此地址开始的所有Url都会被认为是RpcLite服务，UseService中使用的路必需在ServicePaths中。若没有配置此选项则不能正常访问服务。

运行
-----------------------------

| 到此一个RpcLite服务已经创建完成，可运行查看结果。

* F5运行WebApplication，在浏览器中查看地址，假设是http://localhost:11651
* 在浏览器访问http://localhost:11651/api/service/GetDateTimeString，可看到返回的内容是当前日期
* 在浏览器访问http://localhost:11651/api/service/可以看到当前服务的信息，服务名及所有接口名

::

  Service Name: ProductService
  Actions:
  String GetDateTimeString();
