# 文档中心

## 概述

本文档使用 **sphinx-doc** 作为文档工具，所有的文档源码位于 **.rst** 文件中，使用reStructureText
格式，关于这个格式，可以参考文档： 

+ 官方文档：http://sphinx-doc.org/rest.html
+ 中文翻译：http://sphinx-doc-zh.readthedocs.org/en/latest/rest.html

各种编辑器对rst格式的语法高亮支持：http://docutils.sourceforge.net/docs/user/links.html#id6

另外在编写文档时务必注意以下几点：

+ 文件名只能包含英文字母和-，因为文件名为出现在最终文档的URL中。
+ 如果是中文段落或者是中英文混合段落，请使用中文半角标点符号。
+ 所有段落请顶格写，不要前面空两格。
+ 图片请放在images目录下，请使用human readable的文件名。
+ 如非必要，不要引用外部图片资源。
+ 其它静态资源放在static目录下。
+ 有些文档可能写得太烂，你可以选择Refactor或者Rewrite。
+ 一些概念名词的标准化。详细请先阅读tutorial/concept.rst文件。
+ Windows用户请使用Notepad++之类的编辑器，并将文档编码设置为UTF-8。
+ 编写FAQ的时候，如果是文档已有描述的，使用交叉引用链接到目标文档，禁止直接拷贝粘贴。

所谓物以类聚，人以群分，什么样的平台也就吸引什么样的开发者，文档作为SAE服务属性中仅次于服务质量
的属性，请各位务必认真对待。

## 如何编译文档

1.安装编译工具，以Ubuntu为例：

```
sudo apt-get install python-pip
sudo pip install sphinx

```

2.在本文件所在目录中执行 **make html** 编译文档，生成的文档位于 **_build/html** 目录下。

## 如何修改文档

1.以你的staff邮箱前缀为分支名创建一个新的分支并修改提交。

```
git checkout git@git.sae.sina.com.cn:sae/documentation.git
git checkout -b <your-staff-email-prefix>
# 修改
git commit -m <commit message>
# 修改
git commit -m <commit message>
git push
```

2.在gitlab中向项目的master发起一个merge request。让项目的master将你的分支merge到master中。

**提交时请使用中文写提交信息，并在提交信息中说清楚修改了什么**
