---
title: 使用github pages发布hexo blog
date: 2018-02-03 12:09:33
tags:
- github pages
- github
- hexo
---
### github上创建仓库
在github上创建名为USERNAME.github.io的仓库，其中USERNAME为github的名字，不能用别的名字，否则无效。

### 安装git包
`npm install hexo-deployer-git --save`

### _config.yml中添加配置

    deploy:
      type: git
      repo: ssh://git@github.com/lndlwangwei/lndlwangwei.github.io
    branch: master
    
然后通过命令`hexo d`,即可把生成的blog文件发布到github pages上去，通过<https://lndlwangwei.github.io>查看