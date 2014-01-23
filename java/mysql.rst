.. include:: ../services/mysql.rst

API使用手册
===========

SAE上的MySQL服务和普通MySQL服务几乎一样，所以如果你已经熟悉了MySQL的常规使用的话，会感到很简单。对于mysql/mysqli模块的用法，您可以参考PHP官方手册。

SAE也提供了基于MySQL模块的SaeMysql类： ::

    <?php
    $mysql = new SaeMysql();

    $sql = "SELECT * FROM `user` LIMIT 10";
    $data = $mysql->getData( $sql );
    $name = strip_tags( $_REQUEST['name'] );
    $age = intval( $_REQUEST['age'] );
    $sql = "INSERT  INTO `user` ( `name`, `age`, `regtime`) VALUES ('"  . $mysql->escape( $name ) . "' , '" . intval( $age ) . "' , NOW() ) ";
    $mysql->runSql($sql);
    if ($mysql->errno() != 0)
    {
        die("Error:" . $mysql->errmsg());
    }

    $mysql->closeDb();
    ?>
 
SAE的PHP Runtime环境提供了标准的MySQL,MySQLI和PDO模块,三个模块都由MYSQLND驱动,支持所有MYSQL的特性,您可以使用您习惯的方式来操作数据库。如果您没有使用SaeMysql类操作数据库，可以使用以下预定义常量进行连接： ::

          用户名　 :  SAE_MYSQL_USER
          密　　码 :  SAE_MYSQL_PASS
          主库域名 :  SAE_MYSQL_HOST_M
          从库域名 :  SAE_MYSQL_HOST_S
          端　　口 :  SAE_MYSQL_PORT
          数据库名 :  SAE_MYSQL_DB
   

使用方法，以mysql模块为例: ::

    // 连主库
    $link=mysql_connect(SAE_MYSQL_HOST_M.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS);

    // 连从库
    // $link=mysql_connect(SAE_MYSQL_HOST_S.':'.SAE_MYSQL_PORT,SAE_MYSQL_USER,SAE_MYSQL_PASS);

    if($link)
    {
        mysql_select_db(SAE_MYSQL_DB,$link);
        //your code goes here
    }

.. note:: PDO不支持host:port这种写法,请使用PDO标准写法

配额
==========

.. include:: ../quotas/mysql.rst


