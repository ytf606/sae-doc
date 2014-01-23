相关工具
#############

XHProf
=========

XHProf是Facebook放出的轻量级调试工具。和Xdebug相比，XHProf更加易用和可控，尤其是生成流程图和调试数据对比的功能很好很强大。

PHP Runtime已经集成了XHProf工具，开发者可以方便的利用XHProf进行应用性能调优。下边我们就来看看如何在PHP Runtime中使用XHProf。

首先，XHProf生成的调试数据是存放在用户自己的Storage中的，所以开发者需要在Storage中创建一个名为 `xhprof` 的domain来存储数据。

然后，开发者可以在需要调优的代码前后分别加上 ``sae_xhprof_start()`` 和 ``sae_xhprof_end()`` 。 ::

    sae_xhprof_start();
    // 需要调优的代码
    // ...
    sae_xhprof_end();

访问需要调试的页面，然后进入XHProf页面，就能看到刚才的调试数据了。

.. note:: 调试完毕后最好及时删除Storage中的xhprof这个domain。

.. include:: ../services/libraries.rst
