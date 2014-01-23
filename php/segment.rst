.. include:: ../services/segment.rst

API使用手册
============

.. 这里写API Reference或者贴对应服务的API Reference的链接
在SAE在线管理平台进入应用的“分词服务”管理页面，启用分词服务，即可开始使用。

代码示例：::
    
    <?php
        $str = "明天是星期天"
        $seg = new SaeSegment();
        $ret = $seg->segment($str, 1);
      
        print_r($ret);    //输出
      
        // 失败时输出错误码和错误信息
        if ($ret === false)
            var_dump($seg->errno(), $seg->errmsg());
    ?>

使用示例
============

.. 这里贴示例
