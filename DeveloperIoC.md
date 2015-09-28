# Choose an IoC Container #

As we discussed, WebWind does **NOT** manage the web components but requests
all from IoC container. WebWind supports two build-in IoC container:
[Guice](http://code.google.com/p/google-guice/) and [Spring](http://www.springsource.org/).

All methods with `@Mapping` defined in JavaBeans which are managed by IoC container will
be scanned and hold by WebWind. For example, define such class which contains
two methods with URL mapping:

```
public class Blog {

    @Mapping("/hello/$1")
    public String hello(String name) {
        return "<h1>Hello, " + name + "</h1>";
    }

    @Mapping("/")
    public Renderer index() throws Exception {
        return new TemplateRenderer("/index.htm");
    }
}
```

The scope of JavaBeans in IoC container can be either `singleton` or `prototype`,
but make sure they are stateless, because WebWind will hold the references
of these beans and treat them as singleton.

# Using Guice #

If you want to use Guice as IoC container, you need specify the module class in
`web.xml`:

```
<init-param>
    <param-name>modules</param-name>
    <param-value>org.expressme.sample.BlogModule</param-value>
</init-param>
```

See [Configurations](DeveloperConfig.md) to get more about configuration of Guice.

And write your module class:

```
public class BlogModule implements Module {
    public void configure(Binder binder) {
        binder.bind(Blog.class).asEagerSingleton();
    }
}
```

# Using Spring #

If you want to use Spring as IoC container, you need declare the beans in Spring
configuration file (usually named as `applicationContext.xml`):

```
<bean class="org.expressme.sample.Blog" />
```

See [Configurations](DeveloperConfig.md) to get more about configuration of Spring.

# Need More IoC Container Support #

If you want to use any other IoC container rather than Guice or Spring, you need
additional work to do.

WebWind allows you to add any IoC containers (such as [HiveMind](http://hivemind.apache.org/),
[PicoContainer](http://www.picocontainer.org/), etc) by writing your own
`ContainerFactory` class. For example:

```
package sample;

import java.util.List;
import org.expressme.wind.container.ContainerFactory;

public class MyContainerFactory implements ContainerFactory {
    public List<Object> findAllBeans() throws ServletException {
        // TODO: return all objects managed by IoC container
    }

    public void init(Config config) throws ServletException {
        // TODO: init or find IoC container instance
    }
}
```

You need implement two methods above. It is easy for experienced developers. See
[SpringContainerFactory.java](http://code.google.com/p/express-me/source/browse/trunk/ExpressWind/src/org/expressme/wind/container/SpringContainerFactory.java)
and [GuiceContainerFactory.java](http://code.google.com/p/express-me/source/browse/trunk/ExpressWind/src/org/expressme/wind/container/GuiceContainerFactory.java)
as examples.

Specify your own container factory in `web.xml` with **FULL CLASS NAME**:

```
<init-param>
    <param-name>container</param-name>
    <param-value>sample.MyContainerFactory</param-value>
</init-param>
```