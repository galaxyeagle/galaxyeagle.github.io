---
layout : page
title: Python Tidbits
date : 2021-04-23
---


To start with Python in Jupyter notebooks & Anaconda distribution, refer to : https://www.dataquest.io/blog/jupyter-notebook-tutorial/
  
2 imp. package managers : conda & pip. conda is also an envt. manager in the Anaconda navigator. To install a python package, always prefer using conda if a package is available in conda (to check availability, run >>conda search <pkg-name>, and to check if its already installed (with version)run >>conda list). If unavailable in conda, you may use pip install.


Typing >> python   in Anaconda command opens python prompt



>>conda update anaconda .... updates all packages installed in your conda library
>>conda update spyder


>> python --version

To exit python prompt, press Ctrl+D or Ctrl+Z or type >>exit()

To install PyQt5 :
>> pip install pyqtwebengine==5.12.0 --user
>> pip install pyqt5==5.12.0 --user


There are two ways in which you can run a Python program : either inline from the prompt/ipython shell or console/jupyter notebook,  or,  by running a python script (-> which is nothing but a normal .py file)