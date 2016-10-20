服务端
===============

.. image:: ../_static/rpclist-layers-service.png

RpcLite服务可以被承载于ASP.NET(Full .NET only), Kestrel HttpServer, 或其它自定义组件。

AppHost与ServerHost是相对独立的，在ServerHost收到客户请求后只需要把客户请求转为IServerContext对象，再调用AppHost.ProcessAsync即可。

在ServiceHost中如果Registry支付注册服务地址于可以把服务地址更新到治理系统中，在ServiceHost中还以加入自定义Filter或Monitor等。  