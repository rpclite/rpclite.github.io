Registry
===============

Registry用于向治理系统注册服务地址及从治理系统获取服务提供者的地址及扩展信息。

默认Registry为RpcLite.Registry.DefaultRegistry，只提供从配置中读取服务地址的功能，不能从治理系统读取服务提供者地址。

::

	var appHost = new AppHostBuilder()
		.UseAppId("10000")
		.UseRegistry<MeropsRegistryFactory>("MeropsRegistry", "http://localhost:12974/api/service/")
		//其它配置
		.Build();

.. toctree::
	:titlesonly:

	use-registry
	customize
	merops
