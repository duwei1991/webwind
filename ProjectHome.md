WebWind is a REST-style MVC framework that totally different from traditional MVC frameworks (such as Struts, WebWork).

WebWind is simple and small (86KB including source). See sample class:

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

Read article on [IBM developerWorks China](http://www.ibm.com/developerworks/cn/java/j-lo-restmvc/).

[Learn more](WebWind.md) and [download now](http://code.google.com/p/webwind/downloads/list)!