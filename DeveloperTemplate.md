# Using Template #

As a demonstration, you can return a String that contains HTML like
`<html><body>Hello, world!</body></html>`. But in real world, the complex web
pages are rendered by kinds of template engines. WebWind supports two
build-in template engines: JSP and [Velocity](http://velocity.apache.org/).

Rendering web pages by template in WebWind is simple:

  1. Put all data into a `Map<String, Object>` as model;
  1. Create a TemplateRenderer and return.

See example bellow:

```
public class IndexPage {

    @Mapping("/")
    public Renderer index() {
        Map<String, Object> model = new HashMap<String, Object>();
        model.put("name", "Michael");
        model.put("date", new Date());
        return new TemplateRenderer("/index.jsp", model);
    }
}
```

If you configure nothing about template, the default template engine `JSP` is
used. For example, the `index.jsp` for code above should be like:

```
<html>
<body>
  Hello, <%=request.getAttribute("name") %>!
  It is <%=request.getAttribute("date") %>!
</body>
</html>
```

To use Velocity template, prepare a Velocity template file `index.htm` like:

```
<html>
<body>
  Hello, ${name}! It is ${date.toString()}!
</body>
</html>
```

See [Configurations](DeveloperConfig.md) to get more about configuration of Velocity.

# Need More Template Engine Support #

If you want to use any other template engine rather than Jsp or Velocity, you
need additional work to do.

WebWind allows you to add any template engine (such as [FreeMarker](http://freemarker.org/), etc)
by writing your own `TemplateFactory` class. For example:

```
package sample;

import org.expressme.wind.template.ContainerFactory;

public class MyTemplateFactory implements TemplateFactory {

    public Template loadTemplate(String path) throws Exception {
        return new MyTemplate(path);
    }

    public void init(Config config) {
        // TODO: init your template engine here...
    }
}
```

You need implement two methods above. It is easy for experienced developers. See
[JspTemplateFactory.java](http://code.google.com/p/express-me/source/browse/trunk/ExpressWind/src/org/expressme/wind/template/JspTemplateFactory.java)
and [VelocityTemplateFactory.java](http://code.google.com/p/express-me/source/browse/trunk/ExpressWind/src/org/expressme/wind/template/VelocityTemplateFactory.java)
as examples.

And also `MyTemplate` class must be inherited from `Template`:

```
package sample;

import java.io.PrintWriter;
import java.util.Map;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.expressme.wind.template.Template;

public class MyTemplate implements Template {

    private String path;

    public MyTemplate(String path) {
        this.path = path;
    }

    public void render(HttpServletRequest request, HttpServletResponse response, Map<String, Object> model) throws Exception {
        String content = build_content_by_template(model);
        PrintWriter pw = response.getWriter();
        pw.write(content);
        pw.flush();
    }
}
```

Specify your own template engine in `web.xml` with **FULL CLASS NAME**:

```
<init-param>
    <param-name>template</param-name>
    <param-value>sample.MyTemplateFactory</param-value>
</init-param>
```