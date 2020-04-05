---
title: "终于折腾成功了blog"
date: 2020-04-04T23:16:15+08:00
draft: false
toc: false
images:
tags: 
  - untagged
---

> 一些坑记录下，造福一下像我这样的人吧
### 遇到问题先上网查
* travis自动编译一直出 TOCSS: failed to transform "css/style.css" (text/x-scss): resource "scss/zhang-fork/zhang-fork.github.io/scss/style.scss_c16d144eee185fbddd582cd5e25a4fae"的问题
看样子是模板的问题,发现style.scss_c16d144eee185fbddd582cd5e25a4fae在模板示例文件夹下的resources里面,以为是要把这个加到hugo目录结构中的resources文件夹里,但是搞不定,凡事不能想当然,要是早点查就不会浪费时间了.虽然查了没发现跟这个一样的问题,但是有个类似问题的解决的途径是hugo要用extended版本的.现在还不能确认到底是模板的问题,还是hugo的问题.

### 多测试
* 后来我在本机hugo server了一下,也出现TOCSS: failed to transform的问题,要是我在本机先测试成功再push到github,可能又能节约很多时间吧,(lll￢ω￢).既然用extended可以解决类似问题,下了个extended的hugo,hugo server不报错了,本机多试没坏处.不过也犯了经验主义错误,先没有切换分支的时候本机测试过一遍,以为就没问题了,结果哪知道环境一变就出问题了.还是要多测

### 一定要仔细看帖子，多看英文帖子,并注意时效性
* 在travis.yml的配置修改为下载extended的hugo版本后,travis还是通不过,一直显示hugo: /usr/lib/x86_64-linux-gnu/libstdc++.so.6:缺lib文件,以为是要在配置文件中加script来下载相应的库文件
按照这个方向查,但这其实也是弯路,还好浪费的时间不多,翻issue里面说改dist,我之前在mipad1上装过Ubuntu,反应过来这个dist是发行版的意思,我抄的配置文件里面,dist 是trustly,😰,我还特意找的近一个月的文章
,中文网页很多都是一大抄,看来这个文章抄的也是几年前的英文的帖子.还是要自己仔细看,多用英文google.

### 要多确认,记忆不一定靠谱
* 这个解决后travis还是没通过,显示invalid option "--token="  ,字面上看就是token问题,但是我确认我的token应该没问题,因为我参考的所有文章都是github-token: $GITHUB_TOKEN,但是我在travis的setting里面确认了下,我写的是$GH_TOKEN,真尴尬.

* 在travis.yml改了token后,输入https://zhang-fork.github.io/posts/终于见到了页面,但是一点post就404,一脸懵,还好在网址上发现了问题,config.toml里baseurl不是https://zhang-fork.github.io/posts/.

### 选对路线
* 不敢想要是用hexo,会不会出问题的地方还是多,先前编译psd2html的时候,各种nodejs版本问题,包依赖问题,环境配置问题,简直是地狱,所以这次果断了选了hugo.没想到麻烦也不断,但是网络上的文章,貌似个个都很顺利,不知道是不说,还是我太菜.