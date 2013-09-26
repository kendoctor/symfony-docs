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

Every request handled by a Symfony2 project goes through the same simple lifecycle.
The framework takes care of the repetitive tasks and ultimately executes a
controller, which houses your custom application code: Symfony2项目处理的每个请求都经历同样简单地生命周期。这个框架负责重复的任务，最终执行一个控制器，里面有你的自定义应用程序代码：

#. Each request is handled by a single front controller file (e.g. ``app.php``
   or ``app_dev.php``) that bootstraps the application; #. 每个请求都是由一个单独的前端控制器文件引导应用程序处理（如``app.php``或``app_dev.php``）；

#. The ``Router`` reads information from the request (e.g. the URI), finds
   a route that matches that information, and reads the ``_controller`` parameter
   from the route; #. ``Router``从请求中读取信息（例如，URL），找到与信息相配的路由，从路由中路读取``_controller``参数；

#. The controller from the matched route is executed and the code inside the
   controller creates and returns a ``Response`` object; #. 匹配路由的控制器被执行，控制器中的代码创建并返回一个``Response``对象；

#. The HTTP headers and content of the ``Response`` object are sent back to
   the client. #. HTTP头部和``Response``对象的内容被送回客户端。

Creating a page is as easy as creating a controller (#3) and making a route that
maps a URL to that controller (#2).创建一个页面就跟创建一个控制器(#3)，制造一个将URL映射到控制器(#2)的路由一样容易。

.. note::

    Though similarly named, a "front controller" is different from the
    "controllers" talked about in this chapter. A front controller
    is a short PHP file that lives in your web directory and through which
    all requests are directed. A typical application will have a production
    front controller (e.g. ``app.php``) and a development front controller
    (e.g. ``app_dev.php``). You'll likely never need to edit, view or worry
    about the front controllers in your application.尽管名字相似，“前端控制器”与本章节谈到的“控制器”是不同的。一个前端控制器是你网页目录下的一个短的PHP文件，所有的请求都通过它定向。一个典型的应用程序会有一个生产前端控制器（如``app.php``）和一个开发前端控制器（如``app_dev.php``）。你可能永远都不需要去编辑，查看或是担心你应用程序中的前端控制器。

.. index::
   single: Controller; Simple example

一个简单的控制器
-------------------

While a controller can be any PHP callable (a function, method on an object,
or a ``Closure``), in Symfony2, a controller is usually a single method inside
a controller object. Controllers are also called *actions*.虽然一个控制器可以是任何PHP调用（一个函数，用于对象的方法，或一个``Closure``），在Symfony2中，控制器通常是一个控制器对象中的建议方法。控制器也被成为*动作*。

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

    Note that the *controller* is the ``indexAction`` method, which lives
    inside a *controller class* (``HelloController``). Don't be confused
    by the naming: a *controller class* is simply a convenient way to group
    several controllers/actions together. Typically, the controller class
    will house several controllers/actions (e.g. ``updateAction``, ``deleteAction``,
    etc).注意，*控制器*是``indexAction``方法，存在于一个*控制器类* (``HelloController``)中。不要被名字迷惑：一个*控制器类*只是一个将几个控制器/动作分组的方便的方法。通常情况下，控制器类将容纳几个控制器/动作(如``updateAction``, ``deleteAction``等)。

This controller is pretty straightforward:控制器非常简单：

* *line 4*: Symfony2 takes advantage of PHP 5.3 namespace functionality to
  namespace the entire controller class. The ``use`` keyword imports the
  ``Response`` class, which the controller must return.* *第4行*：Symfony2利用PHP5.3的命名空间功能来命名空间整个控制器类。控制器必须返回的``Response``类由关键词``use``导入。

* *line 6*: The class name is the concatenation of a name for the controller
  class (i.e. ``Hello``) and the word ``Controller``. This is a convention
  that provides consistency to controllers and allows them to be referenced
  only by the first part of the name (i.e. ``Hello``) in the routing configuration.* *第6行*：类名是控制器类名字即（``Hello``）与``Controller``的串联。这是一个惯例，给控制器提供了持续性，允许他们在路由配置中被引用名称的第一部份（即``Hello``）。

* *line 8*: Each action in a controller class is suffixed with ``Action``
  and is referenced in the routing configuration by the action's name (``index``).
  In the next section, you'll create a route that maps a URI to this action.
  You'll learn how the route's placeholders (``{name}``) become arguments
  to the action method (``$name``).* *第8行*：控制器类中的每个动作后缀为``Action``，并在路由配置中被动作的名字(``index``)引用。在下一节中，你将创建一个映射URL到这个动作中的路由。你会学习路由的占位符如何变成动作方法(``$name``)的参数的。

* *line 10*: The controller creates and returns a ``Response`` object.* *第10行*：控制器创建并返回一个``Response``对象。

.. index::
   single: Controller; Routes and controllers

将一个URL映射到控制器
-----------------------------

The new controller returns a simple HTML page. To actually view this page
in your browser, you need to create a route, which maps a specific URL path
to the controller:新的控制器返回一个简单的HTML页面。要真正在你的浏览器中查看这个页面，你需要创建一个路由，将一个特定的URL路径映射到控制器：

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

Going to ``/hello/ryan`` now executes the ``HelloController::indexAction()``
controller and passes in ``ryan`` for the ``$name`` variable. Creating a
"page" means simply creating a controller method and associated route.访问``/hello/ryan`` 现在执行``HelloController::indexAction()``控制器，并为 ``$name``变量进入``ryan``。创建一个“页面”只意味着创建一个控制器和相关连的路由。

Notice the syntax used to refer to the controller: ``AcmeHelloBundle:Hello:index``.
Symfony2 uses a flexible string notation to refer to different controllers.
This is the most common syntax and tells Symfony2 to look for a controller
class called ``HelloController`` inside a bundle named ``AcmeHelloBundle``. The
method ``indexAction()`` is then executed.请注意用来指代控制器的句法：``AcmeHelloBundle:Hello:index``。Symfony2使用灵活的字符串来表示不同的控制器。这是最常见的句法，让Symfony2在``AcmeHelloBundle``包中寻找名叫``HelloController``的控制器类。然后``indexAction()``方法被执行。

For more details on the string format used to reference different controllers,
see :ref:`controller-string-syntax`.关于用于引用不同控制器的字符串的详细信息，请参阅:ref:`controller-string-syntax`。

.. note::

    This example places the routing configuration directly in the ``app/config/``
    directory. A better way to organize your routes is to place each route
    in the bundle it belongs to. For more information on this, see
    :ref:`routing-include-external-resources`.这个例子直接把路由配置放在``app/config/``目录中。组织你的路由的更好的办法是将每个路由放在它所属的包中。更多相关信息，请参阅 :ref:`routing-include-external-resources`。

.. tip::

    You can learn much more about the routing system in the :doc:`Routing chapter</book/routing>`.你可以在:doc:`Routing chapter</book/routing>`中学习更多关于路由系统。

.. index::
   single: Controller; Controller arguments

.. _route-parameters-controller-arguments:

Route Parameters as Controller Arguments路由参数作为控制器参数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You already know that the ``_controller`` parameter ``AcmeHelloBundle:Hello:index``
refers to a ``HelloController::indexAction()`` method that lives inside the
``AcmeHelloBundle`` bundle. What's more interesting is the arguments that are
passed to that method::   你已经知道``_controller``参数 ``AcmeHelloBundle:Hello:index``在``AcmeHelloBundle``包中是指一个``HelloController::indexAction()``方法。更有趣的是传递到那个方法的参数::

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

The controller has a single argument, ``$name``, which corresponds to the
``{name}`` parameter from the matched route (``ryan`` in the example). In
fact, when executing your controller, Symfony2 matches each argument of
the controller with a parameter from the matched route. Take the following
example: 控制器有一个单一的参数，``$name``跟匹配的路由（在此例中是``ryan``）中的``{name}``参数相对应。实际上，当执行你的控制器时，Symfony2的一个相应的路由参数与控制器的每个参数匹配。看看下面的例子:

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

Notice that both placeholder variables (``{first_name}``, ``{last_name}``)
as well as the default ``color`` variable are available as arguments in the
controller. When a route is matched, the placeholder variables are merged
with the ``defaults`` to make one array that's available to your controller.请注意，两个占位符变量(``{first_name}``, ``{last_name}``)以及默认的``color``变量都跟控制器中的参数一样有用。当一个路由匹配时，占位符变量与``defaults``合并，形成一个你的控制器可以使用的一个数组。

Mapping route parameters to controller arguments is easy and flexible. Keep
the following guidelines in mind while you develop.将路由参数映射到控制器参数很灵活容易。当你开发时请用心记住以下原则。

* **The order of the controller arguments does not matter控制器参数的顺序不重要**

    Symfony is able to match the parameter names from the route to the variable
    names in the controller method's signature. In other words, it realizes that
    the ``{last_name}`` parameter matches up with the ``$last_name`` argument.
    The arguments of the controller could be totally reordered and still work
    perfectly   从路由到控制器方法签名中的变量名称，Symfony都能够匹配参数名称。换句话说，它实现了``{last_name}``参数与``$last_name``相匹配。控制器的参数完全可以重新排序并且很好地工作::

        public function indexAction($last_name, $color, $first_name)
        {
            // ...
        }

* **Each required controller argument must match up with a routing parameter每个所需的控制器参数必须匹配路由参数**

    The following would throw a ``RuntimeException`` because there is no ``foo``
    parameter defined in the route  下面将讲到一个``RuntimeException``因为路由中没有定义``foo``参数::

        public function indexAction($first_name, $last_name, $color, $foo)
        {
            // ...
        }

    Making the argument optional, however, is perfectly ok. The following
    example would not throw an exception  然而，使参数可选是非常好的。下面的例子就不会抛出异常::

        public function indexAction($first_name, $last_name, $color, $foo = 'bar')
        {
            // ...
        }

* **Not all routing parameters need to be arguments on your controller并非所有路由参数需要成为你控制器上的参数**

    If, for example, the ``last_name`` weren't important for your controller,
    you could omit it entirely   如果，例如，``last_name``对你的控制器来说不重要，你可以完全忽略它::

        public function indexAction($first_name, $color)
        {
            // ...
        }

.. tip::

    Every route also has a special ``_route`` parameter, which is equal to
    the name of the route that was matched (e.g. ``hello``). Though not usually
    useful, this is equally available as a controller argument.每个路由还具有特殊的``_route``参数，这等同于匹配的路由的名称（如``hello``）。虽然不是经常有用，这同样可以作为一个控制器参数。

.. _book-controller-request-argument:

The ``Request`` as a Controller Argument  ``Request``作为控制器参数
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For convenience, you can also have Symfony pass you the ``Request`` object
as an argument to your controller. This is especially convenient when you're
working with forms, for example  为方便起见，你也可以让Symfony给你传递``Request``对象作为你控制器的参数。这在你工作中处理表单时特别方便，例如::

    use Symfony\Component\HttpFoundation\Request;

    public function updateAction(Request $request)
    {
        $form = $this->createForm(...);

        $form->handleRequest($request);
        // ...
    }

.. index::
   single: Controller; Base controller class

Creating Static Pages创建静态页面
---------------------

You can create a static page without even creating a controller (only a route
and template are needed).你可以创建一个静态页面，甚至不用创建控制器（只需要一个路由和模板）。

Use it! See :doc:`/cookbook/templating/render_without_controller`.使用它吧！请参阅:doc:`/cookbook/templating/render_without_controller`。

The Base Controller Class控制器基类
-------------------------

For convenience, Symfony2 comes with a base ``Controller`` class that assists
with some of the most common controller tasks and gives your controller class
access to any resource it might need. By extending this ``Controller`` class,
you can take advantage of several helper methods.为方便起见，Symfony2配备了``Controller``基类，协助一些最常见的控制器任务，并让你的控制器类访问它需要的任何资源。通过扩展这个``Controller``类，你可以利用几个辅助方法。

Add the ``use`` statement atop the ``Controller`` class and then modify the
``HelloController`` to extend it   在``Controller``类上添加``use``语句，然后修改``HelloController``，将其扩大::

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

This doesn't actually change anything about how your controller works. In
the next section, you'll learn about the helper methods that the base controller
class makes available. These methods are just shortcuts to using core Symfony2
functionality that's available to you with or without the use of the base
``Controller`` class. A great way to see the core functionality in action
is to look in the
:class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller` class
itself.这实际上一点都不改变控制器如何工作的。在下一节，你将学习利用控制器基类可以使用的辅助方法。这些方法只是使用Symfony2的核心功能，让你用不用``Controller``基类都可以使用这些功能的快捷方式。要看在运作的核心功能的好办法是查看:class:`Symfony\\Bundle\\FrameworkBundle\\Controller\\Controller`类本身。

.. tip::

    Extending the base class is *optional* in Symfony; it contains useful
    shortcuts but nothing mandatory. You can also extend
    :class:`Symfony\\Component\\DependencyInjection\\ContainerAware`. The service
    container object will then be accessible via the ``container`` property.扩展基类在Symfony中是*可选的*；它包含有用的快捷方式但是没有强制性。你也可以扩展:class:`Symfony\\Component\\DependencyInjection\\ContainerAware`。服务容器对象届时会通过``container``属性可以使用。

.. note::

    You can also define your :doc:`Controllers as Services</cookbook/controller/service>`.你还可以定义你的:doc:`Controllers as Services</cookbook/controller/service>。

.. index::
   single: Controller; Common tasks

Common Controller Tasks常见的控制器任务
-----------------------

Though a controller can do virtually anything, most controllers will perform
the same basic tasks over and over again. These tasks, such as redirecting,
forwarding, rendering templates and accessing core services, are very easy
to manage in Symfony2.尽管一个控制器几乎能够做任何事，大多数控制器将一遍又一遍地执行相同的基本任务。这些任务，比如重定向，转发，渲染模板和访问核心服务，在Symfony2中很好管理。

.. index::
   single: Controller; Redirecting

Redirecting重定向
~~~~~~~~~~~

If you want to redirect the user to another page, use the ``redirect()`` method   如果你像重定向用户到另一个页面，就用``redirect()``方法::

    public function indexAction()
    {
        return $this->redirect($this->generateUrl('homepage'));
    }

The ``generateUrl()`` method is just a helper function that generates the URL
for a given route. For more information, see the :doc:`Routing </book/routing>`
chapter.  ``generateUrl()``方法只是一个辅助功能，为一个给定的路由生成URL。更多信息请参阅:doc:`Routing </book/routing>`
chapter。

By default, the ``redirect()`` method performs a 302 (temporary) redirect. To
perform a 301 (permanent) redirect, modify the second argument    默认情况下，``redirect()``方法执行一个302（临时的）重定向。要执行一个301（永恒的）重定向，修改第二个参数::

    public function indexAction()
    {
        return $this->redirect($this->generateUrl('homepage'), 301);
    }

.. tip::

    The ``redirect()`` method is simply a shortcut that creates a ``Response``
    object that specializes in redirecting the user. It's equivalent to     ``redirect()``方法只是一个快捷方式，创建一个专门将用户重定向的``Response``对象。这相当于::

        use Symfony\Component\HttpFoundation\RedirectResponse;

        return new RedirectResponse($this->generateUrl('homepage'));

.. index::
   single: Controller; Forwarding

Forwarding转发
~~~~~~~~~~

You can also easily forward to another controller internally with the ``forward()``
method. Instead of redirecting the user's browser, it makes an internal sub-request,
and calls the specified controller. The ``forward()`` method returns the ``Response``
object that's returned from that controller  你也可以很轻松地用``forward()``方法转发到另一个控制器内部。它生成一个内部子请求，并条用指定的控制器，而不是重定向用户的浏览器。``forward()``方法返回了已经从那个控制器返回的``Response``对象::

    public function indexAction($name)
    {
        $response = $this->forward('AcmeHelloBundle:Hello:fancy', array(
            'name'  => $name,
            'color' => 'green',
        ));

        // ... further modify the response or return it directly

        return $response;
    }

Notice that the `forward()` method uses the same string representation of
the controller used in the routing configuration. In this case, the target
controller class will be ``HelloController`` inside some ``AcmeHelloBundle``.
The array passed to the method becomes the arguments on the resulting controller.
This same interface is used when embedding controllers into templates (see
:ref:`templating-embedding-controller`). The target controller method should
look something like the following   请注意，`forward()`方法使用代表路由配置中控制器的同样的字符串。在这个例子中，目标控制器类将是一些``AcmeHelloBundle``中的``HelloController``。传送给方法的数组变成结果控制器上的参数。这个同样的接口在将控制器植入模板时使用（参阅:ref:`templating-embedding-controller`）。目标控制器方法应该看上去像下面这样::

    public function fancyAction($name, $color)
    {
        // ... create and return a Response object
    }

And just like when creating a controller for a route, the order of the arguments
to ``fancyAction`` doesn't matter. Symfony2 matches the index key names
(e.g. ``name``) with the method argument names (e.g. ``$name``). If you
change the order of the arguments, Symfony2 will still pass the correct
value to each variable.就像为一个路由创建控制器时一样，``fancyAction``参数的顺序并不重要。Symfony2将方法参数名称（如``$name``）与索引键名称（如``name``）相匹配。如果你改变参数的顺序，Symfony2仍然会传递正确的值给每个变量。

.. tip::

    Like other base ``Controller`` methods, the ``forward`` method is just
    a shortcut for core Symfony2 functionality. A forward can be accomplished
    directly via the ``http_kernel`` service and returns a ``Response``
    object像其他基本``Controller``方法，``forward``方法只是一个Symfony2核心功能的快捷方式。转发可以直接通过复制当前的请求来实现当这个子请求通过``http_kernel``服务被执行，HttpKernel返回一个``Response``对象。::::

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

Rendering Templates渲染模板
~~~~~~~~~~~~~~~~~~~

Though not a requirement, most controllers will ultimately render a template
that's responsible for generating the HTML (or other format) for the controller.
The ``renderView()`` method renders a template and returns its content. The
content from the template can be used to create a ``Response`` object   尽管不是一个要求，大多数的控制器最终会渲染一个负责为控制器生成HTML（或者其他格式）的模板。``renderView()``方法渲染一个模板并返回它的内容。模板的内容可以用来创建一个``Response``对象::

    use Symfony\Component\HttpFoundation\Response;

    $content = $this->renderView(
        'AcmeHelloBundle:Hello:index.html.twig',
        array('name' => $name)
    );

    return new Response($content);

This can even be done in just one step with the ``render()`` method, which
returns a ``Response`` object containing the content from the template   ::

    return $this->render(
        'AcmeHelloBundle:Hello:index.html.twig',
        array('name' => $name)
    );

In both cases, the ``Resources/views/Hello/index.html.twig`` template inside
the ``AcmeHelloBundle`` will be rendered.

The Symfony templating engine is explained in great detail in the
:doc:`Templating </book/templating>` chapter.

.. tip::

    You can even avoid calling the ``render`` method by using the ``@Template``
    annotation. See the :doc:`FrameworkExtraBundle documentation</bundles/SensioFrameworkExtraBundle/annotations/view>`
    more details.

.. tip::

    The ``renderView`` method is a shortcut to direct use of the ``templating``
    service. The ``templating`` service can also be used directly::

        $templating = $this->get('templating');
        $content = $templating->render(
            'AcmeHelloBundle:Hello:index.html.twig',
            array('name' => $name)
        );

.. note::

    It is possible to render templates in deeper subdirectories as well, however
    be careful to avoid the pitfall of making your directory structure unduly
    elaborate::

        $templating->render(
            'AcmeHelloBundle:Hello/Greetings:index.html.twig',
            array('name' => $name)
        );
        // index.html.twig found in Resources/views/Hello/Greetings is rendered.

.. index::
   single: Controller; Accessing services

Accessing other Services
~~~~~~~~~~~~~~~~~~~~~~~~

When extending the base controller class, you can access any Symfony2 service
via the ``get()`` method. Here are several common services you might need::

    $request = $this->getRequest();

    $templating = $this->get('templating');

    $router = $this->get('router');

    $mailer = $this->get('mailer');

There are countless other services available and you are encouraged to define
your own. To list all available services, use the ``container:debug`` console
command:

.. code-block:: bash

    $ php app/console container:debug

For more information, see the :doc:`/book/service_container` chapter.

.. index::
   single: Controller; Managing errors
   single: Controller; 404 pages

Managing Errors and 404 Pages
-----------------------------

When things are not found, you should play well with the HTTP protocol and
return a 404 response. To do this, you'll throw a special type of exception.
If you're extending the base controller class, do the following::

    public function indexAction()
    {
        // retrieve the object from database
        $product = ...;
        if (!$product) {
            throw $this->createNotFoundException('The product does not exist');
        }

        return $this->render(...);
    }

The ``createNotFoundException()`` method creates a special ``NotFoundHttpException``
object, which ultimately triggers a 404 HTTP response inside Symfony.

Of course, you're free to throw any ``Exception`` class in your controller -
Symfony2 will automatically return a 500 HTTP response code.

.. code-block:: php

    throw new \Exception('Something went wrong!');

In every case, a styled error page is shown to the end user and a full debug
error page is shown to the developer (when viewing the page in debug mode).
Both of these error pages can be customized. For details, read the
":doc:`/cookbook/controller/error_pages`" cookbook recipe.

.. index::
   single: Controller; The session
   single: Session

Managing the Session
--------------------

Symfony2 provides a nice session object that you can use to store information
about the user (be it a real person using a browser, a bot, or a web service)
between requests. By default, Symfony2 stores the attributes in a cookie
by using the native PHP sessions.

Storing and retrieving information from the session can be easily achieved
from any controller::

    $session = $this->getRequest()->getSession();

    // store an attribute for reuse during a later user request
    $session->set('foo', 'bar');

    // in another controller for another request
    $foo = $session->get('foo');

    // use a default value if the key doesn't exist
    $filters = $session->get('filters', array());

These attributes will remain on the user for the remainder of that user's
session.

.. index::
   single: Session; Flash messages

Flash Messages
~~~~~~~~~~~~~~

You can also store small messages that will be stored on the user's session
for exactly one additional request. This is useful when processing a form:
you want to redirect and have a special message shown on the *next* request.
These types of messages are called "flash" messages.

For example, imagine you're processing a form submit::

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

After processing the request, the controller sets a ``notice`` flash message
and then redirects. The name (``notice``) isn't significant - it's just what
you're using to identify the type of the message.

In the template of the next action, the following code could be used to render
the ``notice`` message:

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

By design, flash messages are meant to live for exactly one request (they're
"gone in a flash"). They're designed to be used across redirects exactly as
you've done in this example.

.. index::
   single: Controller; Response object

The Response Object
-------------------

The only requirement for a controller is to return a ``Response`` object. The
:class:`Symfony\\Component\\HttpFoundation\\Response` class is a PHP
abstraction around the HTTP response - the text-based message filled with HTTP
headers and content that's sent back to the client::

    use Symfony\Component\HttpFoundation\Response;

    // create a simple Response with a 200 status code (the default)
    $response = new Response('Hello '.$name, 200);

    // create a JSON-response with a 200 status code
    $response = new Response(json_encode(array('name' => $name)));
    $response->headers->set('Content-Type', 'application/json');

.. tip::

    The ``headers`` property is a
    :class:`Symfony\\Component\\HttpFoundation\\HeaderBag` object with several
    useful methods for reading and mutating the ``Response`` headers. The
    header names are normalized so that using ``Content-Type`` is equivalent
    to ``content-type`` or even ``content_type``.

.. tip::

    There are also special classes to make certain kinds of responses easier:

    - For JSON, there is :class:`Symfony\\Component\\HttpFoundation\\JsonResponse`.
      See :ref:`component-http-foundation-json-response`.
    - For files, there is :class:`Symfony\\Component\\HttpFoundation\\BinaryFileResponse`.
      See :ref:`component-http-foundation-serving-files`.

.. index::
   single: Controller; Request object

The Request Object
------------------

Besides the values of the routing placeholders, the controller also has access
to the ``Request`` object when extending the base ``Controller`` class::

    $request = $this->getRequest();

    $request->isXmlHttpRequest(); // is it an Ajax request?

    $request->getPreferredLanguage(array('en', 'fr'));

    $request->query->get('page'); // get a $_GET parameter

    $request->request->get('page'); // get a $_POST parameter

Like the ``Response`` object, the request headers are stored in a ``HeaderBag``
object and are easily accessible.

Final Thoughts
--------------

Whenever you create a page, you'll ultimately need to write some code that
contains the logic for that page. In Symfony, this is called a controller,
and it's a PHP function that can do anything it needs in order to return
the final ``Response`` object that will be returned to the user.

To make life easier, you can choose to extend a base ``Controller`` class,
which contains shortcut methods for many common controller tasks. For example,
since you don't want to put HTML code in your controller, you can use
the ``render()`` method to render and return the content from a template.

In other chapters, you'll see how the controller can be used to persist and
fetch objects from a database, process form submissions, handle caching and
more.

Learn more from the Cookbook
----------------------------

* :doc:`/cookbook/controller/error_pages`
* :doc:`/cookbook/controller/service`
