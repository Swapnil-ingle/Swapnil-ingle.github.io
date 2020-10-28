## JSP Learning Notes

JSP:

JSP stands for Java Servlet Pages. JSP can be used to create application in Java which are web-based.

Make sure to add `javax-servlet` library in your project to enable Servlet.

Let's start with what happens when a request hit the browser window.

We can decide what Servlet/JSP to call after a request hit the browser, we can do this using web.xml (Servlet mapping) or using WebServlet annotation or can be left to JSP to implicitly handle the request and redirect to appropriately named JSP page.

### Defining a servlet:

Think of servlet as a Java class which has the logic of what to do when a specific URL is hit.

We can define a Servlet as as Java class which extends HttpServlet.

```
public class AddServlet extends HttpServlet { ... }
```

In this class we can override the doGet(), doPost(), etc, methods which will handle specific type of requests.

Once the servlet is defined we need to decide what URL will hit this servlet.

We can do this in multiple ways.

### Using web.xml file:

We can use <servlet></servlet> to define our servlet and <servlet-mapping> to decide when it should be called.

```
<servlet>
    <servlet-name>add</servlet-name>
    <servlet-class>com.github.swapnil.AddServlet</servlet-class>
</servlet>

<servlet-mapping>
    <servlet-name>add</servlet-name>
    <url-pattern>/add</url-pattern>
</servlet-mapping>
```

This will call the above servlet when '/add' URL is hit. (Remember, we can pass any data from the HTML page - from user - and retrieve it in the Servlet)

### Using @WebServlet annotation:

XMLs are hard to work with as I find them very verbose. We can avoid this by using annotations instead.

```
@WebServlet("/add")
public class AddServlet extends HttpServlet {
```

This will tell Java to use AddServlet for /add URL.

Or we can use a JSP page called add.jsp and Java will automagically redirect the /add URL to add.jsp page.

### Using MVC with JSP:

Normally you should not define any business logic in the JSP page as it should be only used to render the view layer.

The web-request will go to the Servlet and process the data and then go the view and display the processed data.

The JSP page will be converted to a Servlet by Java.

### Anatomy of a JSP page:

A JSP page has four sections:

* Directive --> When we want to import some package or use to 3rd party tag libraries we can declare it here. (<%@ %>)
* Declaration --> If we want some code to be available outside of Java. (<%! %>)
* Scriptlet --> If you want to write some Java code in JSP (Which will be in the Servlet) we can use this section. (<% %>)
* Expression --> When you want to write something on the HTML page you can use this. (%= %>)

### Built-in Objects:

Some objects are implicitly defined by JSP and you can use it straight away without the need of defining it first.

These are the implicit objects.
* request → Scope is the current page and the next page; All the page in this current request.
* Response
* pageContext → Using this we can set/get the value of attributes; the scope is defined for that page only, or we can explicitly set the scope.
* out
* session
* application
* config

### JSTL

JSTL is JSP Standard Tag Library,

The idea is, instead of writing in pure Java code in JSP pages, it is better for the front-end developers to write in tag-based code (as the front-end developers would not be familiar with the Java language)

For example, instead of doing <% out.println(request.getAttribute("name")) %> we can do <c: out value = ${name}>

To import the JSTL library in a JSP page use, '<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>'

### Filters

Whenever a client sends a web-request say we want to log it, we can do this in two ways:
Either define the logging code in the Servlet that handles the request
OR create a filter that intercepts a URL (regex pattern) and executes some business logic before forwarding to the Servlet.
Doing the second one is beneficial, as we can then use the same filter and plug it in different URL pattern and voila we will have logging for a new request as well.

No need to edit the code inside Servlet.

### FilterChain

When we have multiple filters we need to decide the order of execution of the filters. This is done in the web.xml file.

There are three methods for the filter:
* Init
* doFilter
* destroy

### Using SpringBoot Controller instead of WebServlet.

https://www.youtube.com/watch?v=uBjeeUfnhUM&ab_channel=JavaGuides
