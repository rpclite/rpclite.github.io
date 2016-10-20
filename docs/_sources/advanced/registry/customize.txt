自定义Registry
===============

自定义Registry需要实现IRegistry和IRegistryFactory接口，IRegistryFactory用于创建IRegistry，IRegistryFactory需要有一个public的无参构造函数。

::

	public class MeropsRegistry : IRegistry
	{
		//......
	}

	public class MeropsRegistryFactory : IRegistryFactory
	{
		public IRegistry CreateRegistry(AppHost appHost, RpcConfig config)
		{
			return new MeropsRegistry(appHost, config);
		}

		public void Dispose()
		{
		}
	}
