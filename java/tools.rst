相关工具
############

本地开发环境
==================

搭建Java本地开发环境非常简单，只需将以下jar包放至应用的classpath下即可,你也可以根据需要删减部分非全局jar包。 ::

    sae-local-1.0.jar(全局)
    log4j.jar (全局)
    derby.jar (kvdb服务)
    mail.jar (mail服务)
    activation.jar (mail服务)
    commons-codec-1.4.jar (Storage,TaskQueue,FetchURL服务)
    commons-logging-1.1.1.jar (Storage,TaskQueue,FetchURL服务)
    httpclient-4.1.2.jar (Storage,TaskQueue,FetchURL服务)
    httpclient-cache-4.1.2.jar (Storage,TaskQueue,FetchURL服务)
    httpcore-4.1.2.jar (Storage,TaskQueue,FetchURL服务)
    httpmime-4.1.2.jar (Storage,TaskQueue,FetchURL服务)
    commons-beanutils-1.8.3.jar (Storage,TaskQueue,FetchURL服务)
    commons-beanutils-bean-collections-1.8.3.jar (Storage,TaskQueue,FetchURL服务)
    commons-beanutils-core-1.8.3.jar (Storage,TaskQueue,FetchURL服务)
    commons-collections-3.2.1.jar (Storage,TaskQueue,FetchURL服务)
    commons-lang-2.6.jar (Storage,TaskQueue,FetchURL服务)
    ezmorph-1.0.6.jar (Storage,TaskQueue,FetchURL服务)
    json-lib-2.4-jdk15.jar (Storage,TaskQueue,FetchURL服务)
    commons-io-1.3.2.jar (Storage,TaskQueue,FetchURL服务)
    mysql-connector-java-5.0.8-bin.jar(MySQL服务驱动包)

:doc:`点击这里去资源下载页面下载所有jar包 </download/java>`

.. note:: 

   1. 上传war包到线上环境时候可将包中以上jar包都删除不会影响应用运行(实际加载SAE线上jar包)，从而减小war包实际大小。
   2. 如果需要使用Memcache服务则需要多一步操作：在本地安装Memcache,可以去官网下载(http://memcached.org/)，具体各服务的使用和线上一致。

Eclipse插件
========================

Eclipse插件提供在Eclipse IDE环境下提交,更新，删除单个或者多个文件至SAE线上应用的功能，并且变更操作及时生效。

:doc:`点击这里去下载页面下载最新的Eclipse插件包 </download/java>`

安装插件：

1. 下载插件至某目录下，打开jee-eclipse；
2. 在jee-eclipse中打开菜单 Help->Install New SoftWare， 打开Install视图 -> Add -> Local选择eclipse插件文件路径->OK 接下来一直Next直到安装完毕；
3. 重启Eclipse，查看选中文件的右键菜单中是否有Sina App Engin选项，如果有就说明安装成功。

点击这里查看插件安装和使用的相关视频 `视频 <http://sae4java.sinaapp.com/eclipse/index.html>`_ 。

使用插件：

安装完Eclipse插件后，我们就可以在Eclipse环境下提交或者删除一个或多个文件至SAE Java应用中。

.. image:: /images/eclipse-plugin.jpg

.. note::

   -  应用名、应用版本、安全邮箱、安全密码为必填项
   -  选择Eclipse中的Java文件，插件会自动寻找相应的class文件来提交
   -  列表中的War Path指的是文件在war包中的路径
   -  通过Configuration区域可删除上传文件和编辑上传文件在war包中的路径或操作

.. include:: ../services/libraries.rst
