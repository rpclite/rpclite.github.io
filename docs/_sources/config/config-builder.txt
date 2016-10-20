RpcConfigBuilder
===============

创建RpcConfigBuilder对象后可以使用UseXXX方法添加相关的配置项，最后通过Build方法生成RpcConfig对象。RpcConfig对象作为的参数创建AppHost。

示例如下：
::

	var config = new RpcConfigBuilder()
		.UseAppId("10000")
		.UseRegistry<MeropsRegistryFactory>("MeropsRegistry", "http://localhost:12974/api/service/")
		.UseMonitor<ConsoleMonitorFactory>("ConsoleMonitor", "http://localhost:6201/api/service/")
		.UseInvoker<DefaultInvokerFactory>(null)
		.UseFilter<UnitTestFilterFactory>()
		.Build();
	var appHost = new AppHost(config);
	appHost.Initialize();
	
AppHostBuilder
-----------------------------
创建AppHost还可以通过AppHostBuilder，AppHostBuilder实际是对RpcConfigBuilder的简单包装。

使用方法
::

	var path = "/service/";
	var appHost = new AppHostBuilder()
		.UseAppId("10000")
		.UseRegistry<MeropsRegistryFactory>("MeropsRegistry", "http://localhost:12974/api/service/")
		.UseMonitor<ConsoleMonitorFactory>("ConsoleMonitor", "http://localhost:6201/api/service/")
		.UseService<ProductService>("ProductService", path, null)
		.UseInvoker<DefaultInvokerFactory>(null)
		.UseFilter<UnitTestFilterFactory>()
		.Build();
	appHost.Initialize();
