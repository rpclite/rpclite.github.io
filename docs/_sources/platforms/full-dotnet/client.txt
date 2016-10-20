创建客户端
=========================================================

前面已经创建好了一个服务，现在我们创建客户端来访问些服务

创建客户端包括以下步骤：
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

* 创建Console工程
* 添加RpcLite引用
* 创建服务契约接口
* 初始化RpcLite
* 创建RpcClient
* 调用服务方法

创建Console Application工程
--------------------------------

* 打开Visual Studio 2015
* 打开菜单File ‣ New ‣ Project...
* 在左边的菜单中选择 Templates ‣ Visual C# ‣ Windows
* 在项目类型中选择 Console Application
* 确保目标Framework版本为 .NET Framework 4.0 或更高
* 填写项目名称HelloRpcLiteClient点Ok

添加RpcLite引用
--------------------------

* 在Solution Explorer中右击HelloRpcLite，选择Manage NuGet Packages...
* 在NuGet页面中选择Browse Tab页，然后搜索RpcLite
* 在搜索结果中安装RpcLite

创建服务契约接口
--------------------------

| 直接复制服务中的文件即可，或添加Link引用。
| 在实际开发时契约接口可放到单独的Project中，Service及Client端引用此Project即可。
| 为了简单在此还是创建一个新的文件

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

.. tip::
  契约接口不要求Service与Client端完全相同，Client端只需定义需要调用的函数即可，名必须与Service端接口中一致。

初始化RpcLite
--------------------------

* 向工程中添加Global.asax
* 在Application_Start函数中添加初始化代码

::

	RpcInitializer.Initialize(builder => builder
			.UseClient<IProductService>()
	);

*说明*
> UseClient<IProductService>()是添加一个服务引用泛型参数IProductService为服务契约接口
> UseClient也有带服务名，及服务地址的重载

创建RpcClient
--------------------------

::

	var serviceAddress = "http://localhost:11651/api/service/";
	var client = ClientFactory.GetInstance<IProductService>(serviceAddress);


调用服务方法
--------------------------

::

    var dateTimeString = client.GetDateTimeString();

*完整代码如下*

::

	using System;
	using RpcLite.AspNet;
	using RpcLite.Client;

	namespace HelloRpcLiteClient
	{
		class Program
		{
			static void Main(string[] args)
			{
				RpcInitializer.Initialize(builder => builder
						.UseClient<IProductService>()
				);

				var serviceAddress = "http://localhost:11651/api/service/";
				var client = ClientFactory.GetInstance<IProductService>(serviceAddress);

				try
				{
					var dateTimeString = client.GetDateTimeString();
					Console.WriteLine("DateTime now from service is " + dateTimeString);
				}
				catch (Exception ex)
				{
					Console.WriteLine(ex);
				}
				Console.ReadLine();
			}
		}
	}

* F5运行HelloRpcLiteService
* 在Solution Explorer中选中HelloRpcLiteClient右击，选择菜单Debug ‣ Start new instance
* Console窗口中可看到运行结果

::

    DateTime now from service is 2016-09-28 00:04:20
