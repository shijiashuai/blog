title: '怎样在Ubuntu14.04上安装Linux,Apache,MySQL,PHP(LAMP)'
id: 7
categories:
  - 教程
date: 2014-12-21 22:59:28
tags:
 - Linux
 - Apache
 - MySQL
 - PHP
---

## 介绍

一个“LAMP”栈是一组通常安装在一起的开源软件，它可以让一台服务器运行动态网站和web应用。LAMP这个术语实际上是一个首字母缩略词，他们分别代表了Linux操作系统，Apache web服务器，存储网站数据的MySQL数据库和处理动态内容的PHP。

在这个教程里，我们将会在Ubuntu 14.04上安装LAMP栈，Ubuntu将会实现我们的第一个需求：一个Linux操作系统。

<!--more-->

- ### 第一步——安装Apache

    Apache 是世界上最流行的Web服务器，它是一个不错的选择。

    我们可以利用Ubuntu的包管理器（package manager）——apt轻易地安装Apache。包管理器可以让我们很轻松地从Ubuntu维护的仓库（repository）中安装大多数软件。

    要实现我们的目标，我们可以从键入以下命令开始：
    ```plain
    sudo apt-get update
    sudo apt-get install apache2
    ```
    由于我们使用了sudo命令，这些操作将会以root权限执行。系统会让你键入当前用户的密码来核对你的意图。

    然后，你的Web服务器就安装完成了。

    你可以立即检查一下这些事情是不是真的做好了，只要在浏览器中输入你的服务器的地址（译者注：如果是本地服务器，输入`http://localhost`即可）
    ```plain
    http://your_server_IP_address
    ```
    你将会看到一个默认的Ubuntu 14.04 Apache Web服务器页面，这个页面是用来提供信息和测试的。它应该看起来像这样：

    ![default_apache](/img/default_apache.png)

    如果你看到了这个页面，那么你的web服务器现在已经被正确安装了。

- ### 第二步——安装MySQL

    既然我们已经安装好web服务器并且成功运行，是时候安装MySQL了。MySQL是数据库管理系统。从根本上说，我们的网站能够存储数据的数据库就是由它组织和提供的。

    我们再次使用`apt`来获取和安装软件。这一次我们还会安装一些其他的辅助包，这些包会让我们的组件之间相互交流：
    <pre>sudo apt-get install mysql-server php5-mysql</pre>
    注意：这一次你不必在指令之前运行`sudo apt-get update`，这是因为我们在上面安装Apache的时候已经运行过一次这个指令了，所以我们电脑中的包索引应该已经是最新的了。

    在安装过程中，你的系统会要求你输入并确认MySQL的root用户密码。这是一个MySQL的管理用户，他拥有最高的权限。你要像思考这个系统的root用户密码一样认真考虑它一下（不过这次你要配置的是MySQL的特定账户）

    当安装完成的时候，我们需要运行一些额外的命令来让我们的MySQL环境更安全。

    首先，我们需要告诉MySQL创建它的数据库目录结构（database directory structure），MySQL会把数据存放在这里。你可以键入以下命令：
    <pre>sudo mysql_install_db</pre>
    然后，我们想要运行一个简单的安全脚本来移除一些危险的默认值，并且稍稍加锁我们的数据库系统。运行下面的命令来启动这个互动脚本：
    <pre>sudo mysql_secure_installation</pre>
    系统会要求你输入你设置的MySQL的root账户的密码，接下来，它会问你是否想要修改这个密码。如果你对当前的密码感到满意，对这个提示说“n”或者“no”吧。

    对于剩下的问题，你可以按"ENTER"来接受每个建议的默认值，这会溢出一些简单的用户和数据库，关闭远程root登录，并且加载这些新规则，所以MySQL会立即接受我们所做的这些更改。

    到这里，你的数据库系统已经成功建立，我们可以继续了。

- ### 第三步——安装PHP

    PHP会运行一些代码来显示动态内容，这也是我们配置的一部分。它会运行脚本，连接到MySQL数据库服务器来获取信息，并且把这些处理过的内容递给我们的web服务器显示。

    我们可以再一次利用`apt`系统来安装我们的组件。我们也会先安装一些辅助组件：
    <pre>sudo apt-get install php5 libapache2-mod-php5 php5-mcrypt</pre>
    这将会直接安装PHP，马上我们就会测试它。

    在大多数情况下，当一个目录被请求时，我们想修改Apache提供文件的方式。目前，如果一个用户向服务器请求一个目录，Apache会寻找一个称为`index.html`的文件。我们想告诉我们的web服务器让它更偏好PHP文件，所以我们想让Apache首先寻找`index.php`文件。

    要做到这些，键入这个命令来用一个文本编辑器打开dir.conf文件，而且是以管理员权限打开的：
    <pre>sudo nano /etc/apache2/mods-enabled/dir.conf</pre>
    它将看起来像这样:
    ```plain
        <IfModule mod_dir.c>
            DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
                                                         ⬆️here
        </IfModule>
    ```
    我们想把那个箭头所指的的`index.php`移到DirectoryIndex后面去，像这样：
    ```plain
        <IfModule mod_dir.c>
            DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
                           ⬆️move to here
        </IfModule>
    ```
    当你做完了这些时，用`CTRL+X`快捷键来保存并退出这个文件。你将要输入“Y”来确认这次保存，并且按下`ENTER`来确认保存的位置。

    在这之后，我们需要重启Apache web服务器来让它识别我们的改动。你可以输入以下命令来重启：
    <pre>sudo service apache2 restart</pre>
    至此，你的LAMP栈就已经安装并配置完成了，让我们来测试一下PHP。

- ### 第四步——在web服务器上测试PHP的处理

    为了测试我们的系统是否成功配置了PHP，我们可以创建一个非常简单的PHP脚本。

    我们会把这个脚本叫做`info.php`。为了让Apache正确地找到并且提供这个文件，它必须被放在一个叫做`web root`的特定目录里。

    在Ubuntu 14.04中，这个目录位于/var/www/html/。我们可以输入一下命令来在这个目录中创建一个文件：
    <pre>sudo nano /var/www/html/info.php</pre>
    这将会打开一个空文件，我们想向其中添加以下的有效的PHP代码:
    ```php
        <?php
        phpinfo();
        ?>
    ```
    当你做完了这些，保存并关闭这个文件。

    现在我们可以测试我们的web服务器是不是能够正确地显示由PHP脚本生成的内容了。为此，我们只需在我们的浏览器中访问这个页面。你会再次用到你的公网IP地址。

    我们要访问的地址是：
    ```plain
    http://your_server_IP_address/info.php
    ```
    你要到达的这个页面看起来会像这样：

    ![default_php](/img/default_php.png)

    这个页面从PHP这个方面给了你关于你服务器的信息。它在调试和确认你的设置已经正确应用时很有用。

    如果你看到了上面的页面，那么你的PHP是在如期工作着的。

    也许你在这之后想要移除这个文件，因为它实际上把关于你服务器的信息给了未授权的使用者。为此，你可以输入这些：
    <pre>sudo rm /var/www/html/info.php</pre>
    如果你以后想要获取这些信息，你总是可以重新创建这个页面。

# 总结

**既然你已经成功安装了LAMP栈，下一步你将会有很多选择。从根本上说，你已经安装好了能够让你在服务器上安装许多种站点和网页软件的平台。**

原文链接：
[https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-14-04)