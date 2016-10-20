使用Registry
===============

通过Fluent API创建AppHost时调用UseRegistry来使用Registry，方法定义如下：

::

	public AppHostBuilder UseRegistry<TFactory>(string name, string address)

参数说明：

* TFactory：实现IRegistryFactory的类，用于创建Registry对象
* name：友好显示名
* address：Registry服务地址

使用示例
::

	var appHost = new AppHostBuilder()
		.UseAppId("10000")
		.UseRegistry<MeropsRegistryFactory>("MeropsRegistry", "http://localhost:12974/api/service/")
		//其它配置
		.Build();
	appHost.Initialize();
