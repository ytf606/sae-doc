Channel
#################

服务概述
========

channel是sae提供的实时消息推送服务。通过在浏览器和SAE服务端之间建立长连接，使得应用可以方便的向javascript客户端实时的推送消息。

下图为channel服务的大致使用流程:

.. image:: /images/channel-overview.png
   :height: 400px

channel服务的使用主要包含两个部分：JS客户端，服务端处理程序。

对于JS客户端，其需要完成：

+ 使用应用服务端创建的channel url连接上channel的服务器。
+ 设置下行消息的处理函数。
+ 需要发送消息时，调用send发送消息给channel服务器（或者直接使用XMLHttpRequest直接发送给应用服务端也可）。

对于应用的服务端：

+ 调用create_channel为每个客户端创建一个channel，并将channel的url返回给客户端。
+ 处理channel server发过来的客户端上行消息（如果有的话）。
+ 有消息要发送时，调用send_message来向客户端推送消息。

当JS客户端和channel服务端连接上/断开或者当JS客户端有发送消息给channel服务器时，channel服务器会使用http回调的方式通知应用。

=========================== ============================================================
回调地址                    事件说明
=========================== ============================================================
/_sae/channel/connected     客户端连接上channel服务器
/_sae/channel/disconnected  客户端和channel服务器断开
/_sae/channel/message       客户端有上行消息，POST内容的message字段为JS客户端发送的内容
=========================== ============================================================

所有的http回调都使用POST方法。所有http回调的POST内容中的from字段为客户端对应channel的名称。其余字段（如果有）见具体回调说明。

.. note::

   1. 每个html页面最多可以建立1个channel连接。
   2. 每个创建的channel只允许一个channel客户端连接。

