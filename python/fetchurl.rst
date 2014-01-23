.. include:: ../services/fetchurl.rst

API使用手册
=============

直接使用urllib, urllib2或者httplib模块访问网络资源即可。SAE Python已经内置了对FetchURL的支持。

对于fetchurl不支持的功能（HTTP代理等），可以禁用掉FetchURL服务，使用socket服务来处理请求。 ::

    import os
    os.environ['disable_fetchurl'] = "1"
