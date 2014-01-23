代码部署手册
################

概述
=========

SAE上所有的应用创建时，系统会自动为该应用创建一个SVN仓库。该SVN仓库是所有代码部署方式的最终入口。开发者可以使用SVN客户端提交代码来完成应用代码的部署。或者使用在线代码编辑器。

.. image::  /images/code-deploy-workflow.png

=============   ======================================
SVN仓库地址     https://svn.sinaapp.com/YOUR_APP_NAME
SVN用户名       SAE安全邮箱
SVN密码         SAE安全密码
=============   ======================================

.. note::

   SVN仓库地址可以在应用管理面板的“代码管理”中找到，仓库地址可以直接使用浏览器进行访问。
   SVN用户名和密码为安全邮箱和安全密码，不是微博账号和微博密码！
   如已启用微盾动态密码，则此处填写“安全密码”+“微盾动态密码”

使用SVN客户端
==================

TortoiseSVN客户端
----------------------

.. note:: 如果使用TortoiseSVN 1.8.x部署代码发生问题，请尝试更新至最新版本或者使用1.8以下的版本。

在Windows下推荐使用乌龟(Tortoise)SVN客户端。 TortoiseSVN 是 Subversion 版本控制系统的一个免费开源客户端，可以超越时间的管理文件和目录。文件保存在中央版本库（即SAE中央SVN仓库），除了能记住文件和目录的每次修改以 外，版本库非常像普通的文件服务器。你可以将文件恢复到过去的版本，并且可以通过检查历史知道数据做了哪些修改，谁做的修改。这就是为什么许多人将 Subversion 和版本控制系统看作一种“时间机器”。

TortoiseSVN下载： http://tortoisesvn.net/downloads.html 

下面详细介绍使用TortoiseSVN向SAE部署代码。

第一步，如图1，创建一个新文件夹作为本地工作目录(Working directory)，可以使用应用名为文件夹名。如，为我的应用devcenter创建本地工作目录。

.. figure:: /images/code-deploy-a0.jpg
   
第二步，从SAE的SVN仓库检出(checkout)一个应用的全部版本代码，如图2，右键-->点击“SVN Checkout”

.. figure:: /images/code-deploy-a1.jpg
   
在弹出页面中填写仓库路径即可，这里是：https://svn.sinaapp.com/devcenter/，其它默认参数即可，如图3：

.. figure:: /images/code-deploy-a2.jpg
   
Reversion处，“HEAD revision”是指最新版，也可以指定Revision为任意一个版本。

点击“OK”，出现下载界面，如图4：

.. figure:: /images/code-deploy-a3.jpg
   
如果一切顺利，devcenter应用所有版本代码将会全部出现在刚刚创建的devcenter文件夹下，如图5：

.. figure:: /images/code-deploy-a4.jpg
   
在本地使用你喜欢的编辑器，编辑任意文件，保存后该文件图标将会出现红色感叹号，如图6所示：

.. figure:: /images/code-deploy-a5.jpg
   
看，刚刚修改过的index.php变色了。下面需要提交(commit)最近的更新。在index.php文件上击右键，出现菜单 

选择“SVN commit”，如图7：

.. figure:: /images/code-deploy-a6.jpg
   
然后填写关于本次更新的日志（log message），这是必填项，否则commit会失败。如图8：

.. figure:: /images/code-deploy-a7.jpg
   
当您看到如图9所示，表明刚才的修改已经成功提交，并且前该devcenter项目的SVN版本号加1，变成30.

.. figure:: /images/code-deploy-a8.jpg
   
在SVN工作目录下，对于文件修改，完成后只需要commit就ok了，但对于新增文件，或者从其它目录复制进来的文件或文件夹，需要在commit之前需 要做一步add操作，即将文件或文件夹添加到svn工作目录中来，否则SVN客户端不认它。具体操作很简单，如图10:

.. figure:: /images/code-deploy-a9.jpg
   
然后再如图7所示，右键commit 即可。

更多Tortoise SVN使用帮助，请参阅： http://www.subversion.org.cn/tsvndoc/ 

Notepad++集成SVN插件
---------------------------

Notepad++是一款Windows环境下很受欢迎的免费开源的代码编辑器，有很丰富的第三方插件。

下面介绍Notepad++的SVN插件的安装及使用。

如图12，点击Plugin --> Plugin Manager --> Show Plugin Manager

.. figure:: /images/code-deploy-a11.jpg
   
打开后，在“Available”页找到“Subversion”，然后点击“Install”，如图13所示：

.. figure:: /images/code-deploy-a12.jpg
   
安装成功后会在Plugins菜单下出现"Subversion"子菜单。

打开之前checkout下来的任一文件，如devcenter/1/index.php，编辑完成后，点击Plugins --> Subversion --> Commit File(或使用 快捷键Ctrl+Alt+A)

出现熟悉的一页，如图14所示：

.. figure:: /images/code-deploy-a13.jpg
   
对，这就是乌龟SVN的Commit页面，输入Log Message，点击“OK”即可提交。

使用UltraEdit Studio部署
-----------------------------

UltraEdit Studio(注意不是UltraEdit，UltraEdit不能与SVN整合)配置稍微复杂些。

第一步，SVN仓库账号设置，如图15所示：

.. figure:: /images/code-deploy-a14.jpg
   
然后填写仓库信息，如图16所示：

.. figure:: /images/code-deploy-a15.jpg
   
完成后，创建Project，如图17所示：

.. figure:: /images/code-deploy-a16.jpg
   
填写project名b.prj后，出现如图18所示"Project Settings"，点击“Add Folder”，出现“NewFolder”，将"Group"前的勾去掉，然后点击“Browse”，在“Browse for Folder”页找到之前checkout出现的本地工作目录。

.. figure:: /images/code-deploy-a17.jpg
   
点击“OK”后出现提示，如图19所示：

.. figure:: /images/code-deploy-a18.jpg
   
表明UEStudio自动识别出了选择的代码目录是一个SVN工作目录，并询问是否整合，点击“Yes”，出现如图20所示：

.. figure:: /images/code-deploy-a19.jpg
   
大功告成，"Close"之后即可以编辑代码，并随时commit修改。如图21所示：

.. figure:: /images/code-deploy-a20.jpg
   
使用EditPlus部署
--------------------

EditPlus依赖于Tortoise SVN(TSVN)，必须先安装。

使用最简单，见图22所示：

.. figure:: /images/code-deploy-a21.jpg
   
当编辑SVN工作目录里的文件时，File --> TSVN即出现相应可操作的菜单。

SVN命令行工具
------------------

例如，您刚刚创建了应用newapp，在本地执行： ::

    svn checkout https://svn.sinaapp.com/newapp

第一次checkout时需要验证，用户名/密码为您的SAE安全邮箱和安全密码(非微博登陆账号密码，如已启用微盾动态密码，则此处填写“安全密码”+“微盾动态密码”)。

1. 上传代码。如部署wordpress到版本1。 ::

    svn checkout https://svn.sinaapp.com/newapp
    cd newapp/1
    cp -rf /path/to/wordpress/* ./
    svn add ./
    svn commit -m"add wordpress"

2. 修改代码。假如需要修改newapp应用版本1根目录下某个文件，如index.php，完整的操作流程如下： ::

    svn checkout https://svn.sinaapp.com/newapp #如果已经checkout过了，不需要重新checkout。
    cd newapp/1
    vim index.php #这里编辑代码
    svn commit -m "edit index.php"

.. note:: 应用配置文件config.yaml不会被部署到用户的代码目录，但会被存储在服务端SVN仓库中。因此下次可以被checkout出来，但不会被用户访问到。
   
SAE提供的SVN服务完全支持标准SVN的所有命令，更详细的使用说明请查阅SVN官方帮助文档。

+ SVN命令帮助文档：http://www.subversion.org.cn/svnbook/1.4/svn.ref.html
+ 中文完全帮助文档：http://svnbook.red-bean.com/index.zh.html

SVN for SAE的限制条件
------------------------

为了保持与SAE兼容，使用svn方式部署代码到SAE有如下限制：

1. 文件名或目录名不允许含有以下字符：",\*,?,<,>,\|，另外文件或文件名的开始与结束也不允许有空格。
2. 上传单个文件大小不超过4M
3. 单个目录下的文 件个数不能超过400个
4. 每个应用代码总大小不超过100M
5. 单个版本代码总大小不超过50M
6. appname目录下只允许存在10个以内的版本，并且版本号必须为正整数。（也就是说appname下面只允许出现10个以内的正整数 目录名，不允许有非目录的存在）

常见错误说明及解决办法
---------------------------

+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|错误提示                                         |  错误原因                                   |解决方法                                  |
+=================================================+=============================================+==========================================+
|The change log need some words(not null)         |  提交时没有添加log message                  |提交时加上log message即可                 |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|The log message is a pathname (was -F intended?);|  与路径名称相同                             |修改log message                           |
|use '--force-log' to override log message        |                                             |                                          |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|Error file xxx in not allowed in root directory! |  working copy的根目录下不允许有名为xxx的文件|删除该文件或者移到到版本目录下            |
|                                                 |  （只能是正整数版本目录）                   |                                          |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|directory xxx/ is not allowed in root directory! |  working copy的根目录下不允许有名为xxx的目录|删除该目录或者移到版本目录下              |
|                                                 |  （只能是正整数版本目录）                   |                                          |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|Only positive integer dirs are allowed!          |  版本目录多于10个，最多不超过10个           |删除刚刚创建的版本目录即可。              |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|Too many versions!(less than 10 is permited)     |  XXX目录下文件的数量超过2000                |删除XXX目录下的文件，使其总数降至2000以下 |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|Too many files: XXX (less than 2000 is allowed). |                                             |                                          |
|dir APPNAME/dir(size:xxxx) is too large less     |  该应用下代码(不包括.svn目录)总大小超过     |删除不必要的代码再尝试提交                |
|than xxx                                         |  100M，或某个版本总大小超过50M              |                                          |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|File XXX is too large!(less than 4M)             |  单个文件大小超过4M                         |删除重试                                  |
|类似Repository UUID                              |  服务器端仓库被删除重建                     |重新checkout出一份代码即可                |
|'305daf1b-94be-48ee-a4b9-f4bcd36320de' doesn't   |  客户端对原仓库进行操作导致。               |                                          |
|match expected UUID                              |                                             |                                          |
|'44e2f7a2-1b69-4710-974a-b6edef0fdc12'           |                                             |                                          |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+
|add vers xxx faild                               |  文件部署失败                               |内部原因，请联系saesupport@sina.cn。      |
+-------------------------------------------------+---------------------------------------------+------------------------------------------+

使用在线编辑器
===============

在线编辑器 可在管理面板>>代码管理>>编辑代码 打开

.. note:: 如果通过在线代码编辑器对代码进行修改、部署等操作，也会和执行 svn commit一样，产生一个新的提交，即可以使用SVN客户端执行svn update获取最近的更新或其他操作。
