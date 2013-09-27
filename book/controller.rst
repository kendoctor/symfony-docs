.. index::
   single: Controller

控制器
==========

控制器是你创建的一个PHP函数值，从HTTP请求和构造中获取信息并返回一个HTTP响
应（作为一个Symfony2 的``Response``对象）。这个响应可能是一个HTML页面，一个XML文档，
一个序列化的JSON数组，一个图像，一个重定向，一个404错误页面或其他任何你可以想象到的。
控制器包含了*你的应用程序*需要的任意逻辑来渲染页面内容。

看到在运行的symfony2控制器，你就知道这多么简单。下面的控制器将渲染一个印有``Hello World!``的页面::

    use Symfony\Component\HttpFoundation\Response;

    public function helloAction()
    {
        return new Response('Hello world!');
    }

控制器的目标一直是相同的：创建和返回一个``Response``对象。这个过程中，它可能从请求中
读取信息，加载资源数据库，发送电子邮件，或设置用户的会话信息。但是，在所有情况下，
控制器终将返回``Response``对象，该对象再被传回客户端。

没有不可思议的事和其他要求需要担心！这里有一些常见的例子：

* *控制器 A* 准备一个代表网页内容的``Response``对象。

* *控制器 B*从请求中读取``slug``参数，来从数据库中加载一个博客条目，并创建一个
``Response``展示那个博客。如果在数据库中找不到``slug``，它将创建并返回一个
``Response``对象和一个404状态代码。

* *控制器 C*处理一个联系人表单的提交。它从请求中读取信息，保存联系人信息到数据库，
并以电子邮件形式发送给网络管理员。最后，它创建一个``Response``对象，重新定向客户端的
浏览器到联系人表单的“谢谢您”页面。

.. index::
   single: Controller; Request-controller-response lifecycle

请求，控制器，响应生命周期
----------------------------------------

Symfony2项目处理的每个请求都经历同样简单地生命周期。这个框架负责重复的任务，最终执行
一个控制器，里面有你的自定义应用程序代码：

#. 每个请求都是由一个单独的前端控制器文件引导应用程序处理（如``app.php``或``app_dev.php``）；

#. ``Router``从请求中读取信息（例如，URL），找到与信息相配的路由，从路由中路读取``_controller``参数；

#. 匹配路由的控制器被执行，控制器中的代码创建并返回一个``Response``对象；

#. HTTP头部和``Response``对象的内容被送回客户端。

创建一个页面就跟创建一个控制器(#3)，制造一个将URL映射到控制器(#2)的路由一样容易。

.. note::

    尽管名字相似，“前端控制器”与本章节谈到的“控制器”是不同的。一个前端控制器是你网页目录下的
    一个短的PHP文件，所有的请求都通过它定向。一个典型的应用程序会有一个生产前端控制器
    （如``app.php``）和一个开发前端控制器（如``app_dev.php``）。你可能永远都不需要
    去编辑，查看或是担心你应用程序中的前端控制器。

.. index::
   single: Controller; Simple example

一个简单的控制器
-------------------

虽然一个控制器可以是任何PHP调用（一个函数，用于对象的方法，或一个``Closure``），
在Symfony2中，控制器通常是一个控制器对象中的建议方法。控制器也被成为*动作*。

.. code-block:: php
    :linenos:

    // src/Acme/HelloBundle/Controller/HelloController.php
    namespace Acme\HelloBundle\Controller;

    use Symfony\Component\HttpFoundation\Response;

    class HelloController
    {
        public function indexAction($name)
        {
            return new Response('<html><body>Hello '.$name.'!</body></html>');
        }
    }

.. tip::

    注意，*控制器*是``indexAction``方法，存在于一个*控制器类* (``HelloController``) 中。
    不要被名字迷惑：一个*控制器类*只是一个将几个控制器/动作分组的方便的方法。
    通常情况下，控制器类将容纳几个控制器/动作(如``updateAction``, ``deleteAction``等)。

控制器非常简单：

* *第4行*：Symfony2利用PHP5.3的命名空间功能来命名空间整个控制器类。控制器必须返回的
   ``Response``类由关键词``use``导入。

* *第6行*：类名是控制器类名字即（``Hello``）与``Controller``的串联。这是一个惯例，
   给控制器提供了持续性，允许他们在路由配置中被引用名称的第一部份（即``Hello``）。
* *第8行*：控制器类中的每个动作后缀为``Action``，并在路由配置中被动作的名字(``index``)
   引用。在下一节中，你将创建一个映射URL到这个动作中的路由。你会学习路由的占位符如何
   变成动作方法(``$name``)的参数的。
* *第10行*：控制器创建并返回一个``Response``对象。

.. index::
   single: Controller; Routes and controllers

将一个URL映射到控制器
-----------------------------

新的控制器返回一个简单的HTML页面。要真正在你的浏览器中查看这个页面，你需要创建
一个路由，将一个特定的URL路径映射到控制器：

.. configuration-block::

    .. code-block:: yaml

        # app/config/routing.yml
        hello:
            path:      /hello/{name}
            defaults:  { _controller: AcmeHelloBundle:Hello:index }

    .. code-block:: xml

        <!-- app/config/routing.xml -->
        <route id="hello" path="/hello/{name}">
            <default key="_controller">AcmeHelloBundle:Hello:index</default>
        </route>

    .. code-block:: php

        // app/config/routing.php
        $collection->add('hello', new Route('/hello/{name}', array(
            '_controller' => 'AcmeHelloBundle:Hello:index',
        )));

访问``/hello/ryan`` 现在执行``HelloController::indexAction()``控制器，
并为 ``$name``变量进入``ryan``。创建一个“页面”只意味着创建一个控制器和相关连的路由。

请注意用来指代控制器的句法：``AcmeHelloBundle:Hello:index``。Symfony2使用灵活的
字符串来表示不同的控制器。这是最常见的句法，让Symfony2在``AcmeHelloBundle``包中
寻找名叫``HelloController``的控制器类。然后``indexAction()``方法被执行。

关于用于引用不同控制器的字符串的详细信息，请参阅:ref:`controller-string-syntax`。

.. note::

    这个例子直接把路由配置放在``app/config/``目录中。组织你的路由的更好的办法
    是将每个路由放在它所属的包中。更多相关信息，请参阅 :ref:`routing-include-external-resources`。

.. tip::

    你可以在:doc:`Routing chapter</book/routing>`中学习更多关于路由系统。

.. index::
   single: Controller; Controller arguments

.. _route-parameters-controller-arguments:

路由参数作为控制器参数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

你已经知道``_controller``参数 ``AcmeHelloBundle:Hello:index``在``AcmeHelloBundle``包中
是指一个``HelloController::indexAction()``方法。更有趣的是传递到那个方法的参数::

    // src/Acme/HelloBundle/Controller/HelloController.php
    namespace Acme\HelloBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;

    class HelloController extends Controller
    {
        public function indexAction($name)
        {
          // ...
        }
    }

控制器有一个单一的参数，``$name``跟匹配的路由（在此例中是``ryan``）中的``{name}``参数相对应。
实际上，当执行你的控制器时，Symfony2的一个相应的路由参数与控制器的每个参数匹配。看看下面的例子:

.. configuration-block::

    .. code-block:: yaml

        # app/config/routing.yml
        hello:
            path:      /hello/{first_name}/{last_name}
            defaults:  { _controller: AcmeHelloBundle:Hello:index, color: green }

    .. code-block:: xml

        <!-- app/config/routing.xml -->
        <route id="hello" path="/hello/{first_name}/{last_name}">
            <default key="_controller">AcmeHelloBundle:Hello:index</default>
            <default key="color">green</default>
        </route>

    .. code-block:: php

        // app/config/routing.php
        $collection->add('hello', new Route('/hello/{first_name}/{last_name}', array(
            '_controller' => 'AcmeHelloBundle:Hello:index',
            'color'       => 'green',
        )));

The controller for this can take several arguments:: 该控制器可以采取以下几个参数::

    public function indexAction($first_name, $last_name, $color)
    {
        // ...
    }

请注意，两个占位符变量(``{first_name}``, ``{last_name}``)以及默认的``color``变量都跟控制器中的
参数一样有用。当一个路由匹配时，占位符变量与``defaults``合并，形成一个你的控制器可以使用的一个数组。

将路由参数映射到控制器参数很灵活容易。当你开发时请用心记住以下原则。

* **控制器参数的顺序不重要**

    从路由到控制器方法签名中的变量名称，Symfony都能够匹配参数名称。换句话说，它实现了`
    `{last_name}``参数与``$last_name``相匹配。控制器的参数完全可以重新排序并且很好地工作::

        public function indexAction($last_name, $color, $first_name)
        {
            // ...
        }

* **每个所需的控制器参数必须匹配路由参数**

    下面将讲到一个``RuntimeException``因为路由中没有定义``foo``参数::

        public function indexAction($first_name, $last_name, $color, $foo)
        {
            // ...
        }

    然而，使参数可选是非常好的。下面的例子就不会抛出异常::

        public function indexAction($first_name, $last_name, $color, $foo = 'bar')
        {
            // ...
        }

* **并非所有路由参数需要成为你控制器上的参数**

    如果，例如，``last_name``对你的控制器来说不重要，你可以完全忽略它::

        public function indexAction($first_name, $color)
        {
            // ...
        }

.. tip::

    每个路由还具有特殊的``_route``参数，这等同于匹配的路由的名称（如``hello``）。
    虽然不是经常有用，这同样可以作为一个控制器参数。

.. _book-controller-request-argument:

``Request``作为控制器参数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

为方便起见，你也可以让Symfony给你传递``Request``对象作为你控制器的参数。这在你工作中处理
表单时特别方便，例如::

    use Symfony\Component\HttpFoundation\Request;

    public function updateAction(Request $request)
    {
        $form = $this->createForm(...);

        $form->handleRequest($request);
        // ...
    }

.. index::
   single: Controller; Base controller class

创建静态页面
---------------------

你可以创建一个静态页面，甚至不用创建控制器（只需要一个路由和模板）。

使用它吧！请参阅:doc:`/cookbook/templating/render_without_controller`。

控制器基类
-------------------------

为方便起见，Symfony2配备了``Controller``基类，协助一些最常见的控制器任务，并让你的
控制器类访问它需要的任何资源。通过扩展这个``Controller``类，你可以利用几个辅助方法。

在``Controller``类上添加``use``语句，然后修改``HelloController``，将其扩大::

    // src/Acme/HelloBundle/Controller/HelloController.php
    namespace Acme\HelloBundle\Controller;

    use Symfony\Bundle\FrameworkBundle\Controller\Controller;
    use Symfony\Component\HttpFoundation\Response;

    class HelloController extends Controller
    {
        public function indexAction($name)
        {
            return new Response('<html><body>Hello '.$name.'!</body></html>');
        }
    }

这实际上一点都不改变控制器如何工作的。在下一节，你将学习利用控制器基类可以使用的辅助方法。
这些方法只是使用Symfony2的核心功能，让你用不用``Controller``基类都可以使用这些功能的快捷方式。
要看在运作的核心功能的好办法是查看:class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`类本身。

.. tip::

    扩展基类在Symfony中是*可选的*；它包含有用的快捷方式但是没有强制性。
    你也可以扩展:class:`Symfony\\Component\\DependencyInjection\\ContainerAware`。
    服务容器对象届时会通过``container``属性可以使用。

.. note::

    你还可以定义你的:doc:`Controllers as Services</cookbook/controller/service>。

.. index::
   single: Controller; Common tasks

常见的控制器任务
-----------------------

尽管一个控制器几乎能够做任何事，大多数控制器将一遍又一遍地执行相同的基本任务。
这些任务，比如重定向，转发，渲染模板和访问核心服务，在Symfony2中很好管理。

.. index::
   single: Controller; Redirecting

重定向
~~~~~~~~~~~

如果你像重定向用户到另一个页面，就用``redirect()``方法::

    public function indexAction()
    {
        return $this->redirect($this->generateUrl('homepage'));
    }

``generateUrl()``方法只是一个辅助功能，为一个给定的路由生成URL。更多信息
请参阅:doc:`Routing </book/routing>`
chapter。

默认情况下，``redirect()``方法执行一个302（临时的）重定向。要执行一个301（永恒的）
重定向，修改第二个参数::

    public function indexAction()
    {
        return $this->redirect($this->generateUrl('homepage'), 301);
    }

.. tip::

    ``redirect()``方法只是一个快捷方式，创建一个专门将用户重定向的``Response``对象。
    这相当于::

        use Symfony\Component\HttpFoundation\RedirectResponse;

        return new RedirectResponse($this->generateUrl('homepage'));

.. index::
   single: Controller; Forwarding

转发
~~~~~~~~~~

你也可以很轻松地用``forward()``方法转发到另一个控制器内部。它生成一个内部子请求，并条用指定的
控制器，而不是重定向用户的浏览器。``forward()``方法返回了已经从那个控制器返回的``Response``对象::

    public function indexAction($name)
    {
        $response = $this->forward('AcmeHelloBundle:Hello:fancy', array(
            'name'  => $name,
            'color' => 'green',
        ));

        // ... further modify the response or return it directly

        return $response;
    }

请注意，`forward()`方法使用代表路由配置中控制器的同样的字符串。在这个例子中，
目标控制器类将是一些``AcmeHelloBundle``中的``HelloController``。传送给方法的
数组变成结果控制器上的参数。这个同样的接口在将控制器植入模板时使用
（参阅:ref:`templating-embedding-controller`）。目标控制器方法应该看上去像下面这样::

    public function fancyAction($name, $color)
    {
        // ... create and return a Response object
    }

就像为一个路由创建控制器时一样，``fancyAction``参数的顺序并不重要。Symfony2将
方法参数名称（如``$name``）与索引键名称（如``name``）相匹配。如果你改变参数的顺序，
Symfony2仍然会传递正确的值给每个变量。

.. tip::

    像其他基本``Controller``方法，``forward``方法只是一个Symfony2核心功能的快捷方式。
    转发可以直接通过复制当前的请求来实现当这个子请求通过``http_kernel``服务被执行，
    HttpKernel返回一个``Response``对象。::::

        $httpKernel = $this->container->get('http_kernel');
        $response = $httpKernel->forward(
            'AcmeHelloBundle:Hello:fancy',
            array(
                'name'  => $name,
                'color' => 'green',
            )
        );

.. index::
   single: Controller; Rendering templates

.. _controller-rendering-templates:

渲染模板
~~~~~~~~~~~~~~~~~~~

尽管不是一个要求，大多数的控制器最终会渲染一个负责为控制器生成HTML（或者其他格式）的模板。
``renderView()``方法渲染一个模板并返回它的内容。模板的内容可以用来创建一个``Response``对象::

    use Symfony\Component\HttpFoundation\Response;

    $content = $this->renderView(
        'AcmeHelloBundle:Hello:index.html.twig',
        array('name' => $name)
    );

    return new Response($content);

甚至只用一步就可以完成，就用``render()``方法返回一个包含着模板内容的``Response``对象::

    return $this->render(
        'AcmeHelloBundle:Hello:index.html.twig',
        array('name' => $name)
    );

这两种情况下，``AcmeHelloBundle``中的``Resources/views/Hello/index.html.twig``模板将被渲染。

Symfony模板引擎在:doc:`Templating </book/templating>`章节中有详细解释。

.. tip::

    使用``@Template``注释，你甚至可以避免调用``render``方法。更多细节
    请参阅:doc:`FrameworkExtraBundle documentation</bundles/SensioFrameworkExtraBundle/annotations/view>`

.. tip::

    ``renderView``方法是直接使用``templating``服务的快捷方式。也可以直接使用``templating``服务::

        $templating = $this->get('templating');
        $content = $templating->render(
            'AcmeHelloBundle:Hello:index.html.twig',
            array('name' => $name)
        );

.. note::

    在更小的子目录中渲染模板也是有可能的，但是小心避免陷阱，使你的目录结构过分地阐述。::

        $templating->render(
            'AcmeHelloBundle:Hello/Greetings:index.html.twig',
            array('name' => $name)
        );
        // index.html.twig found in Resources/views/Hello/Greetings is rendered.

.. index::
   single: Controller; Accessing services

访问其他服务
~~~~~~~~~~~~~~~~~~~~~~~~

当你扩展控制器基类时，你可以通过``get()``方法访问任何Symfony2服务。
这里有几个你可能需要的常见的服务::

    $request = $this->getRequest();

    $templating = $this->get('templating');

    $router = $this->get('router');

    $mailer = $this->get('mailer');

有数不清的别的服务可以使用，你要鼓起勇气定义你自己的服务。要列出所有可以使用的服务，
就用``container:debug``控制台命令:

.. code-block:: bash

    $ php app/console container:debug

For more information, see the :doc:`/book/service_container` chapter.

.. index::
   single: Controller; Managing errors
   single: Controller; 404 pages

管理错误和404页面
-----------------------------

当你找不到东西时，你应该好好使用HTTP歇息，并返回一个404响应。要做到这个，
你将抛出一个特殊类型的异常。如果你正在扩展控制器基类，做以下的操作::

    public function indexAction()
    {
        // retrieve the object from database
        $product = ...;
        if (!$product) {
            throw $this->createNotFoundException('The product does not exist');
        }

        return $this->render(...);
    }

``createNotFoundException()`` 方法创建一个特殊的``NotFoundHttpException``对象，
最终在Symfony中导致一个404HTTP响应。

当然，你可以在你的控制器中随意抛出任何``Exception``类 - Symfony2将自动返回一个500HTTP响应代码。

.. code-block:: php

    throw new \Exception('Something went wrong!');

在每种情况下，风格错误页面是给最终用户显示的，一个完整的调试错误页面是给开发商
显示的（在调试模式下查看页面时）。这两种错误页面都可以被自定义。有关详细信息，
参阅“:doc:`/cookbook/controller/error_pages`”cookbook诀窍。

.. index::
   single: Controller; The session
   single: Session

管理会话
--------------------

Symfony2提供了一个很好的会话对象，你可以在请求中用它来储存用户信息（假使一个真实的人使用
浏览器，机器人或web服务）。默认情况下，Symfony2通过使用本地PHP会话将其属性存储在cookie中。

从会话中存储和检索信息可以从任何控制器中轻松实现::

    $session = $this->getRequest()->getSession();

    // store an attribute for reuse during a later user request
    $session->set('foo', 'bar');

    // in another controller for another request
    $foo = $session->get('foo');

    // use a default value if the key doesn't exist
    $filters = $session->get('filters', array());

这些属性将保留在那个用户会话的其余用户上。

.. index::
   single: Session; Flash messages

Flash Messages闪存消息
~~~~~~~~~~~~~~

你还可以为一个额外的请求存储将被存储在用户会话上的小消息。处理一个表单时，这很有帮
助：你想要重新定向，并有一个特殊的消息显示在*下一个*请求上。这些类型的消息称为“闪存”消息。

例如，想象一下你正在处理一个表单提交::

    public function updateAction()
    {
        $form = $this->createForm(...);

        $form->handleRequest($this->getRequest());

        if ($form->isValid()) {
            // do some sort of processing

            $this->get('session')->getFlashBag()->add('notice', 'Your changes were saved!');

            return $this->redirect($this->generateUrl(...));
        }

        return $this->render(...);
    }

处理请求后，控制器设置一个``notice``闪存消息，然后重新定向。这个名称（``notice``）
没有意义 - 它只是被你用来辨别消息类型的。

在模板的下一个动作中，以下代码可能被用来渲染``notice``消息：

.. configuration-block::

    .. code-block:: html+jinja

        {% for flashMessage in app.session.flashbag.get('notice') %}
            <div class="flash-notice">
                {{ flashMessage }}
            </div>
        {% endfor %}

    .. code-block:: html+php

        <?php foreach ($view['session']->getFlashBag()->get('notice') as $message): ?>
            <div class="flash-notice">
                <?php echo "<div class='flash-error'>$message</div>" ?>
            </div>
        <?php endforeach; ?>


根据设计，闪存消息注定要准确地指定一个请求（他们“瞬间消失”）。正如你在这个例子中所做的，
他们被设计用于交互重定向。

.. index::
   single: Controller; Response object

The Response Object响应对象
-------------------

控制器唯一的要求就是返回一个``Response``对象。:class:`Symfony\\Component\\HttpFoundation\\Response`类
是一个HTTP响应周围的PHP抽象 - 基于文本的消息充满了HTTP头部和被送回给客户端的内容::

    use Symfony\Component\HttpFoundation\Response;

    // create a simple Response with a 200 status code (the default)
    $response = new Response('Hello '.$name, 200);

    // create a JSON-response with a 200 status code
    $response = new Response(json_encode(array('name' => $name)));
    $response->headers->set('Content-Type', 'application/json');

.. tip::

    ``headers``属性是一个:class:`Symfony\\Component\\HttpFoundation\\HeaderBag`对象
    ，带有几个对于阅读和变异``Response``头有用的方法。头部名称被规范化，以便使用``Content-Type``
    相当于`content-type``，甚至`content_type``。

.. tip::

    也有特殊类，使某些类型的响应更容易：

    - 对于JSON，有:class:`Symfony\\Component\\HttpFoundation\\JsonResponse`。
      请参阅:ref:`component-http-foundation-json-response`。
    - 对于文件，有:class:`Symfony\\Component\\HttpFoundation\\BinaryFileResponse`。
      请参阅:ref:`component-http-foundation-serving-files`。

.. index::
   single: Controller; Request object

请求对象
------------------

扩展``Controller``基类时，除了路由占位符的值，控制器还可以访问``Request``对象::

    $request = $this->getRequest();

    $request->isXmlHttpRequest(); // is it an Ajax request?

    $request->getPreferredLanguage(array('en', 'fr'));

    $request->query->get('page'); // get a $_GET parameter

    $request->request->get('page'); // get a $_POST parameter

跟``Response``对象一样，请求头存储在一个`HeaderBag``对象中，很方便使用。

结语
--------------

每当你创建一个页面，你最终会需要编写一些包含逻辑的代码。在Symfony中，这被叫做控制器，
它是一个PHP函数，能够做一切它需要做的事，来返回最终将被返回给用户的``Response``对象。

为了使生活更轻松，你可以选择扩展一个``Controller``基类，它包含许多常见控制器任务的快捷方式。
例如，既然你不想把HTML代码放在你的控制器中，你可以使用``render()``方法来渲染并从一个模板返回内容。

在其它章节中，你会看到，控制器是如何被用来从数据库中坚持并获取对象，进行表单提交，处理缓存等等。

从Cookbook中了解更多
----------------------------

* :doc:`/cookbook/controller/error_pages`
* :doc:`/cookbook/controller/service`
