---
layout : page
title : Working between Local & Remote Git Repos
date : 2020-07-08
---

If you are sharing your local Git repo on Github, you will have 2 copies of your repo - Local (in your PC) & Remote (in the Github server). You may wish to edit the local repo & want to reflect the changes in the remote repo also, or vice-versa. You may also want to work on a branch and merge changes with the master. For all such exercises, this article shines a light.

**Prerequisite :** I assume your Local & Remote Repos are setup. Further, they're linked using the `git add remote origin <>` command.

### <ins> Working on 'master' branch </ins>

#### Pushing local changes to remote

	<Make local changes>  
	git add .  
	git commit -m "changes"	
	git push origin master

#### Pushing remote changes to local

	<Make remote changes>  
	git fetch origin  
	git merge origin/master 

### <ins> Working on another branch (say 'develop') </ins>

#### Pushing local changes to remote

	git branch develop  
	git checkout develop  
	<Make local changes>  
	git add .  
	git commit -m "changes"  
	git push origin develop  

#### Pushing remote changes to local

##### 1st time

	<Create a new branch 'develop' in github & edit it>    
	git fetch  
	git branch develop origin/develop
		
##### After 1st time  

	<Make changes in 'develop' in github>  
	git fetch  
	git merge origin/develop
		
		
----------------------------------------------------------------------------------------  
After changes to 'develop' branch are made, & you want to merge it with master branch
```
<On master branch> git merge develop
```
	
nb. There's a concept called "pull request" only in github where you can create a "pull request" in github while you're on the develop branch
    This will flag a notification on master branch in github with an option for merge. Command line git has no role in this affair.
	
### Deleting 'develop' branch after its merged with master & no longer needed 
  
To delete local copy : ```git branch -d develop```  
To delete remote copy also : ```git push origin --delete develop```
