运行环境
##################

基本环境
==============

SAE平台目前的Web服务器使用的是：

+ CentOS-6.x
+ Apache-2.2.x
+ PHP-5.3.x

Web服务器运行在64位Linux环境下。

Apache运行在Prefork模式下，即每个请求都会对应一个Apache进程，请求结束后该进程才能服务于下一个请求。平台通过模块方式扩展了Apache和PHP的相关功能。

与标准LAMP的差异
-------------------

禁用函数和类
-----------------

出于平台安全性考虑，我们禁用了以下函数和类，禁用的标准主要有四点：

1. 出于对安全性的考虑
2. 出于对资源管理的考虑
3. 不常用的API
4. 我们提供更好替代方案的API

 
禁用的函数:

+ php_real_logo_guid
+ php_egg_logo_guid
+ readlink
+ linkinfo
+ symlink
+ link
+ set_file_buffer
+ exec
+ system
+ escapeshellcmd
+ escapeshellarg
+ passthru
+ shell_exec
+ proc_open
+ proc_close
+ proc_terminate
+ proc_get_status
+ proc_nice
+ getmyuid
+ getmygid
+ getmyinode
+ putenv
+ getopt
+ sys_getloadavg
+ getrusage
+ get_current_user
+ magic_quotes_runtime
+ set_magic_quotes_runtime
+ import_request_variables
+ debug_zval_dump
+ ini_alter
+ dl
+ pclose
+ popen
+ stream_socket_client
+ stream_socket_server
+ stream_socket_accept
+ stream_socket_pair
+ stream_get_transports
+ stream_wrapper_restore
+ get_headers
+ mail
+ mb_send_mail
+ openlog
+ syslog
+ closelog
+ apc_add
+ apc_bin_dump
+ apc_bin_dumpfile
+ apc_bin_load
+ apc_bin_loadfile
+ apc_cache_info
+ apc_cas
+ apc_clear_cache
+ apc_compile_file
+ apc_dec
+ apc_define_constants
+ apc_delete_file
+ apc_delete
+ apc_exists
+ apc_fetch
+ apc_inc
+ apc_load_constants
+ apc_sma_info
+ apc_store
+ flock
+ pfsockopen
+ posix_kill
+ apache_child_terminate
+ apache_get_modules
+ apache_get_version
+ apache_getenv
+ apache_lookup_uri
+ apache_reset_timeout
+ apache_response_headers
+ apache_setenv
+ virtual
+ mysql_pconnect
+ socket_create
+ socket_create_pair

禁用的类:

+ SQLiteDatabase
+ SQLiteResult
+ SQLiteUnbuffered
+ SQLiteException
+ APCIterator

沙箱
==========

代码和数据的隔离：每个应用在运行期间，只能“看”到自己的代码和数据，即A应用无法访问B应用的代码和数据。注意，这里提到的在Web服务器上的数据，往往指一些中间处理过程的临时数据，并非最终落地的数据，比如用户上传照片会临时存储到TmpFS。

连接数的隔离：我们知道，程序写的不好，很容易导致阻塞，并进一步导致连接数的飙升。单个应用过多占用Apache连接数，原因往往是多方面的，应用请求外部资源被阻塞是一个最为常见的因素，另外应用页面过大浏览器下载慢也是常见因素之一。公有云平台同一时刻往往运行着大量的应用，如果某一应用出现连接数异常，最直接的后果是整个平台上的所有应用都将陷入瘫痪。SAE平台目前有设置“应用最大HTTP并发连接数”，目前这个值是500，如果应用平均单个请求处理时长是100ms，那么该应用每秒的HTTP并发连接将可以到达5000，每天的请求超过1亿没有问题。但如果您的应用平均每个请求处理时长2秒，那么该应用每秒的HTTP并发连接只能到达250，每天支撑的请求数将在千万。总体而言，尽量迅速处理完请求对应用是有利的，而且也是平台所鼓励的。

内存隔离：目前SAE平台上对单个PHP脚本的处理，设置了64MB的上限(max_memory，ini_set不可修改)，我们认为这个设置是一个相对很高的值，可以说能够满足绝大部分应用的需求。设想一台服务器8G内存，如果每个PHP处理都消耗64M内存，那么该服务器最多只能同时运行128个PHP脚本。SAE引入了”应用最大并发内存数“的概念，目前的设置是4GB。如果应用程序单个请求的内存消耗平均在16MB，那么可同时运行256个请求，这和上面的并发连接数的设定是基本一致的。

CPU隔离：这主要是通过SAE的配额系统来达到CPU时间的隔离。每个应用都有CPU时间消耗的分钟速度限制，避免了某一应用过多非法获取CPU资源导致其它应用响应慢的问题。

目前SAE平台上允许的“单请求最大存活时长”是300秒 。


环境变量
=============

您可以通过打印PHP的全局变量 `$_SERVER` 来获取跟SAE相关的环境变量信息，每个环境变量的信息如下：


===============     ===================================
变量名              说明
===============     ===================================
HTTP_APPNAME        标志该请求属于哪个应用
HTTP_APPVERSION     标志该请求对应该应用的哪个版本
HTTP_ACCESSKEY      该应用访问各种服务资源的帐号
HTTP_SECRETKEY      该应用访问各种服务资源的密码
HTTP_APPCOOKIE      一些和app管理相关信息
HTTP_APPNAME        标志该请求属于哪个应用
===============     ===================================

.. warning:: 不要直接打印出 `$_SERVER` 变量，这样可能会造成应用的AccessKey和SecretKey的泄露。为了应用的安全考虑，请保护好自己的AccessKey和SecretKey。

本地IO
============

考虑到安全和分布式问题，PHP运行环境对本地的IO做了限制：

+ 应用可以以只读权限访问应用目录以及PHP语言的系统库目录。
+ 可写TmpFS
+ 为最大程度降低应用移植的难度，PHP为Storage、Memcache提供了wrapper封装，用户可以像读写文件一样来读写Memcache和Storage。

其中TmpFS的路径可以通过 ``SAE_TMP_PATH`` 这个全局变量获取，该路径具有写权限，用户可以往这个目录下写文件。

.. warning::

    + 临时文件的生存周期等同于PHP请求，也就是当该PHP请求完成执行时，所有写入TmpFS的临时文件都会被销毁
    + TmpFS是本地临时文件，不是共享存储，而SAE是全分布式环境，所以不同请求之间无法通过TmpFS共享操作文件
    + TmpFS操作的文件限于SAE_TMP_PATH目录内，这个目录对每个应用是不一样的
    + TmpFS的文件为纯内存存储

Wrappers
=============

PHP自4.3版本以来，引入了stream流的概念，简单说，就是可以用通用的IO读写函数来操作各种资源，比如：tcp、udp、http、ftp等等，这样做的好处是统一了接口的封装。这就像在Unix中将各种设备都抽象成文件，你可以使用read/write来操作各种设备，这样统一了操作接口，易于理解和使用。Wrappers就是用来告诉stream流该如何处理(读写)特定的资源。

Wrappers使用非常简单，比如下面就是一个最常见一个使用Wrapper的语句： ::

   $c = file_get_contents("http://sae.sina.com.cn");

这里就是使用 `http://` Wrapper实现抓取远程内容并赋值给一个变量的操作。

由于SAE的PHP Runtime环境并不提供持久性本地IO能力，所以PHP Runtime提供了提供了Memcache，Storage，KVDB的Wrappers来方便开发者迁移原有程序。

如果你的原有程序中，使用了本地文件型缓存，那么你可以方便地使用 `saemc://` 替换本地文件缓存的前缀。

如果你的原有程序中，有文件存储的需求，你原来可能是通过NFS或者就是单机提供的存储服务，那么你可以方便地使用 `saestor://` 或 `saekv://` 来替换原来的存储前缀，注意存储的用途是用于文件落地的永久存储，任何缓存、中间临时交换数据的需求都是不适合使用Storage和KVDB存储的。 ::

    # 使用"saekv://"这个Wrapper将配置文件config.php的内容以"config.php"为key保存到KVDB中,
    # 然后用include引用了这个文件
    file_put_contents('saekv://config.php','');
    include 'saekv://config.php';


.. important:: 使用Wrappers请要先初始化相应的服务,上例中KVDB服务必须是开启的状态.
 
核心模块
=====================

============    ========================================================================================
MySQL相关       SAE提供的MySQL驱动是原生的，支持mysql、mysqli和pdo_mysql三个模块，底层基于mysqlnd驱动，
                使用mysqlnd的好处在于扩展开发更加容易。SAE即将上线的主从分离就是基于mysqlnd扩展开发。
Session         SAE提供了session cluster，使用标准的session相关函数即可。
Memcache        SAE提供的memcache模块，有一点微小改动，即连接memcache时，使用memcache_init获得连接句柄。其他完全一致。Memcache的hash使用的是一致性hash。
GD              为保证兼容性，我们也提供了原生的GD模块，但由于GD效率问题，我们并不很鼓励使用。
cURL            cURL目前已经基本做到了基本兼容。注意SAE的cURL是重载了FetchURL服务的，所以使用cURL本质上会不断消耗带宽资源。
XhProf          为方便开发者调试程序，我们也提供了XHProf模块，具体使用见面板的”XHProf”即可。
============    ========================================================================================
 

日志系统
==============

SAE提供了实时的日志检阅功能，方便开发者在线调试分析。

Runtime中输出的日志类型有：access、warning、error、notice、debug等，具体在管理面板的日志中心可以查阅。

.. 请补充每个类型的日志都有什么意义，和runtime怎么对应

应用配置
================

应用可以通过应用版本目录下的 `config.yaml` 来对Apache服务器做一些配置（类似于Apache的htaccess文件）。

通过配置，开发者可以很方便的实现以下功能：

+ 目录默认页面
+ 自定义错误页面
+ 压缩
+ 页面重定向
+ 页面过期
+ 设置响应头的content-type
+ 设置页面访问权限

.. note:: PHP Runtime的config.yaml文件不会部署到代码目录中，而只是存在于SVN中。

应用配置写在 `config.yaml` 文件的 `handle` 下，例如:    ::

    name: saetest
    version: 1

    handle:
    - rewrite: if(!is_dir() && !is_file()) goto "index.php?%{QUERY_STRING}"

基本语法： ::

    - OPTION: ARG1 ARG2 ...
    - OPTION: if(CONDICTION) ACTION

其中 `OPTION` 为配置项， `ARG1` ， `ARG2`  为参数， `CONDITION` 可以是一个或多个 `EXPRESSION` ，视具体的配置项而定。 `ACTION` 是if条件满足后执行的动作。

`EXPRESSION` 有如下形式： ::

    in_header["header_name"] OP string_or_digit
    out_header["header_name"] OP string_or_digit
    path OP string
    query_string OP string
    is_file()
    is_dir()

关于以上形式说明如下：

1. in_header是请求头，out_header 是响应头，header_name是 HTTP Header的名称，具体的请求头和响应头参考RFC官方文档
2. `OP` 是操作符，有~（正则匹配），!~（正则不匹配），==（相等，用于字符串和数字），!=（不相等，用于字符串和数字），> ，>=， <， <=（比较操作符仅用于整形数字）
3. string是形如"xxxx"的字符串 
4. string_or_digit表示string或者digit，根据 `OP` 的种类，后面跟string或者digit
5. path是系统宏，表示用户请求的url去掉主机部分和查询串后剩下的部分
6. query_string是系统宏，表示查询串，一般是url中问号后面的内容
7. is_file()和is_dir是系统函数，判断path是文件还是目录，!is_file()，!is_dir() 分别是其否定形式
8. 当配置项支持多个 `EXPRESSION` 时， `EXPRESSION`` 和 `EXPRESSION` 之间使用&&隔开

下面将详细的介绍如何配置实现以上的这些功能。

目录默认页面
------------------------

当访问url没有指定文件时，指定返回的文件。

语法： ::

    - directoryindex: file_list

file_list中各个文件名以空格分隔，directoryindex在config.yaml文件中仅有一项

例子： ::

    - directoryindex: aaa.php bbb.html

自定义错误页面
-------------------------

语法： ::

    - errordoc: httpcode error_file

httpcode是诸如404、302之类的http响应码，error_file是服务器以httpcode响应请求时响应的文件。errordoc在config.yaml中可以配置多项。

例子： ::

    - errordoc: 404 /path/404.html
    - errordoc: 403 /path/403.html

压缩
---------------

语法： ::

    - compress: if(CONDICTION) compress

在compress中 `CONDITION` 只能有一个 `EXPRESSION` 。

例子： ::

    - compress: if(out_header["Content-Length"] >= 10240) compress
    - compress: if(in_header["Referer"] == "gphone") compress
    - compress: if(path ~ "/big/") compress


.. note::

   通常情况，我们根据响应头Content-length，判断是否需要压缩，例如：if(out_header["Content-Length"]>=10240) compress，这个静态页面，如js,css,html都是没有问题的。但是对php脚本，响应header中没有Content-length这个头，它使用Transfer-Encoding: chunked,这个头表示页面输出用chunked编码。此时要实现压缩，可以通过应用配置，同时在PHP脚本中输出相应头的方式实现。

   例如在应用配置中写if(out_header["Use-Compress"] == "1") compress，在需要压缩的PHP脚本中写header("Use-Compress: 1")。

.. 上面最后一句话的PHP脚本写的内容原文档中缺失，我是根据上下文猜测的，复查者请确认一下并删掉这个注释。

开发者可以通过检查是不是输出了响应头：Content-Encoding: gzip来判断压缩是否生效。

URL重写
-------------------------

语法： ::

    - rewrite: if(CONDITION) goto target_url

在rewrite中， `CONDITION` 支持多个 `EXPRESSION` 。除out_header （没办法根据响应 header 做重定向）外都可以出现在rewrite的 `CONDITION` 中，并且path只能出现一个（如果有多个，只有最后一个生效，其它被忽略），当省略path时，表示任意请求。

target_url表示重定向的目标url，在target_url可以以$N的形式表示path中匹配到的内容，%N的形式表示最后一个query_string中匹配到的内容，因为query_string可以在 `CONDITION` 中出现多次，以%{QUERY_STRING}表示查询串。
 
例子： ::

    # 当 url 匹配 urldir/(.*) ，并且 输入 header referer 等于 sina 时，跳转至页面 /usr/$1，$1 表示刚刚匹配的 urldir/(.*) 中的 (.*) 部分。
    - rewrite: if (path ~ "urldir/(.*)" && in_header["referer"] == "sina") goto "/url/$1"

    # 当 url 匹配 urldir/(.*)，并且请求的是一个目录时，跳转至 /url/$1
    - rewrite: if(is_dir( ) && path ~ "urldir/(.*)") goto "/url/$1"

    # 当 url 匹配 path，并且请求的不是一个文件时，跳转至 /url/query.php
    - rewrite: if(! is_file() && path ~ "path") goto "/url/query.php"

    # 当查询串等于so，并且 url 以 zhaochou 结尾时，跳转至 /url/%1，%1 表示 query_string 匹配到的部分。
    - rewrite: if(query_string ~ "^(so)$" && path ~ "zhaochou$") goto "/url/%1"

    # 当查询串不包含sohu，并且 url 以zhaochou结尾时，跳转至/url/query.php?%{QUERY_STRING}，%{QUERY_STRING}表示查询串。
    - rewrite: if(query_string !~ "sohu" && path ~ "zhaochou$") goto "/url/query.php?${QUERY_STRING}"

    # 如果 url 既不是文件，也不是目录，跳转至 index.php?%{QUERY_STRING}
    - rewrite: if( !is_file() && !is_dir()) goto "index.php?%{QUERY_STRING}"

.. warning::

   1. 如果有形如path ~ "^(.*)$"类的请求，一定要加上is_file或is_dir之类的判断，防止无穷的rewrite。 
   2. 在goto语句中，虽然某些时候可以不以/开头，但是强烈建议以/开头。 

指定过期时间和头信息
-------------------------

语法： ::

    - expire: if(single_express) time seconds
    - mime: if(single_express) type content-type

seconds是秒数，content-type是表示文档类型的字符串。

例子： ::

    - expire: if(in_header["referer"] ~ "sina") time 10
    # 如果 url 请求文件的扩展名是 pdf2，设置 Content-Type 为 application/pdf
    - mime: if(path ~ "\.pdf2$") type "application/pdf"
    - mime: if(path ~ "\.pdf2$") type "application/pdf"
    # 只要请求 header referer 包含字符串 sina，就设置 Content-Type 为 text/plain
    - mime: if(in_header["referer"] ~ "sina") type "text/plain"

在expire和mime的 `CONDITION` 只支持单个 `EXPRESSION` ，in_header，path都可以出现在 `CONDITION` 中， `OP` 只能是~或者==，即只支持正则匹配和字符串比较。

基于主机的访问控制
------------------------

语法： ::

    - hostaccess: if(CONDITION) deny IP
    - hostaccess: if(CONDITION) allow IP

`CONDITION` 只支持单个 `EXPRESSION` 。

IP需要加引号，IP可以是一个或多个ip地址、all（所有IP地址）、 `CIDR`_ （如108.192.8.0/24），具体可以参考Apache配置，allow是白名单，deny是黑名单。

.. _CIDR: http://zh.wikipedia.org/wiki/%E6%97%A0%E7%B1%BB%E5%88%AB%E5%9F%9F%E9%97%B4%E8%B7%AF%E7%94%B1

例子: ::

    # 禁止127.0.0.1 访问private目录
    - hostaccess: if(path ~ "/private/") deny "127.0.0.1"

    # 只允许127.0.0.1 访问.conf结尾的文件
    - hostaccess: if(path ~ "\.conf$") allow "127.0.0.1"

    # 禁止127.0.0.1 的所有访问（这个要慎用）
    - hostaccess: deny "127.0.0.1"

    # 对cron任务保护，防止被外部抓取，我们将cron任务放在cron目录下(sae中cron服务执行时，走的是内部网络)
    - hostaccess: if(path ~ "/cron/") allow "10.0.0.0/8" 允许10打头的所有IP 

    # 对于屏蔽一组IP地址，可以写成子网掩码形式，或者将多个IP之间加以空格。子网掩码形式如下：
    - hostaccess: if(path ~ "/cron/") deny "108.192.8.0/24" 屏蔽108.192.8打头的所有IP 

    # 允许108.134.13.24和108.122.122.13这两个IP
    - hostaccess: allow "108.134.13.24 108.122.122.13" 

HTTP基础认证
----------------------

语法： ::

    - passwdaccess: passwd "USERNAME:PASSWORD..."
    - passwdaccess: if(CONDITION) passwd "USERNAME:PASSWORD..."

例子： ::

    # 所有访问都要密码，允许用户writer用密码123zxc访问
    - passwdaccess: passwd "write:123zxc"

    # 访问secret目录需要密码，允许用户test用密码123qwe访问，用户coder用密码123asd访问
    - passwdaccess: if(path ~ "/secret/") passwd "test:123qwe coder:123asd"

    # 访问.text结尾的文件需要密码，允许用户writer用密码123zxc
    - passwdaccess: if(path ~ "\.text$") passwd "writer:123zxc"

    # 用户的网站后台程序都放在admin目录下，需要对admin目录做密码保护
    - passwdaccess: if(path ~ "/admin/") passwd "admin:admin123"

`CONDITION` 只能是单个 `EXPRESSION` ，in_header，path可以出现在 `EXPRESSION` 中， `OP` 只能是~或者==，即只支持正则匹配和字符串比较。

