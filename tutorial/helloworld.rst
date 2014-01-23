Hello, World!
==================

**准备工作**

1. 首先，您需要具备SAE的账号，如果您没有该账号，请到SAE官方网站申请
2. 下载并安装SVN客户端，下载地址及安装方法，请参见 在Windows下使用SVN部署代码

如果您已经完成了以上工作，接下来就可以开始创建您的第一个应用了。（以下教程使用Windows+SVN Tortorise，Linux/Mac/SVN命令行使用者请参见 :doc:`helloworld-for-linux-mac` ）。

首先登录SAE，访问 我的应用，单击“创建新应用”

.. image:: /images/hello-world-1.png

填写二级域名，单击“创建应用”，这个域名就是您应用的访问地址

.. image:: /images/hello-world-2.png

现在您可以关闭浏览器，以下操作将在您的本地计算机上进行（请确保SVN已经安装好）

进入您的本地工作目录，如 `D:\SAE` 。单击鼠标右键，在弹出的菜单中选择“SVN Checkout”

.. image:: /images/hello-world-3.png

在弹出页面中填写仓库路径，如： `https://svn.sinaapp.com/myhello` （其中 `myhello` 是您刚创建的应用名称）

.. image:: /images/hello-world-4.png

单击“OK”开始同步，如果是第一次使用会弹出Authentication窗口进行身份验证。（另外，如果您不希望每次使用都进行身份验证，可以勾选Save authentication复选框）。

+ username：注册SAE时填写的 **安全邮箱** （并非微博帐号）
+ password：注册SAE时填写的 **安全密码** （并非微博密码）

.. image:: /images/hello-world-5.png

身份验证成功后SVN会自动将您的应用同步到本地工作目录中并创建以您应用命名的文件夹

.. image:: /images/hello-world-6.png

接下来需要创建一个版本，在该文件夹中创建一个新的文件夹作为这个应用的版本（注意文件夹的名称就是应用的版本号，必须是正整数，如：1, ...）

现在您可以使用您最喜欢的编码工具（如：EditPlus、Notepad++等）在该文件夹下创建您的第一个页面，如 index.php（如果你创建的是PHP应用），文件内容如下： ::

    <?php
    echo 'Hello, World!';
    ?>

完成后，该文件左侧会出现一个红色的“！”，右键单击该文件，在弹出的菜单中选择SVN Commit...，执行提交操作

.. image:: /images/hello-world-7.png

在弹出的窗口的Message处填写更新的理由（必填项，不填写会导致上传失败）

.. image:: /images/hello-world-8.png

上传成功后显示以下界面

.. image:: /images/hello-world-9.png

现在，在浏览器中输入您的应用的地址，就可以马上访问了；本例地址为 http://myhello.sinaapp.com （其中 myhello 为您的应用名称）

.. image:: /images/hello-world-10.png

下面选择一门你喜欢的语言，我们开始学习和开发我们在SAE上第一个应用吧。

