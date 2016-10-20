原理介绍
===============

.. image:: ../_static/archecture.png

RpcLite主要分为服务、客户端和治理系统。

RpcLite服务可以供.NET客户端调用，也可在JS中通过Ajax以GET、POST的方式调用。RpcLite的服务端同时也可以是其它服务的客端，服务与客户端之间的调用通过接口作为契约。

客户端调用服务时，服务地址可以写到代码或配置中。为了减少服务与客户端的耦合也可以以通过治理系统动态地获取服务地址。

.. toctree::
  :titlesonly:
  :caption: 索引
  :maxdepth: 1

  layers
  invoke-relation
  service
  client
  registry
