+++
date = "2013-12-16T20:07:05-07:00"
draft = false
title = "Web API: Extending Web Api Help"
url = "/posts/2013/12/16/web-api--extending-web-api-help"
tags=["web api", "help", "nuget"]
+++

## Web API: Extending Web Api Help

One of the best features of web API is its access to tools to create useful documentation for RPC over HTTP protocols.

### Web Api Help In a Nutshell

Web api help provides us the ability out of the box to build a useful help system for our [[abbr title="hyper text transfer protocol"]]HTTP[[/abbr]] [[abbr title="Remote Procedure Call"]]RPC[[/abbr]] [[abbr title="Application Programming Interface"]]API's[[/abbr]]. The way it works is that it actually uses reflection which is cached at the controller context to create these things called `ActionDescriptors`. These `ActionDescriptors` will actually provide a context to the web [[abbr title="Application Programming Interface"]]API[[/abbr]] on what is the Route To Actually Hit your [[abbr title="Application Programming Interface"]]API[[/abbr]] end point.


Knowing this, we can customize the input and output parameters via [[abbr title="extensible markup language"]]XML[[/abbr]] documentation comments like the ones below.


[[pre]]
/// &lt;summary&gt;
///  This class performs an important function.
/// &lt;/summary&gt;
[[/pre]]


We can also customize sample [[abbr title="javascript object notation"]]JSON[[/abbr]] objects or other Media Type outputs. In fact if you do not specify any sample objects, sample objects are generated automatically for you. While this can be cool it can lead to some unintended behaviors so be careful and when distributing your API you should have some kind of good object that can be demonstrated to users. As in something you may actually see.


> **PRO TIP** Consider overriding default objects with actual things your users might see in production.


Web [[abbr title="Application Programming Interface"]]API[[/abbr]] Help package also has other added benefits. For example you can customize comments on parameters of a `ApiController`.

> For in depth information on web api help pages visit this [site](http://www.asp.net/web-api/overview/creating-web-apis/creating-api-help-pages).

### Adding Web Api Help Pages To Your Projects

By default they are usually installed when you create a new Web API project inside visual studio. If you need to add the package manually because you are creating a very base project then you will need to use the following at the Package Manager Console.

[[pre]]
Install-Package Microsoft.AspNet.WebApi.HelpPage -Version 5.0.0[[/pre]]

### Pages are though kind of meh.

Unfortunately, with all good intentions of the Web [[abbr title="application programming interface"]]Api[[/abbr]] Help Page project. It kind of leaves with little room to customizing the pages or providing more scope or emphasis. For example I can not highlight specific things about the api, or even provide some sample code for the consumer to use or even a sample HTTP request.

**Now**, I know if you have to have a document to describe your [[abbr title="application programming interface"]]API[[/abbr]] [its not considered RESTfull.](http://en.wikipedia.org/wiki/HATEOAS) But Sometimes its [[abbr title="okay"]]OK[[/abbr]] to sacrifice a purists definition in order to get something done. And sometimes we are really just stuck with making something more [[abbr title="Remote Procedure Call"]]RPC[[/abbr]] like or maybe we started with that.  

> I am going to leave the discussion on purists vs pragmatist alone, because that can usually start some zealous dogmatic battles for *both sides*

### Making It a little easier on the eyes

Well one of the great things about this is that with a  little imagination we can change this and add some useful formatting and structure to our documents.

### We could use [[abbr title="Hyper Text Markup Language"]]HTML[[/abbr]]

We could just use ### We could use [[abbr title="Hyper Text Markup Language"]]HTML[[/abbr]]. I mean it has everything we really need and can format some documents beautifully.

[[pre]]
/// &lt;summary&gt;
///  This class &lt;b&gt;performs&lt;/b&gt; an important function.
/// &lt;/summary&gt;
[[/pre]]

And this is all well and good however we are going to get errors in our [[abbr title="extensible markup language"]]XML[[/abbr]] to where it won't render right.

### What to do?

People who have used apps like [GitHub](http://www.github.com) or online apps like [Assembla](http://www.assembla.com). Know that you can use [MarkDown](http://en.wikipedia.org/wiki/Markdown) to express themselves with an easy to read document structure that does not get in the way by adding superficial document structure.

```
  /// &lt;summary&gt;
  ///  This class **performs** an important function.
  /// &lt;/summary&gt;
```
Now obviously, this won't work out of the box. But with some simple changes to the project we can get this to work.

First things first download the following package.

```
  Install-Package MarkdownDeep.NET -Version 1.5
```

Next, add the code below to HelpPage area in the project.

```
  namespace System.Web.Mvc.Html
  {
  using System;
  using System.Collections.Generic;
  using System.Linq;
  using System.Web;
  using System.Web.Mvc;

  public static class MarkdownHelper
  {
      public static MvcHtmlString FromMarkdown(this HtmlHelper helper, string text)
      {
          var instance = new MarkdownDeep.Markdown();

          instance.SafeMode = false;
          instance.ExtraMode = true;
          text = MakeCompatiableString(text);
          text = text.Replace("[[", "<").Replace("]]", ">");

          return MvcHtmlString.Create(instance.Transform(text.Trim()));
      }

      private static string MakeCompatiableString(string text)
      {
          if (string.IsNullOrEmpty(text)){
              return string.Empty;
          }

          var lines = text.Split('\n');
          return string.Concat(lines.Select(x => x.TrimStart() + "\n").ToList());
      }

  }
  }
```

Finally, make some adjustments to the following file. Areas/HelpPage/Views/Help/DisplayTemplates/HelpPageApiModel

Under the **&lt;H2&gt;Summary&lt;/H2&gt;** modify the code.

<pre>
&gt;h2&lt;Summary&gt;/h2&lt;

    @if (description.Documentation != null)
    {
        &gt;p&lt;@Html.FromMarkdown(description.Documentation)&gt;/p&lt;
    }
    else
    {
        &gt;p&lt;No documentation available.&gt;/p&lt;
    }
</pre>

### Additional Resources
1. [Creating Help Pages For ASP.net Web API](http://www.asp.net/web-api/overview/creating-web-apis/creating-api-help-pages)
1. [Design Time Generation of Help Page](http://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)

#### References
1. http://blogs.msdn.com/b/yaohuang1/archive/tags/documentation/
