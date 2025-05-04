---
layout : page
title : Creating a Website- Advanced Features (I)
date : 2021-04-21
---




As discussed in the last post, there are 4 folders in your repo :
/_includes, /_layouts/, /_sass , /assets  
(Ref. https://github.com/jekyll/minima  for their component files).


***What I did to hack out the inessentials***

Firstly, I went to index.html in my LocalRepo & changed layout from home.html to default.html in the frontmatter. Now I deleted home.html from /_includes folder as it not needed anywhere now.

Then, I wanted to make a Responsive web page (ref. https://www.w3schools.com/css/css_rwd_intro.asp) for my site.

I noticed that 2 folders : /sass & /asssets do all the designing work via scss files.

The default.html file includes head.html.    
head.html links the stylesheet /assets/main.scss    
/assets/main.scss imports /_sass/minima.scss    
This /_sass/minima.scss imports /_sass/minima/layout.scss, /_sass/minima/base.scss & /_sass/minima/syntax.highlighter.scss  
These 3 files contain all the real css code & seemed quite complex.  
So I deleted all files & folders in the /_sass folder & made a new single scss file with a code similar to https://www.w3schools.com/css/tryit.asp?filename=tryresponsive_col-s  
& imported this scss file in the /assets/main.scss instead of the /_sass/minima.scss earlier. I knew the scss files I deleted had designs for many classes in the /_layouts & /_includes folders. So I went to all files in these folders & removed any classes in any <div>, <main>, <span>, etc tags & added the classes as provided for in https://www.w3schools.com/css/tryit.asp?filename=tryresponsive_col-s instead.
This sort of overwrote almost the entire minima theme with my own tailor-made one & made the design of my site much more under my control. 

I further went to many files such as post.html, page.html, etc and removed all inessentials, keeping the bare minimum.   
NB. : I could have deleted post.html & page.html & used default.html everywhere but I need to add title & date in heading only for these & not for home page, about page, etc...so I kept it  
NB. I could have deleted post.html & used the page.html layout in posts as well. The only difference is that a post has a Date after the title whilst a page has a 'Last updated' phrase before the date. There's no other difference. But because of this small diff., I chose to keep the 2 layout files separate.







***Creating pagination***

Ref https://ouyi.github.io/post/2017/12/23/jekyll-customization.html  and  https://jekyllrb.com/docs/pagination/ 

1. Install jekyll-paginate gem by adding the line: gem 'jekyll-paginate' in Gemfile & jekyll-paginate plugin in _config.yml. Also specify the no. of pages per page through"paginate:<num>" in _config.yml. Then run 'bundle' in GitBash. A new "paginator" object is now created for your Jekyll repo!
2. Create an /_includes/prev_next.html file wherein the Prev & Next buttons with its functionality is provided.
3. Since we only require pagination in our home page, go to index.html of your LocalRepo folder and replace the earlier list of site.posts with a " for post in paginator.posts " loop (we're substituting the default "site" object of Jekyll with the newly created ""paginator" object.

Note: You will find that after building/serving your website, there is an _index.html within your _sites folder & varoius other folders viz. page2, page3, etc are created in your _sites folder each with their own index.html file. So your original home page (with all posts) is now paginated into multiple pages with a specified no. of posts.

Note: Instead of calling paginator or site object (via paginator.posts or site.posts) in the index.html file of LocalRepo, you can also list each post using "a href.." tag. For this you must know that after building/jekyll serving your Jekyll site, all MD files in your Jekyll repo are compiled into an html file in a corresponding folder within _sites folder. This is the beauty of building your website with Jekyll. For eg. if you have a "2020-01-01-My-first-post.md" in the _posts folder, then on jekyll serve, you'll have a "2020-01-01-My-first-post.html" file in the _sites/_posts/ folder. So if you simply href the /posts/2020-01-01-My-first-post.html file in an ```<a></a>``` tag in index.html. In my site, I used this strategy not for posts, but for my "pages folder" where I stacked all pages and index.html of each page calls various files through href like this. However I am disallowing myself the use of pagination in this way.







***Creating a navigation menu***

Ref https://christianspecht.de/2014/06/18/building-a-pseudo-dynamic-tree-menu-with-jekyll/
It also has the source code in github (ref https://github.com/christianspecht/code-examples/tree/master/jekyll-dynamic-menu/src). 
First create a folder (say 'pages') in your LocalRepo & make your Navigation Menu tree in folder-sub folders form within it.
Now make a _data folder in your LocalRepo & create a menu.yml file which will have many 'items' & 'subitems' each having the same 'text' &'url' as made in the 'pages' folder.
Create a nav.html file in /_includes folder.
Then add the following code in your default.html file :

```

<div>
	<h2> Navigation:</h2>
	 % include nav.html nav=site.data.menu % 
</div>

```

1. So basically when any page in your website(including home page) loads, it will call the default.html layout either directly or indirectly through its frontmatter. 
2. default.html will assign the /data/menu.yml contents into an include.nav parameter & simultaneously loads the /includes/nav.html file.
3. nav.html prints the navigation menu tree with respective URLs using a for loop in liquid


Adding a sitemap tree of all pages in the navigation menu

To do this:
1. Create a 'sitemap' folder in LocalRepo & add an index.html in it which includes /_includes/sitemap.html & assigns an include.map parameter to it.
2. Create an /_includes/sitemap.html file & make a text+url tree of each item in include.map parameter through a for loop in liquid.





