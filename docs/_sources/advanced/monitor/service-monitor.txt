ServiceMonitor
===============

服务端收到调用请求时会从IMonitor.GetServerSession获取IServerMonitorSession，并在相应时间点调用其方法，顺序为：

* BeginRequest，开始处理请求
* EndRequest，请求处理已完成

IServerMonitorSession接口定义如下::

	public interface IServiceMonitorSession
	{
		void BeginRequest(ServiceContext context);

		void EndRequest(ServiceContext context);
	}
