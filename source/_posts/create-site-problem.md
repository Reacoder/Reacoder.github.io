title: 建立本站遇到的问题及解决方法
date: 2014-11-22 16:48:25
tags: 
  - nodejs
  - hexo

---

### 1. hexo 安装出现 Warning "root" does not have permission to access the dev dir 的解决办法

添加 `--unsafe-perm`

具体如下：

`sudo npm install --unsafe-perm --verbose -g hexo`

### 2. Upgrade NodeJS to the latest version on Mac os

Here's how I successfully upgraded from v0.8.18 to v0.10.20 without any other requirements like brew etc, (type these commands in terminal):

* `sudo npm cache clean -f` clear you npm cache
* `sudo npm install -g n` install "n" (this might take a while)
* `sudo n stable` upgrade to lastest version


Note that sudo might prompt your password.

If the version number doesn't show up when typing node -v, you might have to reboot.

[参考来源](http://stackoverflow.com/questions/11284634/upgrade-nodejs-to-the-latest-version-on-mac-os)



