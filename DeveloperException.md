# Handle Exception #

The ExceptionHandler can handle all exceptions during the process of HTTP request.
You may define one ExceptionHandler in your IoC container:

```
public class MyExceptionHandler implements ExceptionHandler {
    public void handle(HttpServletRequest request, HttpServletResponse response, Exception e) throws Exception {
        // TODO: handle exception or re-throw it...
    }
}
```

WebWind will automatically detect your exception handler. If no exception
handler is defined, the `DefaultExceptionHandler` will be used. It just prints
the exception stack trace in web page for debug purpose. If more than one
exception handlers are defined, WebWind will use the first one and ignore
others. Note that it cannot assume the orders of JavaBeans in IoC container.

It is recommend to define your exception handler. It is useful to redirect the
anonymous user to sign on page.