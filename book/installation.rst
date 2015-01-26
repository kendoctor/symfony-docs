.. index::
   single: Installation


安装与配置 Symfony
==================================

Installing and Configuring Symfony
==================================


这章节的目标是让你起步并运行建立在Symfony上的应用程序。幸运的是，Symfony提供“分布”， 它是具有功能性的Symfony起步项目，你可以立刻下载它和开始开发。

The goal of this chapter is to get you up and running with a working application
built on top of Symfony. Fortunately, Symfony offers "distributions", which
are functional Symfony "starter" projects that you can download and begin
developing in immediately.

.. tip::

    如果你在寻找指导，关于如何更好的创建一个新项目并且通过版本控制保存它，查看 `使用版本控制 `_

    If you're looking for instructions on how best to create a new project
    and store it via source control, see `Using Source Control`_.

.. _installing-a-symfony2-distribution:


安装Symfony分布
---------------------------------

Installing a Symfony Distribution
---------------------------------

.. tip::

    首先， 检查你是否安装和配置了PHP Web服务器（譬如Apache）。更多关于Symfony需求信息，
    查看:doc:`requirements reference </reference/requirements>`。

    First, check that you have installed and configured a Web server (such
    as Apache) with PHP. For more information on Symfony requirements, see the
    :doc:`requirements reference </reference/requirements>`.

Symfony打包“分布”，它是一个全功能的应用程序，包含Symfony核心库，一套有用的Bundles，一个
合理的目录结构和一些默认配置。当你下载一份Symfony分布时，你在下载一个功能性应用程序骨架，它
可以立刻使用于开发你的应用程序。

Symfony packages "distributions", which are fully-functional applications
that include the Symfony core libraries, a selection of useful bundles, a
sensible directory structure and some default configuration. When you download
a Symfony distribution, you're downloading a functional application skeleton
that can be used immediately to begin developing your application.


通过开始访问Symfony `http://symfony.com/download`_ 下载页面。
在此页，你可以看到 *Symfony标准版本*，这是一个主Symfony分布。 这里有两种方法开始你的项目：
Start by visiting the Symfony download page at `http://symfony.com/download`_.
On this page, you'll see the *Symfony Standard Edition*, which is the main
Symfony distribution. There are 2 ways to get your project started:


选择 1） Composer
~~~~~~~~~~~~~~~~~~

Option 1) Composer
~~~~~~~~~~~~~~~~~~

`Composer`_ 是为PHP打造的关联管理库，你可以用它来现在Symfony标准版本。

`Composer`_ is a dependency management library for PHP, which you can use
to download the Symfony Standard Edition.

开始 `下载 Composer`_ 到你本机。如果你已经安装了Curl，那么就如此简单：

Start by `downloading Composer`_ anywhere onto your local computer. If you
have curl installed, it's as easy as:

.. code-block:: bash

    $ curl -s https://getcomposer.org/installer | php

.. note::

    如果你的计算机无法使用Composer，那么你查看一些推荐，当运行这个命令的时候。 
    按照推荐安装 Composer 至正常可用。

    If your computer is not ready to use Composer, you'll see some recommendations
    when running this command. Follow those recommendations to get Composer
    working properly.

Composer是一个PHAR执行文件， 你使用它可以下载标准分布。

Composer is an executable PHAR file, which you can use to download the Standard
Distribution:

.. code-block:: bash

    $ php composer.phar create-project symfony/framework-standard-edition /path/to/webroot/Symfony '~2.6'

.. tip::

    为了能够更快的下载第三方文件， 可以在Composer命令后添加 ``--prefer-dist`` 选项。

    To download the vendor files faster, add the ``--prefer-dist`` option at
    the end of any Composer command.

.. tip::

    添加 ``-vvv`` 选项 查看 Composer运行过程的日志信息 － 这个非常有帮助，对于延迟严重的网络，
    看起来没有什么发生。

    Add the ``-vvv`` flag to see everything that Composer is doing - this is
    especially useful on a slow connection where it may seem that nothing is
    happening.


这个Composer会下载标准分布其所有相关的第三方类库，命令可能需要花上几分钟。
当它完成之后， 你应该可以得到如下的一个目录结构：

This command may take several minutes to run as Composer downloads the Standard
Distribution along with all of the vendor libraries that it needs. When it finishes,
you should have a directory that looks something like this:

.. code-block:: text

    path/to/webroot/ <- 你的Web服务器目录夹（ 通常是 htdocs 或者是 public)
    path/to/webroot/ <- your web server directory (sometimes named htdocs or public)
        Symfony/ <- 新项目目录夹
        Symfony/ <- the new directory
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


选择 2) 现在一个文档包
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Option 2) Download an Archive
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


你也可以下载一个标准版本的文档包。这里，你可以有两个选择：

You can also download an archive of the Standard Edition. Here, you'll
need to make two choices:

* 下载 ``.tgz`` 或者 ``.zip`` 文档包 - 两者都一样，下载你觉得合适的：

* Download either a ``.tgz`` or ``.zip`` archive - both are equivalent, download
  whatever you're more comfortable using;

* 下载没有第三方类库的分布。如果你打算之后使用 Composer 来更新安装第三方类库。

* Download the distribution with or without vendors. If you're planning on
  using more third-party libraries or bundles and managing them via Composer,
  you should probably download "without vendors".

下载一种文档包到你的Web服务器的 Web 根目录并且解压缩。 在UNIX控制台中，
可以使用以下命令来解压（``###``替换成你实际下载文件名）

Download one of the archives somewhere under your local web server's root
directory and unpack it. From a UNIX command line, this can be done with
one of the following commands (replacing ``###`` with your actual filename):

.. code-block:: bash

    # for .tgz file
    $ tar zxvf Symfony_Standard_Vendors_2.6.###.tgz

    # for a .zip file
    $ unzip Symfony_Standard_Vendors_2.6.###.zip

如果你已经下载了 “无第三方类库”的文档包，你需要阅读下面章节内容。

If you've downloaded "without vendors", you'll definitely need to read the
next section.

.. note::

    你可以很容易覆盖默认的目录夹结构。 查看 :doc:`/cookbook/configuration/override_dir_structure` 更多信息。

    You can easily override the default directory structure. See
    :doc:`/cookbook/configuration/override_dir_structure` for more
    information.

所有公共文件和前端控制器，在Symfony应用处理来访请求存放在 ``Symfony/web/`` 目录中。 因此，
假设你解压文档包至你的Web 服务器或者虚拟主机的文档根目录，你的应用访问URLs地址将开始于 `http://localhost/Symfony/web/``。

All public files and the front controller that handles incoming requests in
a Symfony application live in the ``Symfony/web/`` directory. So, assuming
you unpacked the archive into your web server's or virtual host's document root,
your application's URLs will start with ``http://localhost/Symfony/web/``.

.. note::

    下面的例子假设你没有接触过文档根配置，因此所有的URLs开始于 ``http://localhost/Symfony/web/``

    The following examples assume you don't touch the document root settings
    so all URLs start with ``http://localhost/Symfony/web/``

.. _installation-updating-vendors:

更新第三方类库
~~~~~~~~~~~~~~~~

Updating Vendors
~~~~~~~~~~~~~~~~

在此时，你已经下载了一个功能齐全的Symfony项目，在其中你可以开始开发你自己的应用。Symfony项目
依赖于一系列的外部类库。这些类库下载到你项目的目录 ``vendor/`` 中，通过一个类库叫`Composer`_.

At this point, you've downloaded a fully-functional Symfony project in which
you'll start to develop your own application. A Symfony project depends on
a number of external libraries. These are downloaded into the ``vendor/`` directory
of your project via a library called `Composer`_.

根据你是如何下载Symfony，你可以或者可以不需要立马更新第三方类库。但是，更新第三方类库总是相对保险，
保证所需要的第三方类库存在。

Depending on how you downloaded Symfony, you may or may not need to update
your vendors right now. But, updating your vendors is always safe, and guarantees
that you have all the vendor libraries you need.

第一步： 获取 `Composer`_ （强大的新兴PHP类库包管理系统）

Step 1: Get `Composer`_ (The great new PHP packaging system)

.. code-block:: bash

    $ curl -s http://getcomposer.org/installer | php

确保在和``composer.json``相同的目录夹中下载 ``composer.phar``（默认就在Symfony项目根目录中）

Make sure you download ``composer.phar`` in the same folder where
the ``composer.json`` file is located (this is your Symfony project
root by default).

第二步： 安装第三方类库

Step 2: Install vendors

.. code-block:: bash

    $ php composer.phar install

这个命令下载所有必要的第三方类库 － 包括Symfony自身 － 到 ``vendor/`` 目录夹中。

This command downloads all of the necessary vendor libraries - including
Symfony itself - into the ``vendor/`` directory.

.. note::

    如果你没有安装 ``curl`` 命令， 也可以在 http://getcomposer.org/installer 下载
    ``installer`` 文件， 把此文件放到你的项目目录中，并且运行

    If you don't have ``curl`` installed, you can also just download the ``installer``
    file manually at http://getcomposer.org/installer. Place this file into your
    project and then run:

    .. code-block:: bash

        $ php installer
        $ php composer.phar install

.. tip::

    当运行 ``php composer.phar install`` 或者 ``php composer.phar update`` 时，
    Composer将执行安装或者更新之后的命令来清理缓存和安装资源文件。 默认情况下，资源文件会复制到
    ``web`` 目录夹中。

    When running ``php composer.phar install`` or ``php composer.phar update``,
    Composer will execute post install/update commands to clear the cache
    and install assets. By default, the assets will be copied into your ``web``
    directory.

    不依赖复制Symfony资源文件，你也可以创建快捷链接（symlinks），只要你的操作系统支持此特性。
    通过创建快捷链接，在composer.json文件中添加一个节点 ``extra``， 节点中配置健 
    ``symfony-assets-install`` 和 值 ``symlink``:

    Instead of copying your Symfony assets, you can create symlinks if
    your operating system supports it. To create symlinks, add an entry
    in the ``extra`` node of your composer.json file with the key
    ``symfony-assets-install`` and the value ``symlink``:

    .. code-block:: json

        "extra": {
            "symfony-app-dir": "app",
            "symfony-web-dir": "web",
            "symfony-assets-install": "symlink"
        }

    如果健值用 ``relative`` 替换 ``symlink``，命令会生成相对地址的快捷链接。

    When passing ``relative`` instead of ``symlink`` to symfony-assets-install,
    the command will generate relative symlinks.

配置与安装
~~~~~~~~~~~~~~~~~~~~~~~

Configuration and Setup
~~~~~~~~~~~~~~~~~~~~~~~

此刻，所有需要的第三方类库已经安装在 ``vendor/`` 目录夹中。 此项目在``app/`拥有默认的安装配置，
在 ``src/`` 中有一些样例代码。

At this point, all of the needed third-party libraries now live in the ``vendor/``
directory. You also have a default application setup in ``app/`` and some
sample code inside the ``src/`` directory.

Symfony项目拥有一个可视化的配置测试页面，帮助你确定Web服务器和PHP配置是否满足Symfony项目运行环境。
适用一下地址来检查你的配置：

Symfony comes with a visual server configuration tester to help make sure
your Web server and PHP are configured to use Symfony. Use the following URL
to check your configuration:

.. code-block:: text

    http://localhost/config.php

如果存在问题，在继续之前纠正再说。

If there are any issues, correct them now before moving on.

.. _book-installation-permissions:

.. sidebar:: 配置访问权限

    通常的一个问题是``app/cache``和``app/logs`` 目录夹必须为Web服务器和开发用户提供读写能力。
    在UNIX系统上，Web服务器账号与当前操作账号不同，你可以运行以下命令一次性解决访问权限问题。

    One common issue is that the ``app/cache`` and ``app/logs`` directories
    must be writable both by the web server and the command line user. On
    a UNIX system, if your web server user is different from your command
    line user, you can run the following commands just once in your project
    to ensure that permissions will be setup properly.

    **1. 使用ACL并支持 chmod +a的系统**

    **1. Using ACL on a system that supports chmod +a**

    许多系统允许你使用 ``chmod +a`` 命令。 首先尝试此命令，如果碰到错误，尝试后面的方法。
    通过命令来获取你Web服务器账户名，并存放到``HTTPDUSER``变量中：

    Many systems allow you to use the ``chmod +a`` command. Try this first,
    and if you get an error - try the next method. This uses a command to
    try to determine your web server user and set it as ``HTTPDUSER``:

    .. code-block:: bash

        $ rm -rf app/cache/*
        $ rm -rf app/logs/*

        $ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
        $ sudo chmod +a "$HTTPDUSER allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs
        $ sudo chmod +a "`whoami` allow delete,write,append,file_inherit,directory_inherit" app/cache app/logs


    **2.使用ACL但不支持chmod +a的系统**

    **2. Using ACL on a system that does not support chmod +a**

    有些系统不支持``chmod +a``， 但支持其他使用方法 ``setfacl``。 需要在当前磁盘分区上 `打开ACL支持`_ 并且在使用方法之前 安装 setfacl（譬如Ubuntu系统）。 
    通过命令来获取你Web服务器账户名，并存放到``HTTPDUSER``变量中：

    Some systems don't support ``chmod +a``, but do support another utility
    called ``setfacl``. You may need to `enable ACL support`_ on your partition
    and install setfacl before using it (as is the case with Ubuntu). This
    uses a command to try to determine your web server user and set it as
    ``HTTPDUSER``:

    .. code-block:: bash

		$ HTTPDUSER=`ps aux | grep -E '[a]pache|[h]ttpd|[_]www|[w]ww-data|[n]ginx' | grep -v root | head -1 | cut -d\  -f1`
		$ sudo setfacl -R -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs
		$ sudo setfacl -dR -m u:"$HTTPDUSER":rwX -m u:`whoami`:rwX app/cache app/logs

    如果上面方法不行，尝试追加一个 ``-n`` 选项。

    If this doesn't work, try adding ``-n`` option.

    **3. 不支持ACL的系统 **
    **3. Without using ACL**

    如果你改变目录文件的ACL，你可以改变umask，因此缓存和日志目录夹组读写（这依赖与Web服务器账户与你操作账户是否在同一个组）。为了达到此目的，在文件 ``app/console``,
    ``web/app.php`` 和 ``web/app_dev.php`` 开始添加以下内容::

    If you don't have access to changing the ACL of the directories, you will
    need to change the umask so that the cache and log directories will
    be group-writable or world-writable (depending if the web server user
    and the command line user are in the same group or not). To achieve
    this, put the following line at the beginning of the ``app/console``,
    ``web/app.php`` and ``web/app_dev.php`` files::

        umask(0002); // 访问权限变为 0775

        // 或者

        umask(0000); // 访问权限便问 0777

    注意使用ACL来获取目录访问权限是首先推荐的方法，因为umask不保证线程安全。

    Note that using the ACL is recommended when you have access to them
    on your server because changing the umask is not thread-safe.

    **4. 使用PHP内置的Web服务器作为开发环境**

    **4. Use the built-in web server in development environments**

    内置的PHPWeb服务器 － 在开发环境中可使用 － 允许Web服务器账户与你当前操作账户相同。这样
    也就解决了访问权限的问题。

    The built-in PHP web server - which can be used during development - allows
    your web server user and CLI user to be the same. This removes any permissions
    issues:

    .. code-block:: bash

        $ php app/console server:start

    .. seealso::

        阅读更多关于内置服务器 :doc:`在技术手册一书中 </cookbook/web_server/built_in>`. 

        Read more about the internal server :doc:`in the cookbook </cookbook/web_server/built_in>`.

    **5. 为CLI和Web服务器使用相同账户**

    **5. Use the same user for the CLI and the web server**

    在开发环境中，常规的做法是命令操作与Web服务器使用相同账户，因为这样可以避免修改Web服务器配置（譬如，apache的 httpd.conf或者appache2.conf配置） 和 设置账户与CLI相同（譬如，Apache，更新用户和用户组值）。

    In development environments, it is a common practice to use the same unix
    user for the CLI and the web server because it avoids any of these permissions
    issues when setting up new projects. This can be done by editing your web server
    configuration (e.g. commonly httpd.conf or apache2.conf for Apache) and setting
    its user to be the same as your CLI user (e.g. for Apache, update the User
    and Group values).

当所有事情完成之后，点击 "Go to the Welcome page" 来访问真正的第一个Symfony页面：

When everything is fine, click on "Go to the Welcome page" to request your
first "real" Symfony webpage:

.. code-block:: text

    http://localhost/app_dev.php/

Symfony会欢迎你并祝贺你完成以上艰巨的任务。

Symfony should welcome and congratulate you for your hard work so far!

.. image:: /images/quick_tour/welcome.png

.. tip::

    为了获得好看的、更短的url地址，你需要把``Symfony/web/``配置为Web根目录。
    虽然在开发的时候没有必要，在发布环境中需要这么做，防止访问用户访问不该访问的目录。
    如何配置Web服务器根目录，请阅读 :doc:`/cookbook/configuration/web_server_configuration`
    或者 查阅Web服务器官方文档:
    `Apache`_ | `Nginx`_ .

    To get nice and short urls you should point the document root of your
    webserver or virtual host to the ``Symfony/web/`` directory. Though
    this is not required for development it is recommended at the time your
    application goes into production as all system and configuration files
    become inaccessible to clients then. For information on configuring
    your specific web server document root, read
    :doc:`/cookbook/configuration/web_server_configuration`
    or consult the official documentation of your webserver:
    `Apache`_ | `Nginx`_ .

开始开发
---------------------

Beginning Development
---------------------

到此为止，一个功能完整的Symfony应用安装完毕，你可以开始开发了！你的这个分布含有一些样例代码 －
检查 ``README.md`` 文档， 里面包含分布信息，告知你的分布中有那些样例代码。

Now that you have a fully-functional Symfony application, you can begin
development! Your distribution may contain some sample code - check the
``README.md`` file included with the distribution (open it as a text file)
to learn about what sample code was included with your distribution.

如果你是Symfony新手， 查看 ":doc:`page_creation`"， 这里教你如何创建页面， 改变配置，
和你想在项目中想做的一切。

If you're new to Symfony, check out ":doc:`page_creation`", where you'll
learn how to create pages, change configuration, and do everything else you'll
need in your new application.

确保查看 :doc:`Cookbook </cookbook/index>`， 里面包含许多宽泛的文章内容，教你使用Symfony如何解决各种特定问题。

Be sure to also check out the :doc:`Cookbook </cookbook/index>`, which contains
a wide variety of articles about solving specific problems with Symfony.

.. note::

    如果你想在分布中去除样例代码， 请看以下技术手册的一篇文章  ":doc:`/cookbook/bundles/remove`"

    If you want to remove the sample code from your distribution, take a look
    at this cookbook article: ":doc:`/cookbook/bundles/remove`"

使用版本控制
--------------------

Using Source Control
--------------------

如果你正在使用版本控制系统，如 ``Git`` 或者 ``Subversion``， 你可以建立你的版本控制系统，并且开始提交你的项目开发成果。 Symfony标准版本 是你新项目的一个起点。

If you're using a version control system like ``Git`` or ``Subversion``, you
can setup your version control system and begin committing your project to
it as normal. The Symfony Standard Edition *is* the starting point for your
new project.

对于特定的指导，关于如何使用最优项目管理方法 Git， 查看 :doc:`/cookbook/workflow/new_project_git`.

For specific instructions on how best to setup your project to be stored
in Git, see :doc:`/cookbook/workflow/new_project_git`.

忽略 ``vendor/`` 目录
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ignoring the ``vendor/`` Directory
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

如果你已经下载了没有第三方类库包的文档安装包，你可以很安全的忽略 ``vendor/`` 目录中的内容，不会对此目录夹内容进行版本控制。 通过``Git``， 创建一个``.gitignore``文件，
在里面添加以下内容：

If you've downloaded the archive *without vendors*, you can safely ignore
the entire ``vendor/`` directory and not commit it to source control. With
``Git``, this is done by creating and adding the following to a ``.gitignore``
file:

.. code-block:: text

    /vendor/

现在，vendor目录不会被版本控制监管。 这样比较好（实际上，非常棒）因为其他人值需要 clone和check out 项目之后运行个命令 ``php composer.phar install`` 就可以获得所需要所有项目关联类库文件。

Now, the vendor directory won't be committed to source control. This is fine
(actually, it's great!) because when someone else clones or checks out the
project, they can simply run the ``php composer.phar install`` script to
install all the necessary project dependencies.

.. _`打开ACL支持`: https://help.ubuntu.com/community/FilePermissionsACLs
.. _`http://symfony.com/download`: http://symfony.com/download
.. _`Git`: http://git-scm.com/
.. _`GitHub Bootcamp`: http://help.github.com/set-up-git-redirect
.. _`Composer`: http://getcomposer.org/
.. _`下载Composer`: http://getcomposer.org/download/
.. _`Apache`: http://httpd.apache.org/docs/current/mod/core.html#documentroot
.. _`Nginx`: http://wiki.nginx.org/Symfony
.. _`Symfony安装页`:    http://symfony.com/download
