Monitor
===============

Monitor分为服务及客户端调用监控两个部分，可用于获取并记录调用次数、执行时间等信息。

Monitor的入口是IMonitorFactory，用于创建IMonitor。

::

	public interface IMonitorFactory
	{
		IMonitor CreateMonitor(AppHost appHost, RpcConfig config);
	}

IMonitor有两个函数GetServiceSession、GetClientSession。

客户端调用服务方法时会从GetClientSession获取IClientMonitorSession，并在相应时间点调用其方法。

服务收到请求时会从GetServiceSession获取IServiceMonitorSession，并在相应时间点调用其方法。

::

	public interface IMonitor : IDisposable
	{
		IServiceMonitorSession GetServiceSession(ServiceContext context);

		IClientMonitorSession GetClientSession();
	}

.. toctree::
	:titlesonly:

	client-monitor
	service-monitor
	customize
