# Introduction #

WebWind is an MVC framework for Java web applications. Unlike most MVC
frameworks such as Struts, WebWind does support friendly URL directly.
Web applications can be more friendly to search engine, and more simple.

# Features #

WebWind provides many effective and simple way to make development of
web application become easier.

  * Friendly URL support like '/blog/display/20090909'.
  * Handling HTTP request in your defined method.
  * Support Interceptor to handle transaction, logging and something else.
  * Support ExceptionHandler to handle exception in a central control.
  * Support file upload.

Unlike [Struts](http://struts.apache.org/) or other MVC frameworks, WebWind
does not require any particular interface known as `Action` or `Controller`.
You can define your own methods to handle URL you defined:

```
public class Blog {

    @Mapping("/hello/$1")
    public String hello(String name) {
        return "<h1>Hello, " + name + "</h1>";
    }

    @Mapping("/")
    public Renderer index() throws Exception {
        return new TemplateRenderer("/index.jsp");
    }
}
```

See [Developer's Guide](Developer.md) to get more details about WebWind.