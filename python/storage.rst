.. include:: ../services/storage.rst

API使用手册
==================

..  module:: sae.storage

..  py:class:: Error()

    Storage异常类。

..  module:: sae.storage

..  py:class:: Connection(accesskey=ACCESS_KEY, secretkey=SECRET_KEY, account=APP_NAME, retries=3)

    Connection类

    accesskey: 可选。storage归属的应用的accesskey，默认为当前应用。

    secretkey: 可选。storage归属的应用的secretkey，默认为当前应用。

    account: 可选。storage归属的应用的应用名，默认为当前应用。

    retries: 请求失败时重试的次数。

    .. py:method:: get_bucket(bucket)

       获取一个bucket类的instance。

       bucket: bucket的名称。

..  module:: sae.storage

..  py:class:: Bucket(bucket, conn=None)

    Bucket类，封装了大部分的storage操作。

    bucket: bucket的名称。

    conn: 可选。一个sae.storage.Connection的实例。

    .. py:method:: put(acl=None, metadata=None)

       创建一个bucket。

       acl: bucket的 :ref:`about-acl` (Access Control List)。

       metadata: 需要保存的元数据，metadata应该是一个dict，例如 `{'color': 'blue'}` 。

    .. py:method:: post(acl=None, metadata=None)

       修改bucket的acl和metadata。其中metadata的修改为增量修改。

    .. py:method:: list(prefix=None, delimiter=None, path=None, limit=10000, marker=None)

       列出bucket中的object。

       prefix: object名称的前缀。

       delimiter: 分割字符。折叠包含该分割字符的条目。

       path: 返回路径path下的全部objects。等价于prefix为path，delimiter为/。

       limit: 最大返回的objects条数。

       marker: 返回object名为marker后面的结果（不包含marker）。

       返回符合条件的objects的属性的一个generator。

    .. py:method:: stat()

       返回当前bucket的属性信息。

    .. py:method:: delete()

       删除bucket。被删除的bucket必须已经清空（没有任何object）。

    .. py:method:: put_object(obj, contents, content_type=None, content_encoding=None, metadata=None)

       创建或更新一个object。

       obj: object的名称。

       contents: object的内容，可以是字符串或file-like object。

       content_type: object的mime类型。

       content_encoding: object的encoding。

       metadata: object的元数据。

    .. py:method:: post_object(obj, content_type=None, content_encoding=None, metadata=None)

       更新object的一些属性。

       注意：object的metadata的更新是全量的，和container的增量修改不一样。

    .. py:method:: get_object(obj, chunk_size=None)

       获取object的内容和属性信息。

       obj: object的名称。

       chunk_size: 不返回object的全部内容，而是返回一个文件内容的generator，每次iterate返回chunk_size大小的数据。

       返回一个tuple (obj的属性信息, object的内容)。

    .. py:method:: get_object_contents(obj, chunk_size=None)

       获取object的内容。

       参数同get_object，只返回object的内容。

    .. py:method:: stat_object(obj)

       获取object的属性信息。

    .. py:method:: delete_object(obj)

       删除object。

    .. py:method:: generate_url(obj)

       返回object的public url。


.. _about-acl:

ACL
------------

ACL的格式是： `[item[,item...]]` 。 每一个item指定了一个referer的访问权限，item的格式为： `.r:[-]value` 。 

+ value可以为 `*` （任何referrer host都可以访问）
+ 如果value的开头包含 `.` 或者 `*` ，则其为一个域名通配。
  比如： `.example.com` 或者 `*.example.com` 。表示以.example.com结尾的referer host可以访问。
+ 在value的前面加上 `-` 则表示禁止referer host为这一域名的访问。
+ acl为空时表示不允许任何referer host的访问，也就等于该bucket为私有bucket。

例：  ::

    .r:*
    .r:-.thief.com
    .r:.example.com,.r:-thief.example.com

缓存过期
--------------

如果用户没有设置bucket或者object的缓存过期时间，storage默认的时间是2小时。

用户可以通过以下metadata设置object的缓存过期时间: ::

    expires       该metadata可以加在bucket或者object上，用来设置object的缓存过期。
                  其value的格式为：[modified] time_delta

    expires-type  该metadata可以加在bucket上。对于该bucket中的object，按照其mime_type来设置对应的缓存过期时间。
                  其value的格式为：mime_type [modified] time_delta
                  如果有多条规则，规则和规则之间使用逗号（，）隔开。

mime_type为object的mime类型，例如： text/html, text/plain, image/gif

time_delta是一个表示时间的字符串，例如： `1y3M`, `48d`, `5s`, `-1` ::

    s   seconds
    m   minutes
    h   hours
    d   days
    w   weeks
    M   months, 30 days
    y   years, 365 days

modified关键字用于指定缓存过期的时间相对于object的最后修改时间。默认expire时间是相对于访问时间。

如果time_delta为负， Cache-Control header会被设置为no-cache. 

正的time_delta会设置Cache-Control为max-age = #，其中 # 是缓存过期的时间（单位为秒）。

模拟storage为文件系统
-----------------------------

::

    from sae.ext.storage import monkey
    monkey.patch_all()

以上代码会将storage ‘挂载’到 `/s/` 目录下，每个bucket为这个目录下的一个子目录。用户可以使用 `/s/<bucket-name>/<object-name>` 这样形式的路径通过文件系统接口来访问storage的中的object。

目前支持（patch）的文件系统接口函数为： `open, os.listdir, os.mkdir, os.path.exists, os.path.isdir, os.open, os.fdopen, os.close, os.chmod, os.stat, os.unlink, os.rmdir`

使用示例
=============

在SAE runtime中操作storage  ::

    >>> # 创建一个bucket的instance
    >>> from sae.storage import Bucket
    >>> bucket = Bucket('t')

    >>> # 创建该bucket
    >>> bucket.put()

    >>> # 修改该bucket的acl和缓存过期时间。
    >>> bucket.post(acl='.r:.sinaapp.com,.r:sae.sina.com.cn', metadata={'expires': '1d'})

    >>> # 获取该bucket的属性信息
    >>> attrs = bucket.stat()
    >>> print attrs
    {'acl': '.r:.sinaapp.com,.r:sae.sina.com.cn',
     'bytes': '0',
     'metadata': {'expires': '1d'},
     'objects': '0'}
    >>> attrs.acl
    '.r:.sinaapp.com,.r:sae.sina.com.cn'

    >>> # 存取一个字符串到bucket中
    >>> bucket.put_object('1.txt', 'hello, world')

    >>> # 获取object的public url
    >>> bucket.generate_url('1.txt')
    'http://pylabs-t.stor.sinaapp.com/1.txt'

    >>> # 存取一个文件到bucket中
    >>> bucket.put_object('2.txt', open(__file__, 'rb'))
    
    >>> # 列出该bucket中的所有objects
    >>> [i for i in bucket.list()]
    [{u'bytes': 12,
      u'content_type': u'text/plain',
      u'hash': u'e4d7f1b4ed2e42d15898f4b27b019da4',
      u'last_modified': u'2013-05-22T05:09:32.259140',
      u'name': u'1.txt'},
     {u'bytes': 14412,
      u'content_type': u'rb',
      u'hash': u'99079422784f6cbfc4114d9b261001e1',
      u'last_modified': u'2013-05-22T05:12:13.337400',
      u'name': u'2.txt'}]

    >>> # 获取object的所有属性
    >>> bucket.stat_object('1.txt')
    {'bytes': '12',
     'content_type': 'text/plain',
     'hash': 'e4d7f1b4ed2e42d15898f4b27b019da4',
     'last_modified': '2013-05-22T05:09:32.259140',
     'metadata': {},
     'timestamp': '1369199372.25914'}

    >>> # 取object的内容
    >>> bucket.get_object_contents('1.txt')
    'hello, world'

    >>> # 取文件的内容，返回generator
    >>> chunks = bucket.get_object_contents('2.txt', chunk_size=10)
    <generator object _body at 0x95ec20c>
    >>> chunks.next()      # 显示第一个chunk
    '\n# Copyrig'

    >>> # 删除objects
    >>> bucket.delete_object('1.txt')
    >>> bucket.delete_object('2.txt')

    >>> # 删除bucket
    >>> bucket.delete()

storage中不支持创建实际的目录，但是用户可以通过在object name中加入 `/` 来模拟目录结构。例如： ::

    >>> bucket.put_object('a/1.txt', '')
    >>> bucket.put_object('a/b/2.txt', '')

    >>> # 列出根目录下的所有objects。
    >>> [i for i in bucket.list(path='')]
    [{u'bytes': None,
      u'content_type': None,    # content_type为None表示这是一个子目录
      u'hash': None,
      u'last_modified': None,
      u'name': 'a/'}]

    >>> # 列出a目录下的objects
    >>> [i for i in bucket.list(path='a/')]
    [{u'bytes': 0,
      u'content_type': u'text/plain',
      u'hash': u'd41d8cd98f00b204e9800998ecf8427e',
      u'last_modified': u'2013-05-23T03:01:59.051030',
      u'name': u'a/1.txt'},
     {u'bytes', None,
      u'content_type': None,    # content_type为None表示这是一个子目录
      u'hash': None,
      u'last_modified': None,
      u'name': u'a/b/'}]

.. note:: 在list目录（除了根目录）时路径的末尾要带上 / 。

