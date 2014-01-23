FetchURL
###########

服务概述
========

FetchURL是SAE为开发者提供的分布式网页抓取服务，用来同步的抓取http页面，FetchURL针对国内的网络的做了优化，内部有调度系统，尽可能保证用户快速的抓取到目标页面。

FetchURL适用的场景主要有：

+ 抓取http资源
+ 发送http rest请求
+ FetchURL支持https，并且支持重定向
 
服务限制
=================

=============== ========
connect_timeout 5秒
send_timeout    15秒
read_timeout    20秒
抓取文件大小    8MB
=============== ========

禁用头，这些头开发者无法修改 

+ Content-Length
+ Vary
+ Via
+ X-Forwarded-For
+ FetchUrl
+ AccessKey
+ TimeStamp
+ Signature
+ AllowTruncated
+ ConnectTimeout
+ SendTimeout
+ ReadTimeout

.. note::

   SAE规定FetchURL的user-agent默认包含SAE/fetchurl-accesskey，其中accesskey是用户的accesskey，用户可以自行添加User-Agent。Referer也可以自行定义。
 
FetchURL的出口IP列表：

::

    220.181.129.126
    220.181.129.121
    220.181.136.229
    220.181.129.93
    220.181.129.102
    220.181.129.117
    220.181.129.90
    220.181.136.230

如果http接口方需要IP授权可以进行相应的设置。
