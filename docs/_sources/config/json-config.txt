Json配置
===============

Json配置文件相对RpcConfigBuilder要复杂，详细说明稍后补齐。

json配置文件使用示例（.Net Core）::

	var configBuilder = new ConfigurationBuilder();
	configBuilder
		.AddJsonFile("rpclite.config.json");
	var config = configBuilder.Build();
	var rpcConfig = RpcConfigHelper.GetConfig(new CoreConfigurationSection(config));

rpclite.config.json示例

::

	{
	  "version": "1.1",
	  "appId": "10000",

	  "registry": {
		"address": "http://localhost:12974/api/service/",
		"type": "RpcLite.Registry.Merops.MeropsRegistryFactory, RpcLite.Registry.Merops"
	  },

	  "monitor": {
		"address": "http://localhost:12974/api/service/",
		"type": "RpcLite.Monitor.Merops.MeropsMonitorFactory, RpcLite.Monitor.Merops"
	  },

	  "formatter": {
		"removeDefault": false,
		"formatters": [
		  { "type": "RpcLite.Formatters.JsonFormatter, RpcLite" }
		]
	  },

	  "service": {
		"mapper": {
		  "name": "ServiceMapper",
		  "type": "RpcLite.Service.DefaultServiceMapperFactory, RpcLite"
		},
		"services": [
		  {
			"name": "ProductService",
			"path": "/api/service/",
			"type": "ServiceTest.ServiceImpl.ProductService, ServiceTest.ServiceImpl"
			//"address": "http://192.168.9.1:5000/api/service/"
		  }
		]
	  },

	  "client": {
		"channel": {
		  "providers": [
			{ "type": "RpcLite.Client.DefaultChannelProvider, RpcLite" }
		  ]
		},
		"invoker": {
		  "type": "RpcLite.Client.DefaultInvokerFactory, RpcLite"
		},
		"clients": [
		  {
			"name": "ProductService",
			"type": "ServiceTest.Contract.IProductService, ServiceTest.Contract",
			"group": "IT",
			"address": "http://localhost:5000/api/service/"
		  }
		]
	  }

	}

