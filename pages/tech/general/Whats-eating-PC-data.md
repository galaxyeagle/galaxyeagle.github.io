---
layout : page
title: What's eating up your PC data?
date : 2020-07-30
---

Often certain processes running in the background of your PC can suck away a lot of internet data very quickly. Most of these are updates to various softwares, usually Windows softwares. However if you're running on a limited data plan, you'd prefer to delay the updates to a later time. If your data usage suddenly increases, do these 2 steps:  
  
1) First go to 'Check for Updates' settings in your PC. If updates are running, pause them.  
2) If it doesn't slow the data leak, go to Task Manager -> Performance -> Open Resource Monitor (at the bottom) -> then go to Network tab & check the processes consuming data. Right click on the culprit & click 'Suspend' or 'End'.  
  
That's gonna solve your problem. 
Sometimes, your Windows runs some updates in the background everytime you start your PC. To stop this behaviour, go to Run->services.msc->Right click on 'Background Intelligent Transfer Service'-> Properties->Startup type: Manual. This will stop those updates from running auto on startup. You can do it manually when you have lots of data to spend :)


That's it. If you know other strategies, do comment.