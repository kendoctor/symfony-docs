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


HTTP响应含有请求资源（这里是HTML内容），同样还有一些关于响应的其他信息。第一行尤其重要，
包含HTTP响应的状态码（这里是200）。这个状态码关系着请求返回至客户端的总体结果。
请求成功与否？是否存在错误？不同的状态码暗示成功、一个错误、客户端需要做些其他事情
（譬如，跳转至其他页面）。所有的状态码列表可以在百科文章 `HTTP状态码列表`_ 中找到。


如同请求，HTTP响应包含额外的一些信息，俗称HTTP头。举例，一个重要的HTTP响应头是 ``Content-Type``。
同一资源的内容形式可以通过多种格式（譬如HTML，XML或者JSON）返回，``Content-Type``头使用互联网媒体
资源类型，如``text/html``，告诉客户端那种格式将被返回。常用的媒体资源类型列表可以在百科文章
`常规的媒体资源类型`_ 中找到。

许多其他的HTTP头存在，有一些非常强大。例如，一些HTTP头可以用于创建强大的缓存系统。


请求,响应和Web开发
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

这种请求－响应的对话方式，是一种基础的处理过程，在Web中驱动所有信息交互。终然如此重要和强大的处理过程，
它也就是这么简单。

最重要的事实是：不管你使用的语言是什么，想要开发的应用程序的类型是什么（Web，移动，JSON API）
或者，你崇尚的开发理念是什么，最终应用程序的目标**始终**是去获知每次请求，创建并返回匹配的响应。

构建Symfony就是针对此事实。


.. tip::

    为了学习更多HTTP规范，查阅原版的 `HTTP 1.1 RFC`_ 或者 针对原版规范内容进行细述辨析的文献 `HTTP Bis`_。 一个，在浏览时用于检查请求和响应头的实用工具，FireFox扩展 `Live HTTP Headers`_。


.. index::
   single: Symfony基础；请求和响应


PHP中的请求和响应
-----------------------------

那么，当使用PHP时，你如何与 “请求” 交互并创建一个“响应”？实际上，PHP帮你抽象了整个过程::

    $uri = $_SERVER['REQUEST_URI'];
    $foo = $_GET['foo'];

    header('Content-Type: text/html');
    echo 'The URI requested is: '.$uri;
    echo 'The value of the "foo" parameter is: '.$foo;

听起来有些奇怪，这个小程序事实上通过请求获取信息，并根据获取信息创建HTTP响应。 不用处理原始的HTTP请求消息，PHP提供超全局变量，如``$_SERVER`` 和 ``$_GET``，包含了请求信息。 同样，不是返回HTTP格式的文本响应，你可以用 ``header()`` 函数创建并输出响应HTTP头；简单打印输出实际的内容，此内容属于响应消息的内容部分。
PHP会创建一个真正的HTTP响应并返回给客户端。

.. code-block:: text

    HTTP/1.1 200 OK
    Date: Sat, 03 Apr 2011 02:14:33 GMT
    Server: Apache/2.2.17 (Unix)
    Content-Type: text/html

    请求URI是: /testing?foo=symfony
    参数 "foo" 的值是: symfony 


Symfony中的请求和响应
---------------------------------

Symfony通过两个类，提供另外一种方式来实现PHP原始的处理方法，
这两个类允许你通过一种更简练的途径，与HTTP请求和响应交互。
:class:`Symfony\\Component\\HttpFoundation\\Request` 类是
一个简单的面向对象的HTTP请求消息表现形式。通过它，你可以在按键提示下获取所有请求消息::


    use Symfony\Component\HttpFoundation\Request;

    $request = Request::createFromGlobals();

    // 获取去除访问参数的请求URI地址（例如 /about)
    $request->getPathInfo();

    // 获得各自针对需要的GET和POST变量
    $request->query->get('foo');
    $request->request->get('bar', 'default value if bar does not exist');

    // 获取SERVER变量
    $request->server->get('HTTP_HOST');

    // 获取由foo标示的UploadedFile实例对象
    $request->files->get('foo');

    // 获取COOKIE值
    $request->cookies->get('PHPSESSID');

    // 通过标准化小写的键值获取HTTP请求头
    $request->headers->get('host');
    $request->headers->get('content_type');

    $request->getMethod();          // GET, POST, PUT, DELETE, HEAD
    $request->getLanguages();       // 客户端支持的语言类种

除此之外，``Request`` 类在背后帮你处理你永远不必担忧的许多事情。 举例，`isSecure()`` 方法
检查PHP中*3*种不同的值， 指示用户是否通过安全连接（如HTTPS）建立连接。


.. sidebar:: 参数包集（ParameterBags）和请求属性集（Attributes）
    
    从上面看，``$_GET`` 和 ``$_POST`` 变量各自通过公共的 ``query`` 和 ``request`` 属性
    进行访问。 这两个对象都是 :class:`Symfony\\Component\\HttpFoundation\\ParameterBag`
    对象， 拥有 :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::get`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::has`,
    :method:`Symfony\\Component\\HttpFoundation\\ParameterBag::all` 等等的方法.
    实际上，在前面例子里使用的每个公共属性是某个参数包（ParameterBag）实例对象。

    .. _book-fundamentals-attributes:

    Request类还拥有一个公共 ``attributes`` 属性， 保存相关应用内部如何工作的特殊数据。
    针对Symfony框架， ``attributes`` 存储匹配路由返回的值，像 ``_controller``，
    ``id`` （如果拥有一个 ``{id}`` 匹配）， 甚至还有匹配路由的名称 (``_route``)。
    ``attributes`` 属性存在的地方，你可以针对当前请求，准备和存储特定环境信息。

    
Symfony也提供一个 ``Response`` 类： HTTP响应消息简易的PHP表现形式。 
它允许应用程序使用面向对象接口来创建返回到客户端的HTTP响应。

    use Symfony\Component\HttpFoundation\Response;

    $response = new Response();

    $response->setContent('<html><body><h1>Hello world!</h1></body></html>');
    $response->setStatusCode(Response::HTTP_OK);
    $response->headers->set('Content-Type', 'text/html');

    // 输出HTTP头，之后输出内容
    $response->send();

.. versionadded:: 2.4
    Symfony2.4中加入了支持HTTP状态码常量

如果Symfony不提供什么，说不定你早已有了轻松访问请求信息的现成工具和创建响应的面向对象接口。
但是在你学习Symfony许多强大的特性时，要搞清楚一点，应用程序的目标始终是*解释一个请求并
基于你应用程序的逻辑创建针对性的响应*

.. tip::

    ``Request`` 和 ``Response`` 类是独立的Symfony组件－HttpFoundation中的一部分。
    这个组件可以被完全独立使用，并且提供处理Sessions和文件上传的类。


从请求到响应的之旅
--------------------------------------------

如同HTTP本身，``Request`` 和 ``Response`` 对象也非常简单。
开发应用的难点在于请求和响应之间你要做的什么。
换而言之，真正的工作来自编写代码实现如何翻译请求信息和创建响应。

你的应用可能需要做许多事情，譬如发送邮件，处理提交，向数据库中保存东西，输出HTML页面，保护
内容安全。你如何管理这些事情而又照样可以保持代码有效的组织和便于维护呢？


Symfony的诞生之初，就是为了解决这些问题，所以你就可以省心了。


前端控制器（Front Controller）
~~~~~~~~~~~~~~~~~~~~~~~~~~~


老方法，应用程序的站点页面是单个的物理文件：

.. code-block:: text

    index.php
    contact.php
    blog.php

这种方式存在几个问题，包括访问URLs地址不具备伸缩性
（在更改``blog.php`` to ``news.php`` 文件名的同时，如何不破坏你的所有链接呢？）和
每个文件*必须*手工包含核心文件，来保证安全性，数据库链接，站点“样貌”一致性的事实。


一个更好的解决方法是使用 :term:`前端控制器(front controller)`: 用一个PHP文件处理
每次向应用发送的请求。 举例：

+------------------------+------------------------+
| ``/index.php``         | 执行 ``index.php``     |
+------------------------+------------------------+
| ``/index.php/contact`` | 执行 ``index.php``     |
+------------------------+------------------------+
| ``/index.php/blog``    | 执行 ``index.php``     |
+------------------------+------------------------+

.. tip::

    使用Apache的 ``mod_rewrite`` （或者其他Web服务器相同的功能）
    访问URLs可以很容易被美化成 ``/``, ``/contact`` 和``/blog``。

现在，每次请求完全以相同方式进行处理。 不是每个URLs访问执行不同的PHP文件，
*始终*执行前端控制器（front controller）， 不同URLs路由至应用不同的地方进行内部处理。
这样可以解决传统方法产生的两个问题。当今的Web应用都这么做，包括WordPress。

保持良好的代码组织
~~~~~~~~~~~~~~~

在前端控制器中，你需要指定哪些代码需要执行，哪些内容需要被返回。为了指定这些，你将
需要检查来访的URI，根据其值执行不同的代码逻辑。这很快能粗陋地做到::

    // index.php
    use Symfony\Component\HttpFoundation\Request;
    use Symfony\Component\HttpFoundation\Response;

    $request = Request::createFromGlobals();
    $path = $request->getPathInfo(); // 请求地不带参数的URI

    if (in_array($path, array('', '/'))) {
        $response = new Response('Welcome to the homepage.');
    } elseif ('/contact' === $path) {
        $response = new Response('Contact us');
    } else {
        $response = new Response('Page not found.', Response::HTTP_NOT_FOUND);
    }
    $response->send();

解决这种问题可能比较困难。幸运的是Symfony设计出来，*正是*是为了这。


Symfony应用程序执行流程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

当让Symfony处理每个请求的时候，工作就如此轻松。Symfony遵循这种相同而又简单的方式来处理请求。


.. _request-flow-figure:

.. figure:: /images/request-flow.png
   :align: center
   :alt: Symfony请求流程

   来访请求通过路由机制被解释，传递至控制器的返回 ``Response`` 对象函数。


站点的每个 “页面” 在路由配置中定义，不同URL地址映射到不同的PHP函数。
称作 :term:`控制器（controller）` 的PHP函数，它的工作是利用请求信息 － 结合Symfony提供的许多其他工具 － 来创建和返回 ``Response`` 对象。
也就是说，控制器是*你*编写代码逻辑的地方：这里解释请求并创建响应。

就是这么简单！回顾一下：

* 每个请求执行同一个前端控制器文件；

* 根据请求信息以及你创建的路由配置, 路由系统决定哪个PHP函数需要执行；

* 正确执行的PHP函数，是你创建并返回相应的 ``Response`` 对象逻辑的地方。


在Action中完成一个Symfony请求
~~~~~~~~~~~~~~~~~~~~~~~~~~~

无需切分成太多细节，这里就是在Action中的处理过程。
假设你想要在Symfony应用中添加一个 ``/contact`` 页面。
首先，在路由配置文件中，添加一个 ``/contact`` 的入口：

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
在 :doc:`路由章节 </book/routing>` 中得知，``AcmeDemoBundle:Main:contact`` 
是一个简短语法，指向类 ``MainController`` 的PHP函数 ``contactAction``::


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

在这个非常简单的例子中，控制器简单的创建了一个含有内容 ``<h1>Contact us!</h1>`` 的 :class:`Symfony\\Component\\HttpFoundation\\Response` 对象。 
在 :doc:`控制器章节 </book/controller>` 中，将学习如何让控制器输出模版，
让“呈现层" 代码（譬如，输出HTML）存储在独立的模版文件中。
这样可以解放控制器去注重更重要的部分：如何跟数据库交互，处理提交数据，或者发送邮件消息。


.. _symfony2-build-your-app-not-your-tools:

Symfony: 开发你的应用，而不是你的工具
---------------------------------------

你现在应该知道，任何应用目标都是为了解释来访的每个请求，并创建一个针对性的响应。当应用慢慢变大了，
代码组织和维护就会变得更加困难。不变的是，同样复杂的任务都是这个套路：保存数据到数据库，
输出和重用模版，处理提交，发送邮件，验证用户输入和处理安全问题。

好消息是，不是每一个问题是唯一的。Symfony提供一个拥有许多功能的框架，让你开发应用，无需你的工具。
使用Symfony，没有什么需要强制你做什么：你可以很自由的使用Symfony框架，
也可以只使用Symfony中你认为有用的部分。

.. index::
   single: Symfony组件（ Components）

.. _standalone-tools-the-symfony2-components:


独立工具包: Symfony *组件（Components）*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

那么Symfony究竟*是*什么？ 首先，Symfony是一个拥有20多个独立类库的集合体，这些类库可以在*任何*
PHP项目中使用。这些类库，称作 Symfony*组件（Components）*，包含几乎任何场合所需的许多实用的东西，
不管你的项目是怎么开发的。 这里提及一些：

* :doc:`HttpFoundation </components/http_foundation/introduction>` - 包含
``Request`` 和 ``Response`` 类, 还有处理Sessions和文件上传等等的其他类;

* :doc:`(路由)Routing </components/routing/introduction>` - 强大而又高效的路由系统，
  允许你映射指定URI地址(譬如 ``/contact``) 到请求如何处理的某些信息。
  (譬如，执行 ``contactAction()`` 方法）;

* :doc:`(表单)Form </components/form/introduction>` - 一个创建表单和处理表单提交，
  具备全特性和伸缩性的架构;

* `验证器(Validator)`_ - 一个创建数据规则和验证用户提交数据是否遵循这些规则的系统

* :doc:`(模版引擎) Templating </components/templating/introduction>` - 一个输出模版，
  处理模版继承（譬如，一个模版驻留在某个布局（layout）中）和 处理其他常规模版任务的工具包。

* :doc:`(安全)Security </components/security/introduction>` - 一个处理应用中所有类型安全问题的强大类库。

* :doc:`(翻译)Translation </components/translation/introduction>` - 一个在应用中处理字符内容翻译的框架.

不管你是使用Symfony框架，或者不是，任何这些组件中的一个，都是分离型(decoupled)，
并且可以在*任何*PHP项目中使用。如果需要，每个部分都可以拿来使用，拿来替换。

.. _the-full-solution-the-symfony2-framework:


完整解决方案: Symfony*框架*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

那么，什么是Symfony*框架*呢？*Symfony框架＊是一个PHP类库，用于完成两个不同任务：

#. 提供一套精选的组件（譬如，Symfony组件）和第三方类库（譬如，用于发送邮件的 `Swift Mailer`_ 类库）

#. 提供精心考究过的配置，和捆绑所有这些的“胶水"类库。

框架目标是整合许多独立工具，为了给开发员提供一致性体验。即使框架本身，也是一种Symfony模块（Bundle,一种插件 ），它完全可以被配置和替换。

Symfony提供一套强大的工具集合，用于快速开发Web应用，不需要难为你自己去做这些。普通用户也可以很快
开始使用Symfony分布包进行开发，它提供一个项目的基础体系架构，包含精心考究的默认配置。对于高级用户，
就看你的能耐如何了。


.. _`xkcd`: http://xkcd.com/
.. _`HTTP 1.1 RFC`: http://www.w3.org/Protocols/rfc2616/rfc2616.html
.. _`HTTP Bis`: http://datatracker.ietf.org/wg/httpbis/
.. _`Live HTTP Headers`: https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/
.. _`HTTP状态码列表`: http://en.wikipedia.org/wiki/List_of_HTTP_status_codes
.. _`HTTP头信息列表`: http://en.wikipedia.org/wiki/List_of_HTTP_header_fields
.. _`常用媒体资源类型列表`: http://en.wikipedia.org/wiki/
.. _`验证器`: https://github.com/symfony/Validator
.. _`Swift Mailer`: http://swiftmailer.org/
