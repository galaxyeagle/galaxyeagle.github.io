---
layout : page
title : Using MathJax in your website
date : 2023-04-27
---

I discovered the need to write mathematical expressions in my blog posts and pages. I initially looked up the good old LaTex framework. However I quickly found out that better Tex support for webpages is provided by a Javascript library called MathJax which was accessible through a CDN (Content Delivery Network) website called cdn.mathjax.org. This allowed our website to use the library from the cloud without downloading it on our web server ! In 2017, the website cdn.mathjax.org was shut down. However copy of the MathJax library is available from many CDNs which host it. One such is jsdelivr.com.  

The following script inserted in the <head> tag of our webpage enables us to use this library. 


```
	  <script type="text/javascript" id="MathJax-script" async
	    src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
	  </script>
```


Since my master layout is <> default.html located in the _layouts folder of my web directory, and all other layouts use this template, so I inserted this script in the <head> tag of my default.html layout file. And bingo ! All my webpages are now rendering MathJax syntax such as \\(x=y+z\\) or \\[\sqrt x =y-z\\]  

- 	For centered formulae, use ``` \\[ and \\]  ```
- 	For inline formulae, use ``` \\( and \\)  ```

__References :__     
	https://www.mathjax.org/ -> Go to Documentation -> Go to Getting started
	https://hiltmon.com/blog/2017/01/28/mathjax-in-markdown/