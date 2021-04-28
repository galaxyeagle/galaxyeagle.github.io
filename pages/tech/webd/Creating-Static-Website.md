---
layout : page
title : Create a static website using Jekyll & Github
date : 2020-06-26
---

Delivering content/service to an audience (with/out their feedback) is the prime object of any website. Your website files need to be present in some web server (even your PC can be a server !), and these files need to be hosted on a cloud platform which is connected to the Internet (eg. Github pages, AWS, etc) - and bingo ! Your website will go live on the Internet.
In order to create your website files on the server, you'll need a "site-generator" (eg. Static site generators like Jekyll, Hugo, Flask, Django , OR CMSs (for dynamic sites) like Gatsby, Wordpress, Joomla, Drupal,etc).
In this post I'll discuss how to create a website on your Windows PC using Jekyll and then host it on Github pages. A similar process will work on Linux & Mac with some modifications.

Github pages is essentially a cloud platform to host your website. Other such hosting platforms are Heroku, Firebase, Wordpress, Amazon S3, Gitlab pages, etc. Many of these site-hosters like Wordpress have basic tools themselves which allow you to create basic sites directly online without using any site-generator on your local machine. 

Ruby is a programming language used to create individual packages called 'gems'. Each Ruby gem is a complete software application capable of being used in many project. Jekyll is one such Ruby gem which is used to create static (ie. without any database) websites. 

#### So to create your website, the broad steps are :

**1** 	Install the Ruby Environment on your web server (ie. your PC here)  
**2** 	Install the Jekyll gem  
**3** 	Build your website skeleton in any folder of your PC using Jekyll  
**4** 	Modify your website skeleton to fully create your website locally on your PC  

Thereafter to host your website on Github pages, the broad steps are :  
**5** 	Install Git on your PC  
**6** 	Initialize your local website folder (mentioned in Step 3 above) as a Git Repository  
**7** 	Push this local repository to the Github pages platform....And hurray, your website's live !

Prerequisite : You should have signed up on www.github.com

Let us now go through each of these steps in practical detail.

### Step 1
**A.**	Go to the Downloads section of www.rubyinstaller.org & download the latest stable version of Ruby+Devkit  
**B.**	Install it after downloading, preferably in C:/ drive of your PC  
**C.**	Ensure that the folder containing the .exe files of Ruby installation (say folder X) is present in your system PATH. To check it run `gem` on cmd & it should show no errors. If there are errors, go to My Computer( or 'This PC') in your File Explorer -Right click- Properties - Advanced System Settings - Environment Variables. Ensure that the folder X is present in the PATH of both User & System variables : if not, add it. Alternatively this can be checked through the command line using `echo %PATH%` command.  

### Step 2
**A.** Install gems 'jekyll' & 'bundler' using `gem install jekyll bundler` from cmd

### Step 3
**A.** Create a folder anywhere in your PC & give it any dummy name say MyWebsites.  
**B.** cmd in that folder & run : `jekyll new MyLocalSite (or any other name)`.  This will make jekyll create a website skeleton folder named MyLocalSite within the MyWebsites folder

### Step 4
There are so many modifications that can be done to this skeleton folder that it will require a separate post for it. For the time being just go to the _config.yml file and edit the title : <> to your website title. That's it.

### Step 5
**A.** Go to the Downloads section in www.git-scm.com & download the latest stable version of Git  
**B.** Install it in C:/ or C:/Program Files  
**C.** Ensure that the folder containing the .exe files of Git has been added to system PATH.(Run `git` in cmd to check)  

### Step 6
**A.** Right click in MyLocalSite folder & Git Bash   
**B.** Run the following commands in Git Bash to initialize this folder as a git repository
	{% highlight ruby %}
	git init 
	git add .
	git commit -m "My first commit"{% endhighlight %}  
**C.**  Now run : `jekyll serve`(This will run a local copy of your website in your PC).  
	If you now go to localhost:4000 in your web browser you'll see your website running locally on your PC.
	
	
### Step 7	
**A.** Now go to your Github web account & create a *completely empty* new repo in master branch named yourusername.github.io putting your github user name in place of 'yourusername'. You can also create it on a new branch & name it gh-pages branch but I won't recommend it since then your root username.github.io URL will stay blank & your site will be created on a branch of this root URL.  
Copy the https ID (say Y) displayed in the next page.   
**B.** In your Git Bash run :  
	`git remote add origin Y` (where Y is the https ID) (This makes origin = Y)  
	`git remote -v`  (This should display Y if everything's OK)  
	`git push origin master` (This pushes your MyLocalSite repo to the master branch of origin)  
**C.** If you now check your repo in Github, it'll have been populated with your Local Website files.  
**D.** Type yourusername.github.io in your web browser & yay ! Your website's live !

After you make further modifications akin to Step 4, run `bundle exec jekyll build` (or simply running 'jekyll serve' will also serve the same purpose) and then just run 3 the commands in the git bash - _git add, git commit & git push_ - you'll update your website.  

Further points :

Note that Jekyll is a static-site generator. You can create your static site & host it on your local server even without gems like jekyll or bundler or even the Ruby environment ! You'll then need to write ALL codes by hand ! That's not how you should do it though. 
Now focussing on Jekyll, it has different themes. By default, 'minima' is used, which is also a ruby gem in the Jekyll family. You can find gem "minima" written in your Gemfile by default.  
In order to change the theme, add disqus comments, add a sitemap, etc, you'll mainly need to edit the following :
- Gemfile (lists the *gems*)
- Config.yml file (lists the *plugins* which make settings for the respective gems)
- index.md/index.html
- _includes folder (create a new _includes folder in you LocalSite folder & copy the relevant files such as head.html,etc from C:\Ruby26-x64\lib\ruby\gems\2.6.0\gems\minima-2.5.1\_includes folder into your LocalSite folder & edit as needed or create new files if needed. The original files will be overriden.)
- _layouts folder (follow same instructions as in _includes. Usually editing the 4 files present in this folder (default.html, home.html, page.html, post.html) suffices).

Infact the C:\Ruby26-x64\lib\ruby\gems\2.6.0\gems\minima-2.5.1\ folder will have the following files:

.
├── LICENSE.txt
├── README.md
├── _includes
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── google-analytics.html
│   ├── head.html
│   ├── header.html
│   ├── icon-github.html
│   ├── icon-github.svg
│   ├── icon-twitter.html
│   └── icon-twitter.svg
├── _layouts
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   └── post.html
├── _sass
│   ├── minima
│   │   ├── _base.scss
│   │   ├── _layout.scss
│   │   └── _syntax-highlighting.scss
│   └── minima.scss
└── assets
    └── main.scss


All these files can be overriden in your LocalSite folder as per requirement. (ref: https://jekyllrb.com/docs/themes/#overriding-theme-defaults )

That's it folks ! If you've any queries, let me know in the comments. 




















