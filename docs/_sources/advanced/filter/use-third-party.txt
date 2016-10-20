使用第三方Formatter
==============================

使用第三方Formatter和前一小节一样有两种方式。第一种方式直接设置RpcClient.Formatter。

第二种方式是需要在创建AppHost时添加到配置中，同上一小节。

.. tip::

	添加的第三方Formatter名称可以重复但生效的只有最后添加的Formatter

::

	var appHost = new AppHostBuilder();
	appHost.UseFormatter<XmlFormatter>("XmlFormatter");
