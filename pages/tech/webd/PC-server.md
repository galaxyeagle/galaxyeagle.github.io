---
layout : page
title : Your PC as a live server
date : 2024-12-21
---


A server is a system which serves .html files or .ipynb Jupyter notebooks to a URL in a browser. Your PC can act as a live server for testing/debugging before you finally commit your app to a cloud server for public hosting and viewing. That HTML file can contain/be linked to CSS, JS codes/files, but ultimately its the .html file being served to the localhost:xxxx/ URL in a browser. Similarly a .ipynb notebook can contain python code, html tags, markdown code, etc., but ultimately its the .ipynb file being served to the localhost:xxxx/ URL in a browser. 

## Serving HTML files

In Sublime Text you can directly right click on an html file you've opened on the screen, and then click "Open in Browser". However I have observed some unexpressed CSS styles when serving site html files of github-pages this way. So for my github-pages site I use the `jekyll serve` command in the terminal to serve the entire website at once. (I use Alt+1 to launch the terminal within Sublime Text. For a tutorial on it refer [here](https://www.geeksforgeeks.org/how-to-use-terminal-in-sublime-text-editor/).
Apart from this jekyll-based github-page repo, any other repo containing an index.html file can also be served from within Sublime Text. To do this Ctrl+P and go to 'Package Install' and install `Browser-Sync` package. Then go to `Preferences`-`Browse Packages` and edit the `browser_sync_launch.js` file. I found the code was launching a settings file instead of my repo displaying the "Cannot GET /Preferences.sublime-settings" error in the browser. So I replaced with [this](https://gist.github.com/galaxyeagle/be26f8444cc3861e206bfb3bb3994f8b). Then you simply need to go the `Browser Sync` button in Sublime & click Launch.
Apart from using your PC, you can use the cloud service [githack](https://raw.githack.com/) to directly launch the main html file of your Github, BitBucket, Gitlab or Sourcehut repo, without making any local copy at all.

## Serving Jupyter Notebooks

They can be launched from Sublime Text using the [Helium](https://github.com/SublimeText/Helium.git) plugin. But that's not so popular. Instead Anaconda distribution or Google Colab provide the best way to edit and simultaneously run Jupyter Notebooks in the localhost:xxxx/ URL in a browser.