---
layout : page
title : Creating authentication for a Jekyll site hosted as a Heroku App - using 'jekyll-auth' & 'Github OAuth Application'
date : 2021-04-28
---

Follow the instructions from https://github.com/benbalter/jekyll-auth and http://fabian-kostadinov.github.io/2014/11/13/installation-of-jekyll-auth/.

1. Sign up on Heroku and create an app(say h-app).
2. Download Heroku CLI from heroku website & run the exe file
3. Follow https://www.youtube.com/watch?v=aUW5GAFhu6s&t=21s to publish your Jekyll site local repo as a Heroku app. It will be published as an unauthenticated app/website.
   Nb. first run 'heroku login'...then run 'git init'...then run 'heroku git:remote -a "h-app"...then add & commit...then git push heroku master.
4. Now follow each & every step in http://fabian-kostadinov.github.io/2014/11/13/installation-of-jekyll-auth/ to create an authenticated site. This tutorial teaches how to use 'jekyll-auth' gem made by benbalter. You need to use the OAuth protocol which is an industry-standard protocol for authentication. Github is used as the OAuth client. When you create an oauth application in github (say g-app) (nb. I used the callback url the same as heroku homepage url), you are provided with an ID & Secret which you need to add to a .env file in your local repo root folder. You also need to create a github org & a team in it (say g-org/g-team)whose name & id you also need to add to the .env file. Note that the jekyll-auth gem & jekyll-auth git folder (as mentioned in step 8 of kostadinov tutorial) query the .env file every time the site is run (locally or remotely).   
Note that when you open the site(ie. heroku app), 
(i) jekyll-auth causes you to run the g-app (ofcourse after you enter your github login credentials). 
(ii) the g-app acts on behalf of g-org members (eg. you)... for this to be possible, when you enter your github login credentials the very 1st time you open the site/app, as a g-org member you'll have to 'grant' authorization to g-app to act on your behalf, and, you'll also need to grant heroku access to g-app. That's it. In subsequent logins, these grants won't be asked, you just login & view the site. To confirm if these grants are effected, visit https://dashboard.heroku.com/account/applications & you'll find github as a 3rd party service. You can also visit github.com/organizations/g-org/settings and then go to the '3rd party access' tab. You'll find g-app is granted access to act on behalf of g-org members. 
(iii) With the above access, g-app redirects you to the callback url...and bingo the flowchart is complete.

Note : Always start your cmd session with 'heroku login'. This will start the heroku cli. Otherwise jekyll-auth would serve the open site (as in jekyll serve) and not the auth site. Even if you push to the remote ie. to heroku master, the auth details (maybe .env details) aren't pushed and so the open site may be published instead. So do start your session with 'heroku login' command.

Note: To serve the auth site locally, run 'jekyll-auth' in cmd. But before that run 'jekyll-auth new'. This will create the necessary files like Rakefile, config.ru, .gitignore & .env in your root folder, if they don't already exist. Prompts as mentioned in Steps 11a,b,c,d,e,f of kostadinov tutorial are no longer asked on 'jekyll-auth new'. Instead just give those details in .env file, or for a one-time session you can run 'set GITHUB_TEAM_ID=12345', 'set GITHUB_CLIENT_ID=12345', etc in cmd (replace set with export in linux)

Heroku toolbelt is discontinued as of 2021 as its features are included in heroku cli. So step 4 of kostadinov tutorial should be omitted.

Note : Do check if sinatra_auth_github gem is installed as a part of jekyll-auth gem...if not install it separately in cmd/gemfile using gem install. Sinatra is the main engine on which jekyll-auth works.

