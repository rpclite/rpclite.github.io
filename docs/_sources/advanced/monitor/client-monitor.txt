ClientMonitor
==============================

客户端调用服务方法时会从IMonitor.GetClientSession获取IClientMonitorSession，并在相应时间点调用其方法，顺序为：

* OnInvoking，开始调用服务方法
* OnSerializing，如果请用服务方法有参数会执行到，在序列化之前调用
* OnSerialized，序列化之后调用
* OnInvoked，服务方法已经调用结束

IClientMonitorSession接口定义如下::

	public interface IClientMonitorSession
	{
		void OnInvoking(ClientContext request);

		void OnInvoked(ClientContext request);

		void OnSerializing(ClientContext request);

		void OnSerialized(ClientContext request);
	}
