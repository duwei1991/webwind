# Hello, world #

The WebWind version of 'Hello, world' sample is very simple. See:

```
import org.expressme.webwind.Mapping;

public class MyPage {

    @Mapping("/hello")
    public String hello() {
        return "<h1>Hello, world</h1>";
    }
}
```

Note that WebWind does NOT require any framework-provided interface known
as Action or Controller. URL mapping is defined on method by Java 5 annotation.

Now make sure this JavaBean is managed by your IoC container. Start web server
and see web page in browser:

![http://webwind.googlecode.com/svn/wiki/Hello.png](http://webwind.googlecode.com/svn/wiki/Hello.png)

# Passing parameters #

Let's do little extension to make it say 'Hello' to everyone. You can do it by
adding a parameter like `/hello?name=michael`, and handle the parameter by yourself
in old-style MVC frameworks. But is it simple and friendly if URL is `/hello/michael`:

```
@Mapping("/hello/$1")
public String hello(String name) {
    return "<h1>Hello, " + name + "</h1>";
}
```

The web page in browser is shown as follow:

![http://webwind.googlecode.com/svn/wiki/HelloMichael.png](http://webwind.googlecode.com/svn/wiki/HelloMichael.png)

You may guess that the `$1` is the parameter in URL and passed automatically to
the first argument `name` in method `hello(String)`. That's right.

The two methods can be placed together, both works well:

```
import org.expressme.webwind.Mapping;

public class MyPage {

    @Mapping("/hello")
    public String hello() {
        return "<h1>Hello, world</h1>";
    }

    @Mapping("/hello/$1")
    public String hello(String name) {
        return "<h1>Hello, " + name + "</h1>";
    }
}
```

Let's get into the details of [URL mapping](DeveloperMapping.md).