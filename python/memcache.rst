.. include:: ../services/memcache.rst

API使用手册
=============

..  module:: pylibmc
    :synopsis: memcache模块

SAE Python使用 http://sendapatch.se/projects/pylibmc/ 作为mc客户端。
不同之处在于，创建Client时不用指定servers参数（如果指定了会被忽略）。

pylibmc接口和 `python-memcached`_ 基本兼容，可以直接替换使用。 `python-memcache文档 <../_static/memcache.html>`_ 。

对于现有使用python-memcache的代码，可以直接在index.wsgi中（任何import memcache语句执行之前）加入如下代码，即可不修改代码使用pylibmc了。 ::

    import pylibmc
    import sys
    sys.modules['memcache'] = pylibmc

.. _python-memcached: http://www.tummy.com/Community/software/python-memcached/

使用示例
=============

::

    import pylibmc as memcache

    mc = memcache.Client()
 
    mc.set("foo", "bar")
    value = mc.get("foo")
 
    if not mc.get('key'):
        mc.set("key", "1")
    mc.incr("key")

