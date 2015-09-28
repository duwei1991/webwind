# Using Interceptor #

Interceptors are like Servlet Filters somehow, but they are managed by IoC
container. WebWind will get all interceptors from IoC container automatically.
Interceptor can be used to do transaction, logging, and other things.

It is easy to define an interceptor:

```
import org.expressme.webwind.*;

@InterceptorOrder(0)
public class PerformanceInterceptor implements Interceptor {
    public void intercept(Execution execution, InterceptorChain chain) throws Exception {
        long start = System.currentTimeMillis();
        try {
            chain.doInterceptor(execution);
        }
        finally {
            long last = System.currentTimeMillis() - start;
            System.out.println("Execute " + last + " ms for URL: " + execution.request.getRequestURI());
        }
    }
}
```

But don't forget to call `chain.doInterceptor(execution)`, otherwise the process
of HTTP request cannot be continued to next interceptor and the method which
handles URL.

More than one interceptors are linked as a chain. `@InterceptorOrder` is used to
specify the orders of interceptors. The lower value has higher priority.