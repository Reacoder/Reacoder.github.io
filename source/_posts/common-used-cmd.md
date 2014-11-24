title: 常用命令
categories:
  - Android
date: 2014-11-24 14:48:34
tags:
  - cmd
  - git
  - adb
 
---
### 1.retrace error log
Proguard wants each "at" on a separate line, and only with white space before it. If it sees anything but white space before the at, it will not unobfuscate it.

每行以at 开头，前面只能有空格，不能有其他。

`/Users/samzhao/Documents/adt-bundle-mac-x86_64-20131030/sdk/tools/proguard/bin/retrace.sh -verbose mapping.txt 1.txt `

### 2.让gitignore 起作用
Even if you haven't tracked the files so far, git seems to be able to "know" about them even after you add them to .gitignore. 
A quick fix that I've used was to run the following commands from the top folder of your git repo:


`git rm -r --cached .`

Followed by:

`git add .`

and

`git commit -m "fixed untracked files"`

### 3.让git不跟踪文件的变化
`git update-index --assume-unchanged <file>`