# Handle URL #

As you see, WebWind does not require any particular interface, and allows
multiple methods put together in one class. Each method can handle one URL
defined by `@Mapping` annotation.

The method you defined to handle URL must have following requirements:

  * It must be `public`, it must not be `static`.
  * It must return `void`, `String` or `org.expressme.wind.Renderer`.
  * Its arguments must be supported by WebWind. See [Supported types of arguments](DeveloperMapping#Supported_Types_of_Arguments.md).
  * The number of arguments must be equals to the number of parameters in URL.

Method can throw any type of exception.

# Definition of URL #

WebWind uses regular expression to handle URL internally. However, regular
expression is not easy to write correctly. So the definition of URL in
WebWind uses human-readable format like `/user/$1/$2.html`.

When you define the URL with parameters, WebWind can pass these parameters
to the method as arguments. The placeholder of parameters in URL is `$`, and
its index starts from 1, maximum to 9, which means you can pass at most 9 parameters.

Each `$n` (n=1 to 9) will match the particular group of URL. It may match the
empty string, but it does **NOT** match any string contains `/`, which means:

`/user/$1.html` will match `/user/123456.html`, but not match `/user/123/456.html`.

Such definitions of URL are invalid:

  * /user/$2.html: Because no $1 found.
  * /blog/$1/$3.html: Because no $2 found.

Such definitions of URL are valid but not recommended:

  * /user/$1$2.html: Because $1 will always the empty string.
  * $1.html: It cannot match any URL because URL always starts with `/`.

Parameters in URL will automatically converted to the actual types of arguments
in method. For example:

```
@Mapping("/user/$1/$2.html")
public void handle(long userId, int postId) throws Exception {
    // $1 is converted to long, $2 is converted to int...
}
```

# Supported Types of Arguments #

Not all the Java types of arguments of method can be supported, because
WebWind must convert the string to the particular Java type of argument.
The build-in supported types are `java.lang.String` and all primitive types
as well as its wrapper types (int, Integer, boolean, Boolean, etc.).

| **Type** | **Convertion** | **Exception** |
|:---------|:---------------|:--------------|
| java.lang.String | The same       | none          |
| boolean, Boolean | Boolean.valueOf(String) | none          |
| char, Character | String.charAt(0) | IllegalArgumentException |
| byte, Byte | Byte.parse(String) | NumberFormatException |
| short, Short | Short.parse(String) | NumberFormatException |
| int, Integer | Integer.parse(String) | NumberFormatException |
| long, Long | Long.parse(String) | NumberFormatException |
| float, Float | Float.parse(String) | NumberFormatException |
| double, Double | Double.parse(String) | NumberFormatException |

TODO: How to add more types