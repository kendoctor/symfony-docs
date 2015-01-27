.. index::
   single: 安装


安装与配置 Symfony
==================================

这章节的目的是让你热身一下，教你如何使用Symfony技术，创建一个可运行的Web应用。
幸运的是，Symfony提供具备一定功能的起步项目“安装分布包”，你可以很快下载它并进行项目开发。

.. tip::

    如果你希望创建新项目之后，使用版本控制来管理项目，查看 `使用版本控制`_ 获取相关指导。

.. _installing-a-symfony2-distribution:


安装Symfony分布包
---------------------------------

.. tip::

    首先， 检查你是否安装和配置了PHP Web服务器（譬如Apache）。更多Symfony安装需求，
    查看 :doc:`requirements reference </reference/requirements>`。

Symfony提供的“安装分布包”，拥有完整功能的Web应用，其中包含Symfony核心库，一套实用的Bundles模块，
一个完善的目录结构和一些默认配置。下载一份Symfony安装分布包，你就拥有了一个新项目，拥有了一个新应用，接着就开始你的开发旅程了。


在 `http://symfony.com/download`_ 页面进行Symfony下载。
在此页，可以下载 *Symfony标准版本*，这是Symfony主要的安装分布包。 
提供两种开始Symfony项目的方法:

方法 1） Composer
~~~~~~~~~~~~~~~~~~

`Composer`_ 是一种PHP关联库管理工具，你可以用它来下载Symfony标准版本。

`下载 Composer`_ 到你本机， 如果你安装了Curl命令，那么很简单：

.. code-block:: bash

    $ curl -s https://getcomposer.org/installer | php

.. note::

    如果你的计算机无法安装 Composer，请到 `https://getcomposer.org`_ 学习如何安装。


Composer是一个PHAR执行文件， 使用它可以下载标准安装分布包：

.. code-block:: bash

    $ php composer.phar create-project symfony/framework-standard-edition /path/to/webroot/Symfony '~2.6'

.. tip::

    如果你想下载第三方类库包快一点，可以在Composer命令后添加 ``--prefer-dist`` 选项。

.. tip::

    添加 ``-vvv`` 选项可以查看Composer运行日志信息 － 对于网络延迟的朋友非常有帮助，没有日志信息有时候你不知道什么原因命令一直没反应。

实用Composer下载标准安装分布包，会同时下载项目所有相关的第三方类库，命令可能需要花上几分钟。
安装完后， 可以得到如下的一个目录结构：

.. code-block:: text

    path/to/webroot/ <- 你的Web服务器根目录（ 通常是 htdocs 或者是 public)
        Symfony/ <- 新项目目录夹
            app/
                cache/
                config/
                logs/
            src/
                ...
            vendor/
                ...
            web/
                app.php
                ...


方法 2) 下载一个文档包
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

下载一个标准版本的文档包。有两个选择：

* 下载 ``.tgz`` 或者 ``.zip`` 文档包 - 两者都一样，适合你的下你的：

* 下载没有第三方类库的分布包，下载之后用 Composer 进行更新就可以安装第三方类库。

下载一种压缩类型的文档包到你的Web服务器的Web根目录并且解压。 在UNIX控制台中，
使用以下命令来解压（``###``替换成你实际下载的文件名）

.. code-block:: bash

    # for .tgz file
    $ tar zxvf Symfony_Standard_Vendors_2.6.###.tgz

    # for a .zip file
    $ unzip Symfony_Standard_Vendors_2.6.###.zip

如果你下载了“无第三方类库”的文档包，阅读下面的内容。

.. note::

    默认的目录夹结构可以重新配置， 查看 :doc:`/cookbook/configuration/override_dir_structure` 获取更多信息。

所有公共访问文件和处理请求的前端控制器，存放在 ``Symfony/web/`` 目录中。 因此，
假设解压文档包到Web服务器或者虚拟主机的文档根目录，
访问 `http://localhost/Symfony/web/`` 进入Web应用首页


.. note::

    接下来的例子，假设你没有配置过文档根目录，
    那么所有访问从 ``http://localhost/Symfony/web/`` 开始。

.. _installation-updating-vendors:

更新第三方类库
~~~~~~~~~~~~~~~~

到此，你下载了一个功能完整的Symfony起始项目，基于它开发你自己的应用。Symfony项目
依赖一系列的外部类库。这些类库，通过 `Composer`_ 下载到项目目录 ``vendor/`` 中。

根据如何下载的Symfony，需要，也可能不需要立马更新第三方类库。但是，更新第三方类库更加保险，
能确保所需要的第三方类库都存在。

第一步： 获取 `Composer`_ （强大的新兴PHP类库包管理工具和系统）

.. code-block:: bash

    $ curl -s http://getcomposer.org/installer | php

确保下载 ``composer.phar`` 到与 ``composer.json`` 相同的目录中（默认就在Symfony项目根目录中）


第二步： 安装第三方类库

.. code-block:: bash

    $ php composer.phar install

此命令下载所有必要的第三方类库，包括Symfony自身，到 ``vendor/`` 目录中。

.. note::

    如果你没有安装 ``curl`` 命令， 可以到 http://getcomposer.org/installer 下载
    ``installer`` 文件， 把此文件放到你的项目目录中，并且运行

    .. code-block:: bash

        $ php installer
        $ php composer.phar install

.. tip::

    当运行 ``php composer.phar install`` 或者 ``php composer.phar update`` 结束之前，
    Composer会执行命令，来清理缓存和安装资源文件。 默认情况下，资源文件会复制到 ``web`` 目录中。

    不靠复制Symfony资源文件，也可以靠创建快捷链接（symlinks），只要你的操作系统支持此特性。
    为了创建快捷链接，需要在 composer.json 文件中添加一个节点 ``extra``， 节点中配置健 
    ``symfony-assets-install`` 和 值 ``symlink``:

    .. code-block:: json

        "extra": {
            "symfony-app-dir": "app",
            "symfony-web-dir": "web",
            "symfony-assets-install": "symlink"
        }

    如果健值用 ``relative`` 替换 ``symlink``，命令会生成相对地址的快捷链接。


配置与安装
~~~~~~~~~~~~~~~~~~~~~~~

到此，所有需要的第三方类库已经安装在 ``vendor/`` 目录中。 此项目在 ``app/` 拥有默认的安装配置，
在 ``src/`` 中有一些样例代码。

Symfony给你一个可视化的配置测试页面，帮助你核实Web服务器与PHP配置是否满足Symfony项目运行要求。
使用以下地址来检查你的配置：

.. code-block:: text

    http://localhost/config.php

如果存在问题，纠正后继续。

.. _book-installation-permissions:

.. sidebar:: 配置访问权限

    通常的一个问题是 ``app/cache`` 和 ``app/logs`` 目录必须为Web服务器和开发用户，提供读写能力。
    在UNIX系统上，Web服务器账号与当前操作用户账号不同，你可以运行以下命令一次性解决访问权限问题。

    **1. 在支持 chmod +a 的系统上使用ACL**

    许多系统允许你使用 ``chmod +a`` 命令。 首先尝试此命令，如果碰到错误，尝试后面的方法。
    通过命令来获取Web服务器账户名，并存放到 ``HTTPDUSER`` 变量中：

    .. code-block:: bash

        $ rm -rf app/cache/*
        $ rm -rf app/logs/*

        $ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
        $ sudo chmod +a "$HTTPDUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
        $ sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs


    **2.在不支持 chmod +a 的系统上使用ACL**

    有些系统不支持 ``chmod +a``， 但支持其他方法 ``setfacl``。需要在当前磁盘分区上 `打开ACL支持`_，
    再安装 setfacl， 最后才能使用此命令（譬如Ubuntu系统）。 
    通过命令来获取你Web服务器账户名，并存放到``HTTPDUSER``变量中：

    .. code-block:: bash

		$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
		$ sudo setfacl -R -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
		$ sudo setfacl -dR -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs

    如果此方法不行，尝试追加一个 ``-n`` 选项。

    **3. 不使用ACL **

    如果无法改变目录的ACL，考虑改变umask，使缓存和日志目录具备组读写（这需要Web服务器账户与操作用户账户是否在同一个组）。
    为了做到这点，在 ``app/console``, ``web/app.php`` 和 ``web/app_dev.php`` 文件开始行，添加以下内容::

        umask(0002); // 访问权限变为 0775

        // 或者

        umask(0000); // 访问权限便问 0777

    注意使用ACL来获取目录访问权限是首先推荐的方法，因为umask不保证线程安全。

    **4. 使用PHP内置的Web服务器作为开发环境**

    在开发环境中可使用内置的PHPWeb服务器，允许Web服务器账户当前操作用户账户相同。这样
    也就解决了访问权限的问题。

    .. code-block:: bash

        $ php app/console server:start

    .. seealso::

        :doc:`在《技术手册》一书中 </cookbook/web_server/built_in>`， 阅读更多关于内置服务器的内容. 

    **5. 让CLI和Web服务器使用相同账户**

    在开发环境中，常规的做法是CLI与Web服务器使用相同账户，这样可以避免在建立新项目时出现任何权限问题。
    这只需要修改Web服务器配置，（譬如，apache的 httpd.conf或者appache2.conf配置） 并设置其账户与CLI相同（譬如，Apache，更新用户和用户组值）。


当一切都完成后，点击 "Go to the Welcome page" 访问真正的Symfony页面：

.. code-block:: text

    http://localhost/app_dev.php/

Symfony会欢迎你并祝贺你完成了以上艰巨的任务。

.. image:: /images/quick_tour/welcome.png

.. tip::

    为了拥有更好看的、更短的url地址，你需要把 ``Symfony/web/`` 配置为文档根目录。
    虽然在开发时，没有必要，但在发布环境中需要这么做，防止浏览者访问不该访问的目录和文件。
    如何配置Web服务器根目录，请阅读 :doc:`/cookbook/configuration/web_server_configuration`
    或者 查阅Web服务器官方文档:
    `Apache`_ | `Nginx`_ .


开始开发
---------------------

到此为止，一个功能完整的Symfony应用安装完毕，你可以开始开发了！这个分布包含有一些样例代码 －
检查在分布包中的 ``README.md`` 文档（以文本文件打开），了解有哪些样例代码。

如果你是Symfony新手， 查看文档 ":doc:`page_creation`"， 教你如何创建页面， 改变配置，
以及你想在新项目中想做的一切。

确保查看 :doc:`Cookbook </cookbook/index>`， 里面包含一系列教你使用Symfony如何解决各种特定问题的文章。

.. note::

    如果你想在分布包中去除样例代码， 请看一下《技术手册》中的一篇文章  ":doc:`/cookbook/bundles/remove`"


使用版本控制
--------------------

如果你正在使用版本控制系统，如 ``Git`` 或者 ``Subversion``， 如同往常一样，可以建立版本控制，
提交项目内容到版本控制系统。 Symfony标准版本 *是* 你新项目的开发起点。

针对关于如何使用Git，进行版本控制你的项目的专项指导， 查看 :doc:`/cookbook/workflow/new_project_git`.

忽略 ``vendor/`` 目录
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

如果下载的是“无第三方类库包”的文档安装包，你可以安全地忽略 ``vendor/`` 目录中的内容，
不会对此目录中得内容进行版本控制。 通过 ``Git``， 创建一个 ``.gitignore`` 文件，
在里面添加以下内容：

.. code-block:: text

    /vendor/

这样，vendor目录不会被版本控制监管。 这样比较好（实际上，非常棒）因为其他人只需要 clone 和 check out 项目之后，
运行个命令 ``php composer.phar install`` 就可以获得所有需要的项目关联文件。

.. _`打开ACL支持`: https://help.ubuntu.com/community/FilePermissionsACLs
.. _`http://symfony.com/download`: http://symfony.com/download
.. _`Git`: http://git-scm.com/
.. _`GitHub Bootcamp`: http://help.github.com/set-up-git-redirect
.. _`Composer`: http://getcomposer.org/
.. _`下载 Composer`: http://getcomposer.org/download/
.. _`Apache`: http://httpd.apache.org/docs/current/mod/core.html#documentroot
.. _`Nginx`: http://wiki.nginx.org/Symfony
.. _`Symfony安装页`:    http://symfony.com/download
