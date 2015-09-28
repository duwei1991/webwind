# Requirement #

WebWind framework requires:

  * Java 5 or above.
  * Servlet 2.4 compatible Java web server (or above).
  * An IoC container such as [Spring](http://www.springsource.org/) or [Guice](http://code.google.com/p/google-guice/).

WebWind does **NOT** manage any web components but requests all from IoC container.

# Configurations #

Configurations of WebWind are simple. Do following steps:

## Declare DispatcherServlet in web.xml ##

First please add a `Servlet` declaration in your `web.xml` file:

```
<servlet>
    <servlet-name>dispatcher</servlet-name>
    <servlet-class>org.expressme.webwind.DispatcherServlet</servlet-class>
    <init-param>
        <param-name>container</param-name>
        <param-value>Guice</param-value>
    </init-param>
    <init-param>
        <param-name>modules</param-name>
        <param-value>org.expressme.sample.BlogModule</param-value>
    </init-param>
    <init-param>
        <param-name>template</param-name>
        <param-value>Velocity</param-value>
    </init-param>
    <load-on-startup>0</load-on-startup>
</servlet>

<servlet-mapping>
    <servlet-name>dispatcher</servlet-name>
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

Note that all parameters' names and values are **CASE-SENSITIVE**.

The parameter `container` is used to specify your IoC container. WebWind
supports two build-in IoC containers `Spring` and `Guice`.

If you want to use other IoC containers, you can specify the full class name.
See [how to integrate other IoC containers](DeveloperExtension#Integration_of_3rd-part_IoC_Container.md).

The parameter `template` is used to specify how to render the web page. WebWind
supports two build-in templates `Jsp` and `Velocity`.

The `Jsp` is the default template. If you specify [Velocity](http://velocity.apache.org/)
as template engine, another `velocity.properties` file is needed and must be put in `/WEB-INF/`.

If you want to use other template engine such as [FreeMarker](http://freemarker.sourceforge.net/),
see [how to integrate other template engines](DeveloperExtension#Integration_of_3rd-part_Template_Engine.md).

`<load-on-startup>` is the standard option in Servlet configurations. Setting to
`0` to make web server initialize the Servlet on startup immediately.

It is very important that the `url-pattern` **MUST** be set to `/` in `servlet-mapping`
section. [Struts](http://struts.apache.org/) (or other similiar MVC framework)
uses `*.do` as URL mapping, but WebWind gives you completely freedom to
organize your URL structure. Such URLs are supported:

  * /blog/display/20090909
  * /post/2009/09/09/12345.html
  * /user/1002/35af98c260\_1.js

So the URL `/` MUST be mapped to the DispatcherServlet provided by WebWind.

If you choose Guice as your IoC container, the `module` parameter is needed,
and its value specify the full class name of module required by Guice.

If you choose Spring as your IoC container, you need add following section into
`web.xml` to start Spring before WebWind loaded:

```
<listener>
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
```

The default configuration file of Spring is `applicationContext.xml` which should
be put in `/WEB-INF/`.

That's all configurations! No extra configuration files!

Continue with first ['Hello, world'](DeveloperHello.md) application.

## Alternative Declaration ##

If you already have a default `Servlet` which is mapped to `/`, WebWind
provides another choose of `DispatcherFilter`. The declaration is just like
the `DispatcherServlet`:

```
<filter>
    <filter-name>dispatcher</servlet-name>
    <filter-class>org.expressme.webwind.DispatcherFilter</servlet-class>
    <init-param>
        <param-name>container</param-name>
        <param-value>Guice</param-value>
    </init-param>
    <init-param>
        <param-name>modules</param-name>
        <param-value>org.expressme.sample.BlogModule</param-value>
    </init-param>
    <init-param>
        <param-name>template</param-name>
        <param-value>Velocity</param-value>
    </init-param>
</filter>

<filter-mapping>
    <filter-name>dispatcher</servlet-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

The only differences are:

  * No configuration of `<load-on-startup>`.
  * The value of `<url-pattern>` are `/*`, not `/`.