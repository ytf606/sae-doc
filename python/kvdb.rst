.. include:: ../services/kvdb.rst

API使用手册
==============

..  module:: sae.kvdb

..  py:class:: Error
    :module: sae.kvdb

    通用错误

..  py:class:: RouterError
    :module: sae.kvdb

    路由meta信息错误

..  py:class:: StatusError
    :module: sae.kvdb

    kvdb状态不为OK

..  py:class:: KVClient(debug=0)
    :module: sae.kvdb

    debug 是否输出详细调试、错误信息到日志，默认关闭

    .. py:method:: set(key, val, min_compress_len=0)

       设置key的值为val

       min_compress_len 启用zlib.compress压缩val的最小长度，如果val的长度大于此值
       则启用压缩，0表示不压缩。

    .. py:method:: add(key, val, min_compress_len=0)

       同set，但只在key不存在时起作用

    .. py:method:: replace(key, val, min_compress_len=0)

       同set，但只在key存在时起作用

    .. py:method:: delete(key)

       删除key。

    .. py:method:: get(key)

       从kvdb中获取一个key的值，存在返回key的值，不存在则返回None

    .. py:method:: get_multi(keys, key_prefix='')

       从kvdb中一次获取多个key的值。返回一个key/value的dict。

       keys: key的列表，类型必须为list。

       key_prefix: 所有key的前缀。请求时会在所有的key前面加上该前缀，返回值里所有的key都会去掉该前缀。

    .. py:method:: get_by_prefix(prefix, limit=100, marker=None)

       从kvdb中查找指定前缀的 key/value pair。返回一个generator，yield的item为一个(key, value)的tuple。

       prefix: 需要查找的key的前缀。

       limit: 最多返回的item个数，默认为100。

       marker: 指定从哪一个key开始继续查找，只返回该key后面的结果（该key不含在内）。

    .. py:method:: getkeys_by_prefix(prefix, limit=100, marker=None)

       从kvdb中查找指定前缀的key。返回符合条件的key的generator。

       prefix: 需要查找的key的前缀。

       limit: 最多返回的key的个数，默认为100。

       marker: 指定从哪一个key开始继续查找，只返回该key后面的结果（该key不含在内）。

    .. py:method:: get_info()

       获取本应用kvdb统计数据，返回一个字典::

            {
                'outbytes': 126, 
                'total_size': 3, 
                'inbytes': 180, 
                'set_count': 60,
                'delete_count': 21, 
                'total_count': 1, 
                'get_count': 42
            }

    .. py:method:: disconnect_all()
        
       关闭kvdb连接

使用示例
===============

::

    import sae.kvdb

    kv = sae.kvdb.KVClient()

    k = 'foo'
    kv.set(k, 2)
    kv.delete(k)

    kv.add(k, 3)
    kv.get(k)

    kv.replace(k, 4)
    kv.get(k)

    print kv.get_info()
