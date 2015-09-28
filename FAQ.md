# FAQ #

Here are some common issues you may encountered.

## How to Access Servlet Objects ##

All Servlet objects (ServletContext, HttpSession, HttpServletRequest,
HttpServletResponse) are always available because they are arealy bind to
ThreadLocal and can be retrieved using following code:

```
ActionContext ctx = ActionContext.getActionContext();
ServletContext servletContext = ctx.getServletContext();
HttpSession httpSession = ctx.getHttpSession();
HttpServletRequest httpServletRequest = ctx.getHttpServletRequest();
HttpServletResponse httpServletResponse = ctx.getHttpServletResponse();
```

## How to Set Character Encoding ##

WebWind always uses `UTF-8` as default character encoding if possible,
which supports international applications.

If you want to specify another character encoding, use TextRenderer:

```
@Mapping("/hello")
public Renderer hello() {
    return new TextRenderer("<h1>Hello, world</h1>", "iso-8859-1");
}
```

If you use JSP, the character encoding is set in your JSP file:

```
<%@page contentType="text/html;charset=UTF-8" %>
```

Please note that the default character encoding of JSP is `ISO-8859-1`
according to the JSP standards.

If you use Velocity, the character encoding is specified in
`/WEB-INF/velocity.properties`:

```
# the velocity template file encoding:
input.encoding=UTF-8

# the output encoding:
output.encoding=UTF-8
```

If the encoding is not specified, WebWind will automatically use `UTF-8`
for both `input.encoding` and `output.encoding`.

## How to Generate Dynamic Text Content ##

The TextRenderer can be used to output any text:

```
@Mapping("/hello/$1")
public Renderer alert(String name) {
    TextRenderer renderer = new TextRenderer("hello, " + name + "!");
    renderer.setContentType("text/plain");
    return renderer;
}
```

If you want to send text as JavaScript, use JavaScriptRender which set the
content type as 'application/x-javascript' automatically.

## How to Generate Dynamic Binary Content ##

If you want to output dynamic binary file such as PDF, JPG, etc., you can use
`BinaryRender`:

```
@Mapping("/download/pdf")
public Renderer download() {
    byte[] data = generateBinaryContent(); // build your binary content here
    BinaryRenderer renderer = new BinaryRenderer(data);
    renderer.setContentType("application/pdf");
    return renderer;
}
```

If you do not specify the content type, WebWind will use the default MIME
type `application/octet-stream` for binary file.

## How to Send a Redirect ##

You do not need to operate HttpServletResponse object. Just return a String
starts with 'redirect:':

```
@Mapping("/redirect")
public String redirect() {
    return "redirect:/index.jsp";
}
```

## How to Inject a ServletContext Object ##

If you use Guice as IoC container, you may want to inject the ServletContext
for some reason:

```
public class MyClass {
    @Inject ServletContext servlectContext;
}
```

You can get the ServletContext instance in your module. If the module class
implements the `org.expressme.wind.guice.ServletContextAware` interface,
WebWind will call `setServletContext(ServletContext)` to pass the
ServletContext instance:

```
public class MyModule implements Module, org.expressme.wind.guice.ServletContextAware {

    private ServletContext servletContext;

    public void setServletContext(ServletContext servletContext) {
        this.servletContext = servletContext;
    }

    public void configure(Binder binder) {
        // bind the ServletContext:
        binder.bind(ServletContext.class).toInstance(this.servletContext);
        // bind others...
    }
}
```

If you use Spring, just implements [ServletContextAware](http://static.springsource.org/spring/docs/2.5.x/api/org/springframework/web/context/ServletContextAware.html)
provided by Spring API:

```
public class MyClass implements org.springframework.web.context.ServletContextAware {

    private ServletContext servletContext;

    public void setServletContext(ServletContext servletContext) {
        this.servletContext = servletContext;
    }
}
```

## Keep Updating ##

Please leave a comment if you have more questions.