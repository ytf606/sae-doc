
.. include:: ../services/mysql.rst

API使用手册
===========

获取MySQL的连接信息。

.. module:: sae.const
   :synopsis: MYSQL连接信息

::

    import sae.const

    sae.const.MYSQL_DB      # 数据库名
    sae.const.MYSQL_USER    # 用户名
    sae.const.MYSQL_PASS    # 密码
    sae.const.MYSQL_HOST    # 主库域名（可读写）
    sae.const.MYSQL_PORT    # 端口，类型为<type 'str'>，请根据框架要求自行转换为int
    sae.const.MYSQL_HOST_S  # 从库域名（只读）

下面就可以跟平常一样使用MySQL服务了，SAE Python内置了MySQLdb模块，对于MySQLdb的使用，可以参考其 `官方文档`_ 。

.. _官方文档: http://mysql-python.sourceforge.net/MySQLdb.html

注意： MySQL 连接超时时间为30s 

配额
==========

.. include:: ../quotas/mysql.rst


