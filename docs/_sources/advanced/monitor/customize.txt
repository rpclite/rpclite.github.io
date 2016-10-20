自定义Monitor
===============

自定义Monitor只需要实现IMonitorFactory、IMonitor、IClientMonitorSession、IServiceMonitorSession 4个接口，IClientMonitorSession、IServiceMonitorSession这两个接口可以根据需要只实现其中一个，如果不需要监控只需在IMonitor返回null即可不能抛异常。

IFormatter.Name是用于在客户端选择使用哪个已添加的Formatter，在服务中不会使用到。

客户端调用服务时会传把当前Formatter.SupportMimes中的第一个元素传到服务端，服务端会在Formatter中找到第一个支持这个mime的Formatter用于反序列化参数及序列化返回结果。

下面的示例实现了在相应时间点把信息输出到控制台的一个Monitor
::

	class TestConsoleMonitorFactory : IMonitorFactory
	{
		public IMonitor CreateMonitor(AppHost appHost, RpcConfig config)
		{
			return new TestConsoleMonitor();
		}
	}


	class TestConsoleMonitor : IMonitor
	{
		public void Dispose()
		{
		}

		public IServiceMonitorSession GetServiceSession(ServiceContext context)
		{
			return new TestConsoleServiceSession();
		}

		public IClientMonitorSession GetClientSession()
		{
			return new TestConsoleClientSession();
		}
	}

	class TestConsoleServiceSession : IServiceMonitorSession
	{
		public void BeginRequest(ServiceContext context)
		{
			Console.WriteLine("BeginRequest at " + DateTime.Now);
		}

		public void EndRequest(ServiceContext context)
		{
			Console.WriteLine("BeginRequest at " + DateTime.Now);
		}
	}

	class TestConsoleClientSession : IClientMonitorSession
	{
		public void OnInvoking(ClientContext request)
		{
			Console.WriteLine("OnInvoking at " + DateTime.Now);
		}

		public void OnInvoked(ClientContext request)
		{
			Console.WriteLine("OnInvoked at " + DateTime.Now);
		}

		public void OnSerializing(ClientContext request)
		{
			Console.WriteLine("OnSerializing at " + DateTime.Now);
		}

		public void OnSerialized(ClientContext request)
		{
			Console.WriteLine("OnSerialized at " + DateTime.Now);
		}
	}