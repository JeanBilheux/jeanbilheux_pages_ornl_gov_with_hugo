+++
date = 2018-01-08
draft = false
tags = ["github", "git"]
title = "How to remove a file from git tracking"
summary = """Remove file from git tracking"""

+++

Let’s suppose you have a file you have been updated for a while (ex: .idea/workspace.xml)  
in your git repository but decided to remove it.

 1. You need to update your  *.gitignore*  file and add this file

    ```buildoutcfg
    .idea/workspace.xml
    ```

 2. Remove all tracking files by cleaning the cached
 
     ```buildoutcfg
    git rm -r –cached .
    ```

 3. commit changes
 
    ```buildoutcfg
    git add .

    git commit -m “Here it is”

    git push
    ```