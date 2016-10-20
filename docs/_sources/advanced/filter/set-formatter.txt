设置Formatter
===============

客户端
````````````````````````````````
对于客户端设置Formatter有两种方式。

RpcClient.Formatter
--------------------------------
类型为IFormatter，直接设置为IFormatter对象，如果是第三方Formatter不需要在配置中添加。
::

	var client = ClientFactory.GetInstance<IProductService>();
	var clientInfo = (IRpcClient<IProductService>)client;
	clientInfo.Formatter = new XmlFormatter();

RpcClient.Format
--------------------------------
字符串，设置后会在已添加IFormatter中找到名称对应的IFormatter并设置RpcClient.Formatter属性。
::

	var client = ClientFactory.GetInstance<IProductService>();
	var clientInfo = (IRpcClient<IProductService>)client;
	clientInfo.Format = "xml";

服务
````````````````````````````````
服务要支持某种Formatter需要在创建AppHost时添加到配置中。对过Fuent API方法如下：

UseFormatter<TFormatter>(string name); 

这里的name只作显示的友好名称，Formatter名称是放在IFormatter.Name属性中，建议都使用小写。

示例
::

	var appHost = new AppHostBuilder();
	appHost.UseFormatter<XmlFormatter>("XmlFormatter");




