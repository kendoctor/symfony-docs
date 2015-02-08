.. index::
   single: Symfony基础

.. _symfony2-and-http-fundamentals:

Symfony和HTTP基础
=============================

恭喜！学习Symfony，让你走上成为一个更高产能、更加成熟全面、更受欢迎的Web开发员之路。
（事实上，最终你将走上你的个性之路）。Symfony的诞生为了回归简单： 在跳出你的方式下，
开发各种各样工具，让你开发更快并且创建应用更加健壮。Symfony建立在从许多技术中借鉴的最好的思想上：
你将学习的工具和理念，代表了成众多人经历了许多年的努力成果。
也就是说，你不仅仅只是在学习 “Symfony”，在Symfony中或脱离Symfony之外，
同时还在学习Web核心基础，最强的开发实战经验，如何使用众多让你惊喜的PHP类库。
那么，做好准备吧。


是的，Symofny是一种哲学，这章节将讲解Web开发的常用基本概念：HTTP。 
不管你的背景和擅长的编程语言如何，这章节对于所有人都**必读**。


HTTP很简单
--------------

HTTP（超文本传输协议）是一种文本语言，允许两台机器进行互相交流。就是这个样子！举个例子，
当在站点 `xkcd`_ 上查看最新的喜剧漫画时，接下来（大致的）对话如下：

.. image:: /images/http-xkcd.png
   :align: center


当然，实际的语言会更加正式一点，但依然简单到死。
HTTP这个术语用于描述此类简单的文本语言。不管你如何进行Web开发，你服务器的目标始终知道
这种简单的文本请求，并返回简单的文本响应。

Symfony建立在这个事实之上。无论你知道与否，HTTP基本上你每天都在接触。
伴随Symfony，你将学习如何精通它。

.. index::
   single: HTTP; 请求－响应模式


第一步: 客户端发送一个请求
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

在Web站点上的每次对话，以一个*请求*开始。 
由客户端（举例，一个浏览器，一个智能手机App等等）发起的一次请求是一种俗称HTTP特殊格式的文本消息。
客户端向服务器发送请求，并等待回应。


看一下在浏览器与xkcd Web服务器之间交互（请求）的第一部分：

.. image:: /images/http-xkcd-request.png
   :align: center

用HTTP方式来讲， HTTP请求实际上看起来应该如此：


.. code-block:: text

    GET / HTTP/1.1
    Host: xkcd.com
    Accept: text/html
    User-Agent: Mozilla/5.0 (Macintosh)

这个简单的消息在交流关于客户端请求资源相关必要的 *所有事情*。HTTP请求的第一行最为重要，
含有两部分内容： URI 和 HTTP请求方法。

URI（举例， ``/``, ``/contact`` 等等） 是唯一地址和标示客户端需要资源的地点。
HTTP请求方法（举例，``GET``）定义你对所需要的资源准备*做*什么。 
HTTP请求方法是请求的*动词形式*，定义了你如何操作资源一些常规的方法：


+----------+---------------------------------------+
| *GET*    | 从服务器获取资源                         |
+----------+---------------------------------------+
| *POST*   | 在服务器上创建资源                       |
+----------+---------------------------------------+
| *PUT*    | 在服务器上更新资源                       |
+----------+---------------------------------------+
| *DELETE* | 在服务器上删除资源                       |
+----------+---------------------------------------+


记住，想像一下删除一篇指定博文的HTTP请求，举例：


.. code-block:: text

    DELETE /blog/15 HTTP/1.1

.. note::

    事实上，HTTP规范定义了9种HTTP请求方法，但大多数没有被广泛使用和支持。真实情况，
    现在许多的浏览器都甚至不支持 ``PUT`` 和 ``DELETE`` 请求方法。

除了HTTP第一行信息以外，HTTP请求总是包含其他以行为单元的俗称*请求头*的信息。
请求头提供很多宽泛的信息，譬如，请求主机名 ``（Host）``，客户端可接受（``Accept``）的响应格式，客户端创建请求的应用程序－俗称用户代理(``User-Agent``)。存在许多其他请求头信息类型，可以在百科文章 `HTTP头信息字段列表`_  里找到 。


Step 2: 服务器返回一个响应
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当服务器收到请求，（通过URI）知道客户端实际需要的资源，（通过请求方法）明白客户端需要对资源做什么。
举例，在GET请求中，服务器准备好资源，并在HTTP响应时返回资源。考虑一下从xkcd Web服务器上的响应：

.. image:: /images/http-xkcd.png
   :align: center

转换成HTTP，响应返送回浏览器，看起来就像这样：


.. code-block:: text

    HTTP/1.1 200 OK
    Date: Sat, 02 Apr 2011 21:05:05 GMT
    Server: lighttpd/1.4.19
    Content-Type: text/html

    <html>
      <!-- ... HTML for the xkcd comic -->
    </html>


HTTP回应包含了请求资源（这里是HTML内容），同样还有一些关于响应的其他信息。第一行尤其重要，
包含HTTP响应的状态码（这里是200）。这个状态码联系请求回客户端的所有输出。这个请求成功与否？
是否存在错误？不同的状态码暗示成功，错误，客户端需要做些其他事情（譬如，跳转至其他页面）。
所有的状态码列表可以在百科文章 `HTTP状态码列表`_ 中找到。


The HTTP response contains the requested resource (the HTML content in this
case), as well as other information about the response. The first line is
especially important and contains the HTTP response status code (200 in this
case). The status code communicates the overall outcome of the request back
to the client. Was the request successful? Was there an error? Different
status codes exist that indicate success, an error, or that the client needs
to do something (e.g. redirect to another page). A full list can be found
on Wikipedia's `List of HTTP status codes`_ article.

如同请求，HTTP响应通过HTTP头包含额外的一些信息。举例，一个重要的HTTP响应头是 ``Content-Type``。
同一个资源的主体内容可以以多种格式，像HTML，XML或者JSON，返回，``Content-Type``头使用互联网媒体
资源类型，如``text/html``，告诉客户端那种格式将被返回。常用的媒体资源类型列表可以在百科文章
`常规的媒体资源类型`_ 中找到。

Like the request, an HTTP response contains additional pieces of information
known as HTTP headers. For example, one important HTTP response header is
``Content-Type``. The body of the same resource could be returned in multiple
different formats like HTML, XML, or JSON and the ``Content-Type`` header uses
Internet Media Types like ``text/html`` to tell the client which format is
being returned. A list of common media types can be found on Wikipedia's
`List of common media types`_ article.

许多其他的头信息存在，有一些非常强大。举例，一些头信息可以用于创建强大的缓存系统。

Many other headers exist, some of which are very powerful. For example, certain
headers can be used to create a powerful caching system.

请求,响应和Web开发
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Requests, Responses and Web Development
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

这种请求－响应的对话方式，是一种基本过程，驱动Web之间的所有通讯。终然如此重要和强大的过程，
它也就是这么简单。

This request-response conversation is the fundamental process that drives all
communication on the web. And as important and powerful as this process is,
it's inescapably simple.

最重要的事实是：不管你使用的语言是什么，想要开发的应用程序的类型是什么（Web，移动，JSON API）
或者，你崇尚的开发理念是什么，最终应用程序的目标**始终**是去了解每次请求，创建并返回正确的响应。

The most important fact is this: regardless of the language you use, the
type of application you build (web, mobile, JSON API) or the development
philosophy you follow, the end goal of an application is **always** to understand
each request and create and return the appropriate response.

构建Symfony就是针对此事实。

Symfony is architected to match this reality.

.. tip::

    为了学习更多HTTP规范，查阅原版的 `HTTP 1.1 RFC`_ 或者 为了细述原版规范内容的`HTTP Bis`_。
    一个好使的在浏览时用于检查请求和响应头的FireFox扩展 `Live HTTP Headers`_ 工具。


    To learn more about the HTTP specification, read the original `HTTP 1.1 RFC`_
    or the `HTTP Bis`_, which is an active effort to clarify the original
    specification. A great tool to check both the request and response headers
    while browsing is the `Live HTTP Headers`_ extension for Firefox.

.. index::
   single: Symfony基础；请求和响应

.. index::
   single: Symfony Fundamentals; Requests and responses

PHP中的请求和响应
-----------------------------

Requests and Responses in PHP
-----------------------------

那么，当使用PHP时，你如何与 “请求” 交互并创建一个“响应”？实际上，PHP帮你抽象了整个过程::

So how do you interact with the "request" and create a "response" when using
PHP? In reality, PHP abstracts you a bit from the whole process::

    $uri = $_SERVER['REQUEST_URI'];
    $foo = $_GET['foo'];

    header('Content-Type: text/html');
    echo 'The URI requested is: '.$uri;
    echo 'The value of the "foo" parameter is: '.$foo;

听起来有些奇怪，这个小程序事实上通过请求获取信息，并用它创建HTTP响应。 而不是处理原始的HTTP请求消息，
PHP准备超全局变量，如``$_SERVER`` 和 ``$_GET`` 包含请求信息。 同样，不是返回HTTP格式的文本响应，
你可以用 ``header()`` 函数创建响应头和简练的带出实际的内容，它将是响应内容部分。PHP会创建一个
真正的HTTP响应并返回给客户端。

As strange as it sounds, this small application is in fact taking information
from the HTTP request and using it to create an HTTP response. Instead of
parsing the raw HTTP request message, PHP prepares superglobal variables
such as ``$_SERVER`` and ``$_GET`` that contain all the information from
the request. Similarly, instead of returning the HTTP-formatted text response,
you can use the ``header()`` function to create response headers and simply
print out the actual content that will be the content portion of the response
message. PHP will create a true HTTP response and return it to the client:

.. code-block:: text

    HTTP/1.1 200 OK
    Date: Sat, 03 Apr 2011 02:14:33 GMT
    Server: Apache/2.2.17 (Unix)
    Content-Type: text/html

    请求URI是: /testing?foo=symfony
    参数 "foo" 的值是: symfony 

    The URI requested is: /testing?foo=symfony
    The value of the "foo" parameter is: symfony

Symfony中的请求和响应
---------------------------------

Requests and Responses in Symfony
---------------------------------

Symfony提供另外一种方式来原始PHP处理方法通过两个类，它允许你使用一种更简单的方法与HTTP请求和响应交互。
:class:`Symfony\\Component\\HttpFoundation\\Request` 类是HTTP请求消息一个简单的面向对象的表现形式。
通过它，你可以在提示下获取所有请求消息::

Symfony provides an alternative to the raw PHP approach via two classes that
allow you to interact with the HTTP request and response in an easier way.
The :class:`Symfony\\Component\\HttpFoundation\\Request` class is a simple
object-oriented representation of the HTTP request message. With it, you
have all the request information at your fingertips::

    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();

    // 取出请求参数的请求URI地址

    // the URI being requested (e.g. /about) minus any query parameters
    $request->getPathInfo();

    // 获得针对需要的 GET 和 POST 变量

    // retrieve GET and POST variables respectively
    $request->query->get('foo');
    $request->request->get('bar', 'default value if bar does not exist');

    // 获取 SERVER 变量

    // retrieve SERVER variables
    $request->server->get('HTTP_HOST');

    // 通过foo获取 UploadedFile 对象

    // retrieves an instance of UploadedFile identified by foo
    $request->files->get('foo');

    // 获取 COOKIE 值

    // retrieve a COOKIE value
    $request->cookies->get('PHPSESSID');

    // 通过正常化，小写key获取HTTP请求头

    // retrieve an HTTP request header, with normalized, lowercase keys
    $request->headers->get('host');
    $request->headers->get('content_type');

    $request->getMethod();          // GET, POST, PUT, DELETE, HEAD
    $request->getLanguages();       // 客户端支持的语言种类集
    $request->getLanguages();       // an array of languages the client accepts

除此之外，``Request`` 类在背后帮你处理许多事情，你永远不必担忧的。 举例，`isSecure()`` 方法
检查PHP中 *3* 种不同的值， 指示用户是否通过安全连接（如HTTPS）相连。

As a bonus, the ``Request`` class does a lot of work in the background that
you'll never need to worry about. For example, the ``isSecure()`` method
checks the *three* different values in PHP that can indicate whether or not
the user is connecting via a secured connection (i.e. HTTPS).

.. sidebar:: 参数包（ParameterBags）和请求属性（Attributes）
.. sidebar:: ParameterBags and Request Attributes
    
    从上面看，``$_GET`` 和 ``$_POST`` 变量各自通过公共的 ``query`` and ``request`` 属性
    进行访问。 这两个对象都是 :class:`Symfony\\Component\\HttpFoundation\\ParameterBag`
    对象， 它拥有方法
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::get`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::has`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::all` 等等.
    实际上，在前面例子里使用的每个公共属性是不同的参数包（ParameterBag）实例对象。

    As seen above, the ``$_GET`` and ``$_POST`` variables are accessible via
    the public ``query`` and ``request`` properties respectively. Each of
    these objects is a :class:`Symfony\\Component\\HttpFoundation\\ParameterBag`
    object, which has methods like
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::get`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::has`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::all` and more.
    In fact, every public property used in the previous example is some instance
    of the ParameterBag.

    .. _book-fundamentals-attributes:

    请求类也拥有一个公共 ``attributes`` 属性， 它保存相关应用内部工作的特殊数据。
    针对Symfony框架， ``attributes`` 存储匹配路由返回的值，像 ``_controller``，
    ``id`` （如果拥有一个 ``{id}`` 匹配）， 甚至匹配路由的名称 (``_route``)。
    ``attributes`` 属性存在的地方可以针对当前请求让你准备和存储特定环境信息。

    The Request class also has a public ``attributes`` property, which holds
    special data related to how the application works internally. For the
    Symfony framework, the ``attributes`` holds the values returned by the
    matched route, like ``_controller``, ``id`` (if you have an ``{id}``
    wildcard), and even the name of the matched route (``_route``). The
    ``attributes`` property exists entirely to be a place where you can
    prepare and store context-specific information about the request.

Symfony 也提供一个 ``Response`` 类： HTTP响应消息的简单封装。 它允许应用程序使用面向对象接口
来创建返回到客户端的响应。

Symfony also provides a ``Response`` class: a simple PHP representation of
an HTTP response message. This allows your application to use an object-oriented
interface to construct the response that needs to be returned to the client::

    use Symfony\Component\HttpFoundation\Response;

    $response = new Response();

    $response->setContent('<html><body><h1>Hello world!</h1></body></html>');
    $response->setStatusCode(Response::HTTP_OK);
    $response->headers->set('Content-Type', 'text/html');

    // 输出HTTP头和内容
    // prints the HTTP headers followed by the content
    $response->send();

.. versionadded:: 2.4
    Symfony2.4中加入了支持HTTP状态码常量
    Support for HTTP status code constants was introduced in Symfony 2.4.

如果Symfony不提供什么，那你应该拥有访问请求信息的工具和创建响应的面向对象的接口。
甚至，当你学习Symfony许多强大的特性时，搞清楚一点应用程序的目标始终时 *翻译一个请求并
基于你应用程序的逻辑创建针对性的响应*

If Symfony offered nothing else, you would already have a toolkit for easily
accessing request information and an object-oriented interface for creating
the response. Even as you learn the many powerful features in Symfony, keep
in mind that the goal of your application is always *to interpret a request
and create the appropriate response based on your application logic*.

.. tip::

    ``Request`` 和 ``Response`` 类是独立Symfony组件 HttpFoundation 中的一部分。
    这个组件可以被完全独立使用，并且提供处理 Sessions 和文件上传的类。

    The ``Request`` and ``Response`` classes are part of a standalone component
    included with Symfony called HttpFoundation. This component can be
    used entirely independently of Symfony and also provides classes for handling
    sessions and file uploads.

从请求到响应的过程
--------------------------------------------

The Journey from the Request to the Response
--------------------------------------------

如同HTTP本身，``Request`` 和 ``Response`` 对象也非常简单。
开发应用的难点在于请求和响应之间你要做什么。
换而言之，真正的工作是编制代码来如何翻译请求信息和创建响应。

Like HTTP itself, the ``Request`` and ``Response`` objects are pretty simple.
The hard part of building an application is writing what comes in between.
In other words, the real work comes in writing the code that interprets the
request information and creates the response.

你的应用可能需要做许多事情，譬如发送邮件，处理提交，向数据库中保存东西，输出HTML页面，保护
内容安全。 你如何管理这些事和仍然可以保持你代码有效组织和便于维护呢？

Your application probably does many things, like sending emails, handling
form submissions, saving things to a database, rendering HTML pages and protecting
content with security. How can you manage all of this and still keep your
code organized and maintainable?

Symfony被创造，来解决这些问题，所以你就可以省事了。

Symfony was created to solve these problems so that you don't have to.

前端控制器（Front Controller）
~~~~~~~~~~~~~~~~~~~~

The Front Controller
~~~~~~~~~~~~~~~~~~~~

传统方法，应用程序的站点页面是单个的物理文件：

Traditionally, applications were built so that each "page" of a site was
its own physical file:

.. code-block:: text

    index.php
    contact.php
    blog.php

这种方式存在几个问题，包括访问URLs地址不具伸缩性（
在更改``blog.php`` to ``news.php`` 文件名的同时，如何不破坏所有链接）
， 每个文件*必须*手工包含核心文件，安全，数据库链接，站点“样貌”能保留继续可用性。

There are several problems with this approach, including the inflexibility
of the URLs (what if you wanted to change ``blog.php`` to ``news.php`` without
breaking all of your links?) and the fact that each file *must* manually
include some set of core files so that security, database connections and
the "look" of the site can remain consistent.

一个更好的解决方法是使用 :term:`前端控制器(front controller)`: 一个PHP文件，处理
每次向应用发送的请求。 举例：

A much better solution is to use a :term:`front controller`: a single PHP
file that handles every request coming into your application. For example:

+------------------------+------------------------+
| ``/index.php``         | 执行 ``index.php``     |
+------------------------+------------------------+
| ``/index.php/contact`` | 执行 ``index.php``     |
+------------------------+------------------------+
| ``/index.php/blog``    | 执行 ``index.php``     |
+------------------------+------------------------+

+------------------------+------------------------+
| ``/index.php``         | executes ``index.php`` |
+------------------------+------------------------+
| ``/index.php/contact`` | executes ``index.php`` |
+------------------------+------------------------+
| ``/index.php/blog``    | executes ``index.php`` |
+------------------------+------------------------+

.. tip::

    使用Apache的 ``mod_rewrite`` （其他Web服务器相同的功能）
    访问URLs可以很容易被干净成 ``/``, ``/contact`` 和
    ``/blog``。

    Using Apache's ``mod_rewrite`` (or equivalent with other web servers),
    the URLs can easily be cleaned up to be just ``/``, ``/contact`` and
    ``/blog``.

现在，每次请求以相同方式正确被处理。 不是每个URLs访问执行不同的PHP文件，
前端控制器（front controller）*始终*被第一执行， 不同URLs路由至应用不同地方进行内部处理。
这样可以解决传统方法产生的两个问题。当今的Web应用都这么做，包括WordPress应用。

Now, every request is handled exactly the same way. Instead of individual URLs
executing different PHP files, the front controller is *always* executed,
and the routing of different URLs to different parts of your application
is done internally. This solves both problems with the original approach.
Almost all modern web apps do this - including apps like WordPress.

保持良好的代码组织
~~~~~~~~~~~~~~

Stay Organized
~~~~~~~~~~~~~~

在前端控制器中，你需要指出哪些代码需要执行，哪些内容需要被返回。为了能指出这，你将
需要检查来访的URI，根据其值执行不同的代码逻辑。这很快做到::

Inside your front controller, you have to figure out which code should be
executed and what the content to return should be. To figure this out, you'll
need to check the incoming URI and execute different parts of your code depending
on that value. This can get ugly quickly::

    // index.php
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;

    $request = Request::createFromGlobals();
    $path = $request->getPathInfo(); // the URI path being requested

    if (in_array($path, array('', '/'))) {
        $response = new Response('Welcome to the homepage.');
    } elseif ('/contact' === $path) {
        $response = new Response('Contact us');
    } else {
        $response = new Response('Page not found.', Response::HTTP_NOT_FOUND);
    }
    $response->send();

解决这种问题可能比较困难。幸运的是Symfony设计出来就完全是为了干这个的。

Solving this problem can be difficult. Fortunately it's *exactly* what Symfony
is designed to do.

Symfony应用程序执行流程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Symfony Application Flow
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当你让Symfony处理每个请求的时候，工作就日次简单。Symfony遵循这种相同简单的方式来处理请求。

When you let Symfony handle each request, life is much easier. Symfony follows
the same simple pattern for every request:

.. _request-flow-figure:

.. figure:: /images/request-flow.png
   :align: center
   :alt: Symfony request flow

   来访请求通过路由机制被翻译，传递至控制器的返回 ``Response`` 对象的函数。
   Incoming requests are interpreted by the routing and passed to controller
   functions that return ``Response`` objects.

每个站点的 “页面” 在路由配置种定义，不同URL地址映射到不同的PHP函数。称作 :term:`（控制器）controller`
的PHP函数，它的工作是使用请求信息 － 结合Symfony提供的许多其他工具 － 来创建和返回 ``Response`` 对象。
也就是说，控制器是*你*编制代码逻辑的地方：这里翻译请求并创建响应。

Each "page" of your site is defined in a routing configuration file that
maps different URLs to different PHP functions. The job of each PHP function,
called a :term:`controller`, is to use information from the request - along
with many other tools Symfony makes available - to create and return a ``Response``
object. In other words, the controller is where *your* code goes: it's where
you interpret the request and create a response.

就是这么简单！回顾一下：

It's that easy! To review:

* 每个请求执行同一个前端控制器文件；

* Each request executes a front controller file;

* 路由系统决定哪个PHP函数需要执行，根据请求信息以及你创建的路由配置；

* The routing system determines which PHP function should be executed based
  on information from the request and routing configuration you've created;

* 正确的PHP函数执行，你的代码创建并返回相应的 ``Response`` 对象。

* The correct PHP function is executed, where your code creates and returns
  the appropriate ``Response`` object.

一个Symfony请求在Action中完成
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Symfony Request in Action
~~~~~~~~~~~~~~~~~~~~~~~~~~~

无需分的太过详细，这里就是在Action中的过程。假设你想要在Symfony应用中添加一张 ``/contact`` 页面。
首先，在路由配置文件中，添加一个 ``/contact`` 的入口：

Without diving into too much detail, here is this process in action. Suppose
you want to add a ``/contact`` page to your Symfony application. First, start
by adding an entry for ``/contact`` to your routing configuration file:

.. configuration-block::

    .. code-block:: yaml

        # app/config/routing.yml
        contact:
            path:     /contact
            defaults: { _controller: AppBundle:Main:contact }

    .. code-block:: xml

        <!-- app/config/routing.xml -->
        <?xml version="1.0" encoding="UTF-8" ?>
        <routes xmlns="http://symfony.com/schema/routing"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://symfony.com/schema/routing
                http://symfony.com/schema/routing/routing-1.0.xsd">

            <route id="contact" path="/contact">
                <default key="_controller">AppBundle:Main:contact</default>
            </route>
        </routes>

    .. code-block:: php

        // app/config/routing.php
        use Symfony\Component\Routing\Route;
        use Symfony\Component\Routing\RouteCollection;

        $collection = new RouteCollection();
        $collection->add('contact', new Route('/contact', array(
            '_controller' => 'AppBundle:Main:contact',
        )));

        return $collection;

当有人访问 ``/contact`` 页面时，这个路由被匹配到，路由指定的控制器就被执行。
你将在 :doc:`路由章节 </book/routing>` 中学习到，``AcmeDemoBundle:Main:contact`` 
是一个简单的语法，它执行指定类 ``MainController`` 的 PHP函数 ``contactAction``::

When someone visits the ``/contact`` page, this route is matched, and the
specified controller is executed. As you'll learn in the :doc:`routing chapter </book/routing>`,
the ``AcmeDemoBundle:Main:contact`` string is a short syntax that points to a
specific PHP method ``contactAction`` inside a class called ``MainController``::

    // src/AppBundle/Controller/MainController.php
    namespace AppBundle\Controller;

    use Symfony\Component\HttpFoundation\Response;

    class MainController
    {
        public function contactAction()
        {
            return new Response('<h1>Contact us!</h1>');
        }
    }

在这个简单的例子中，控制器简单的创建了一个包含 ``<h1>Contact us!</h1>`` 内容的 :class:`Symfony\\Component\\HttpFoundation\\Response` 对象。 在 :doc:`控制器章节 </book/controller>` 中，
你将学习如何让让控制输出模版，让你的 “呈现层" 代码（譬如，输出HTML）存储在独立的模版文件中。
这样可以解放控制器去关注更重要的部分：如何跟数据库交互，处理提交数据，或者发送邮件消息。

In this very simple example, the controller simply creates a 
:class:`Symfony\\Component\\HttpFoundation\\Response` object with the HTML
``<h1>Contact us!</h1>``. In the :doc:`controller chapter </book/controller>`,
you'll learn how a controller can render templates, allowing your "presentation"
code (i.e. anything that actually writes out HTML) to live in a separate
template file. This frees up the controller to worry only about the hard
stuff: interacting with the database, handling submitted data, or sending
email messages.

.. _symfony2-build-your-app-not-your-tools:

Symfony: 开发你的应用，不是你的工具
---------------------------------------

Symfony: Build your App, not your Tools
---------------------------------------

你现在应该知道任何应用目标都是为了翻译来访的每个请求，并创建一个针对性的响应。当应用慢慢变大了，
代码组织和维护就会变得更加困难。不变的是，同样复杂的任务都是这个套路：保存数据到数据库，
输出和重用模版，处理提交，发送邮件，验证用户输入和处理安全问题。

You now know that the goal of any app is to interpret each incoming request
and create an appropriate response. As an application grows, it becomes more
difficult to keep your code organized and maintainable. Invariably, the same
complex tasks keep coming up over and over again: persisting things to the
database, rendering and reusing templates, handling form submissions, sending
emails, validating user input and handling security.

好消息是，不是每一个问题是唯一的。Symfony提供一个拥有许多功能的框架，让你开发你的应用，无需你的工具。
使用Symfony，没有什么需要强制你做什么：你可以很自由的使用Symfony框架，也可以只使用Symfony中你认为
有用的部分。

The good news is that none of these problems is unique. Symfony provides
a framework full of tools that allow you to build your application, not your
tools. With Symfony, nothing is imposed on you: you're free to use the full
Symfony framework, or just one piece of Symfony all by itself.

.. index::
   single: Symfony组件（ Components）
   single: Symfony Components

.. _standalone-tools-the-symfony2-components:


独立功能逻辑: Symfony *组件（Components）*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Standalone Tools: The Symfony *Components*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

那么Symfony究竟*是*什么？ 首先，Symfony是一个拥有20多个独立类库的集合体，这些类库可以在*任何*
PHP项目中使用。这些类库，称作 Symfony*组件（Components）*，针对不同情况，提供不同解决，
不管你的项目是怎么开发的。 这里提及一些：

So what *is* Symfony? First, Symfony is a collection of over twenty independent
libraries that can be used inside *any* PHP project. These libraries, called
the *Symfony Components*, contain something useful for almost any situation,
regardless of how your project is developed. To name a few:

* :doc:`HttpFoundation </components/http_foundation/introduction>` - 包含
``Request`` 和 ``Response`` 类, 还有处理Sessions和文件上传的其他类;

* :doc:`HttpFoundation </components/http_foundation/introduction>` - Contains
  the ``Request`` and ``Response`` classes, as well as other classes for handling
  sessions and file uploads;

* :doc:`(路由)Routing </components/routing/introduction>` - 强大而又高效的路由系统，
  允许你映射指定URI地址(譬如 ``/contact``) 到需要针对信息该如何处理请求。
  (譬如，执行 ``contactAction()`` 方法）;

* :doc:`Routing </components/routing/introduction>` - Powerful and fast routing system that
  allows you to map a specific URI (e.g. ``/contact``) to some information
  about how that request should be handled (e.g. execute the ``contactAction()``
  method);

* :doc:`(表单)Form </components/form/introduction>` - 创建表单和处理表单提交的
  具备完整特性，伸缩性架构;

* :doc:`Form </components/form/introduction>` - A full-featured and flexible
  framework for creating forms and handling form submissions;

* `验证器(Validator)`_ - 创建数据规则和验证用户提交数据是否遵循这些规则的系统

* `Validator`_ - A system for creating rules about data and then validating
  whether or not user-submitted data follows those rules;

* :doc:`(模版引擎) Templating </components/templating/introduction>` - 输出模版，处理模版继承
  （譬如，模版修饰成布局（layout））和 处理其他常规模版任务的工具包。

* :doc:`Templating </components/templating/introduction>` - A toolkit for rendering
  templates, handling template inheritance (i.e. a template is decorated with
  a layout) and performing other common template tasks;

* :doc:`(安全)Security </components/security/introduction>` - 处理应用中所有类型的安全问题的
  一个强大类库。

* :doc:`Security </components/security/introduction>` - A powerful library for
  handling all types of security inside an application;

* :doc:`(翻译)Translation </components/translation/introduction>` - 在应用中处理内容翻译的框架.

* :doc:`Translation </components/translation/introduction>` - A framework for
  translating strings in your application.

任何这些组件中的一个，都是可独立使用，并且可以在*任何*PHP项目中使用，不管你是使用Symfony框架，
或者不是。如果需要，每个部分都可以拿来使用，拿来替换。

Each and every one of these components is decoupled and can be used in *any*
PHP project, regardless of whether or not you use the Symfony framework.
Every part is made to be used if needed and replaced when necessary.

.. _the-full-solution-the-symfony2-framework:


完整解决方案: Symfony *框架*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The Full Solution: The Symfony *Framework*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

那么，什么是Symfony*框架*呢？*Symfony Framework＊是一个PHP类库，用于完成两大任务：

So then, what *is* the Symfony *Framework*? The *Symfony Framework* is
a PHP library that accomplishes two distinct tasks:

#. 提供一套精选的组件（譬如，Symfony组件）和第三方列哭（譬如，用于发送邮件的 `Swift Mailer`_ 类库）

#. Provides a selection of components (i.e. the Symfony Components) and
   third-party libraries (e.g. `Swift Mailer`_ for sending emails);

#. 提供精心考究过的配置，和联系所有这些的“胶水"类库。

#. Provides sensible configuration and a "glue" library that ties all of these
   pieces together.

框架的目标是整合独立的工具，为了给开发员提供一致性体验。即使是框架本本身，也是Symfony 模块（Bundle）
，它完全可以被配置和替换。

The goal of the framework is to integrate many independent tools in order
to provide a consistent experience for the developer. Even the framework
itself is a Symfony bundle (i.e. a plugin) that can be configured or replaced
entirely.

Symfony提供一条强大的工具集合，用于快速开发Web应用，不需要难为你自己去做这些。普通用户也可以很快
开始使用Symfony分布包进行开发，它提供一个项目的基本架构，包含精心考究的默认配置。对于高级用户，
就看你的能耐如何了。

Symfony provides a powerful set of tools for rapidly developing web applications
without imposing on your application. Normal users can quickly start development
by using a Symfony distribution, which provides a project skeleton with
sensible defaults. For more advanced users, the sky is the limit.

.. _`xkcd`: http://xkcd.com/
.. _`HTTP 1.1 RFC`: http://www.w3.org/Protocols/rfc2616/rfc2616.html
.. _`HTTP Bis`: http://datatracker.ietf.org/wg/httpbis/
.. _`Live HTTP Headers`: https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/
.. _`HTTP状态码列表`: http://en.wikipedia.org/wiki/List_of_HTTP_status_codes
.. _`List of HTTP status codes`: http://en.wikipedia.org/wiki/List_of_HTTP_status_codes
.. _`HTTP头信息列表`: http://en.wikipedia.org/wiki/List_of_HTTP_header_fields
.. _`List of HTTP header fields`: http://en.wikipedia.org/wiki/List_of_HTTP_header_fields
.. _`常用媒体资源类型列表`: http://en.wikipedia.org/wiki/
.. _`List of common media types`: http://en.wikipedia.org/wiki/Internet_media_type#List_of_common_media_types
.. _`验证器`: https://github.com/symfony/Validator
.. _`Swift Mailer`: http://swiftmailer.org/
