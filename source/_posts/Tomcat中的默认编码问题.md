title: Tomcat中的默认编码问题
id: 93
categories:
  - Programming
date: 2015-03-18 11:50:38
tags:
 - Tomcat
 - Java
---

前段时间做了一个小项目，需要获取一个网页的HTML源码，然后在服务器上保存交给另一个数学模型处理。模型在单独测试的时候没出现什么问题，但是放到Tomcat中就突然出现了莫名其妙的错误。因为需要处理中文，经师兄提醒可能是编码的问题，于是设断
点到程序中查看，果然是，所有中文都变成了乱码。

<!--more-->

首先，我想看看本地和Tomcat中的默认编码有什么不同，于是写了一行代码
```java
Charset.defaultCharset().displayName();
```
果然，在本地的输出是`UTF-8`，在tomcat中的输出是`US-ASCII`。

既然在本地测试可以，说明读写文件的时候只要指定正确的编码方式就好了，而不是使用系统默认的。

指定方式如下，假定要读写的文件是file，要指定的编码是`UTF-8`
```java
Scanner = new Scanner(file, "UTF-8");
BufferedReader reader = new Buffered(new InputStreamReader(file, "UTF-8"));
PrintWriter writer = new PrintWriter(new OutputStreamWriter(new FileOutputStream(saveFile), "UTF-8"));
```