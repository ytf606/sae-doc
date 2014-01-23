Linux/Mac用户入门指南
########################

.. _hello-world-for-linux-and-mac:

首先创建一个应用，登录SAE，访问 `我的应用 <http://sae.sina.com.cn/?m=myapp>`_ ，单击“创建新应用”

.. image:: /images/helloworld-for-linux-mac-1.png
   
填写二级域名，单击“创建应用”，这个域名就是您应用的访问地址

.. image:: /images/helloworld-for-linux-mac-2.png
   
现在您可以关闭浏览器，以下操作将通过代码行完成。

首先将应用同步到本地 ::

    svn co https://svn.sinaapp.com/myhello 

该地址是您的仓库路径，其中 *myhello* 是您刚创建的应用的名称

.. image:: /images/helloworld-for-linux-mac-3.png
   
此时，如果是第一次使用，则需要进行身份认证，按提示输入用户名和密码； 

.. image:: /images/helloworld-for-linux-mac-4.png

.. note:: 这里输入的密码为安全密码（并非微博密码，如已启用微盾动态密码，则此处填写“安全密码”+“微盾动态密码” ）
   
身份验证成功后，会将应用同步到您的工作目录中，创建以您应用命名的目录

进入该目录，创建下一级目录作为版本，注意目录的名称就是应用的版本号，必须是正整数，如：1 ::

    cd myhello
    mkdir 1 

.. image:: /images/helloworld-for-linux-mac-5.png
   
在该版本下创建页面index.html内容为"hello world"，然后将该目录下所有文件提交到SVN中。 ::

    cd myhello
    mkdir 1
    
    echo "hello world" > 1/index.html
    svn add 1/
    svn ci -m "submit code"  # 注意，这里填写的submit code为更新的理由，必填项

.. image:: /images/helloworld-for-linux-mac-6.png
   
现在，在浏览器中输入您应用的地址，就可以马上访问了；本例地址为 http://myhello.sinaapp.com （其中 *myhello* 为您的应用名称）

.. image:: /images/helloworld-for-linux-mac-7.png
