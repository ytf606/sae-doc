.. include:: ../services/storage.rst

API使用手册
============

.. 这里写API Reference或者贴对应服务的API Reference的链接

使用示例
============

.. 这里贴示例


File Wrap
=============

Java Runtime对File相关类进行了包装，这样开发者可以直接调用Java的File相关的类来操作Storage。

使用File操作SAE的Storage服务非常简单，和普通的IO操作没什么差别，唯一不同的是你只需要在File相关类的filePath的构造参数前加上 `saestor://domain` (区分大小写)，其中 `saestor://` 是固定字符串，而domain则是Storage对应的域名。 ::
 

  //向域名为domain写入一个test.txt文件，文件内容为“测试内容”
  FileOutputStream outputStream = new FileOutputStream("saestor://domain/test.txt");
  Writer writer = new OutputStreamWriter(outputStream);
  writer.write("测试内容");
  writer.close();

  //读storage中域名为domain，文件名为test.txt的文件
  FileInputStream inputStream = new FileInputStream("saestor://domain/test.txt");
  Reader reader = new InputStreamReader(inputStream);
  StringBuilder filetext = new StringBuilder();
  int tempchar;
  while ((tempchar = reader.read()) != -1) {
     filetext.append((char) tempchar);
  }
  reader.close();
