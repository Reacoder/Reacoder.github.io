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

### 3. 部署到github 上的问题

采用`hexo deploy -g`部署时，如果是从github的备份blog 拉下来的话，项目里不包含`.deploy`目录，如果部署的话，会生成新的`.deploy`目录，`.deploy`目录中包含`.git`目录，这样部署会清空远程仓库，再把`public`整个放到github 上，效率太低。

*两种办法解决*

* 只在一个地方部署，在其他地方更新blog 备份。
* 如果在多个地方部署的话,如下：
  
```bash
zhao:BlogBackup zhaosam$ cd .deploy/
zhao:.deploy zhaosam$ 
zhao:.deploy zhaosam$ git pull
From https://github.com/Reacoder/Reacoder.github.io
 * branch            master     -> FETCH_HEAD
Already up-to-date.
zhao:.deploy zhaosam$
```

* 如果是第一次，就新建`.deploy`目录，然后克隆部署过的blog，如：`git clone https://github.com/Reacoder/Reacoder.github.io`再执行第二步。




