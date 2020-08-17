---
layout:     post
title:      "第一篇文章"
subtitle:   "换个地方写东西"
date:       2020-08-17
author:     "jygao"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - 生活
---

> “Yeah,a new beginning.”

## 缘由
今天换了个地方写东西，之前自己买了个服务器和域名搭了个worldpress的博客，域名也没有备案(好鬼麻烦)，写了大半年东西，没留意站点安全问题，然后中了病毒，链接都被谷歌提示为诈骗网站。   

服务器那边快到期了，想来想去，觉得Jekyll + GitHubPages来搭建博客更适合我，理由如下：
1. 不用自己管理维护服务器，域名等，可以专注于写东西
2. 有我爱的markdown和git版本管理
3. 排版更灵活自由，随心所欲

在此，感谢大神黄玄上提供的[模板](https://github.com/Huxpro/huxpro.github.io)，直接在他的基础上改了一下,所谓前人栽树，后人乘凉。    

## 搭建
1. 在github上面fork一份大神模板[项目](https://github.com/jygao/jygao.github.io)。
2. 根据_doc/README.zh.md指引进行个性化改造，指引写的非常好，两下上手，直接fork黄玄的仓库不是个好主意，因为后面你要删他写的内容，大神专门给大家单独写了份[模板仓库](https://github.com/Huxpro/huxblog-boilerplate)。我最后才看到这个。
3. 作为没有用过Ruby的小白，搭建环境后碰到两个问题，凭着看报错解决了：    
   - 因为在windows ,Gemfile得加上配置：gem "wdm", ">=0.1.0"，然后bundle install
   - 本地起站点查看时，报"socket.rb ruby permission denied bind(2)",查了一下是应该4000端口被占用，kill掉占用该端口相关服务就好了。
   - 安装ruby后有一个报错：Unable to load the EventMachine C extension; To use the pure-ruby reactor, require 'em/pure_ruby',谷歌了一下[解决方法](https://github.com/oneclick/rubyinstaller2/issues/96) ，大概是版本兼容问题，照着做解决了问题。