---
layout:     post
title:      "从wordpress迁移到jekyll"
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
今天换了个地方写东西，之前自己买了个服务器和域名搭了个wordpress的博客，域名也没有备案(好鬼麻烦)，写了大半年东西，没留意站点安全问题，然后中了病毒，链接被谷歌提示为诈骗网站。   

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
   
   
## 迁移
1. 由于用wordpress没有手动设置备份，系统自动备份只保留最近的几个，发现博客站点进不去时备份已经都是有问题的了。这个故事告诉我们备份是多么重要。
2. 转到githubpages后，发现github.io被墙掉了，访问巨慢。于是在github项目setting下设置custom domain，把原来wordpress站点的域名给配置上，然后再阿里购买的域名那边增加新增CNAME的解析记录，访问ok了。
3. 如何把wordpress写的文章转过来？用的是这个工具 [wpXml2Jekyll](https://github.com/theaob/wpXml2Jekyll)，它支持把wordpress导出的xml文件转成对应jekyll支持的md格式。
4. 问题来了：进不去wordpress的管理后台，原来的域名也被使用了。咋整？    
   - 把旧的站点域名暂改为:old.jiangyungao.com，然后dns解析到对应的主机ip。
   - 把对应wordpress的数据库对应的wp_options,wp_site,wp_post 里面的域名改为：old.jiangyungao.com。
   - 还是进不去，把站点内容ftp拉下来看，对照wp_config.php，开trace模式：报数据库连接错误。
   - 找了半天，域名有的地方改漏了(用了多站点网络还有wp_post_2 wp_post_3，表数据有分页的,只看了第一页的o(╯□╰)o)，改完不报数据连接错误了。
   - 文件解析错误，有些文件不完整的，然后下载了一个wordpress找到对应的文件替换上传，折腾很久不断有报错：主题找不到，报空，心累，要放弃了。
   - 后来看到有一个我最开始手动备份的站点内容，是在wordpress安装完之后备份的。于是恢复到这个点，安装流程，安装缺失的主题，配置network站点，最后成功登录后台管理，顺利导出了xml.