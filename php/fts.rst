.. include:: ../services/fts.rst

API使用手册
============

.. 这里写API Reference或者贴对应服务的API Reference的链接

SAE全文检索服务使用方法非常简单，开发者只须提交文档内容，待索引成功建立后即可检索，示例：

添加文档： ::

       $fts = new SaeFTS();
       $docid = 1;
       $content = "this is a test string";
       $ret = $fts->addDoc($docid, $content);
       if($ret === true){
           echo "add doc success";
       }else{
           var_dump($fts->errno(), $fts->errmsg());
       }

删除文档： ::

       $ret = $fts->deleteDoc($docid);
       if($ret === true){
           echo "delete doc success";
       }else{
           var_dump($fts->errno(), $fts->errmsg());
       }

修改文档： ::

       $ret = $fts->modifyDoc($docid, 'new content');
       if($ret === true){
           echo "modify doc success";
       }else{
           var_dump($fts->errno(), $fts->errmsg());
      }

检索文档： ::

       $arr = $fts ->search('test');
       if ( $arr === false ) {
           var_dump($fts->errno(), $fts->errmsg());
       } else {
           print_r($arr);
       }


使用示例
============

.. 这里贴示例
