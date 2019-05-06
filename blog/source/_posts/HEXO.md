---
layout: post
title: HEXO
date: 2019-05-06 12:47:29
tags: 
	- 建站
---
 hexo建站小计
<!-- more -->

# hexo建站

关于hexo建站的具体步骤可以参考:

[https://zhuanlan.zhihu.com/p/26625249](https://zhuanlan.zhihu.com/p/26625249)

这里仅总结和记录建站过程中个人遇到的问题

## 命令汇总
```bash
# 安装 hexo 和其脚手架
npm install -g hexo
npm install -g hexo-cli

# 升级 hexo
npm update -g hexo

# 初始化blog
hexo init blog 

# 新建文章
hexo n "helloworld"	# hexo new "helloworld"

# 生成站点文件
hexo g	# == hexo generate

# 本地预览发布(热部署),http://localhost:4000/
hexo s	# == hexo server
hexo server -s				#静态模式
hexo server -p 5000 		#更改端口
hexo server -i 192.168.1.1 	#自定义 IP

# 部署
hexo d	# == hexo deploy

# 清除,会删除生成的站点 public 文件夹
hexo clean
```

## 问题汇总

### 数学公式
hexo 默认不支持数学公式,可以进行如下修改

1. 替换默认渲染引擎

	打开项目的`package.json`文件,进行修改,然后运行`npm install`命令

	```
	将
	"hexo-renderer-marked": "^0.3.2", 
	修改为
	"hexo-renderer-kramed": "^0.1.4",
	```

	hexo-renderer-kramed 是 hexo-renderer-marked 的 Fork 修改版，仅针对 MathJax 渲染的语义冲突问题进行了修改。

2. 解决语义冲突

	在项目根目录下，找到`node_modules/kramed/lib/rules/inline.js`文件，在inline变量中做出如下修改：

	第11行的 escape 变量的值,取消了对`\`,`{`,`}`的转义:
	```
		// escape: /^\\([\\`*{}\[\]()#$+\-.!_>])/,
		escape: /^\\([`*\[\]()#$+\-.!_>])/,
	```
	第20行的 em 变量的值,取消对`_.*_`模式内容的转义:
	```
		//em: /^\b_((?:__|[\s\S])+?)_\b|^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
		em: /^\*((?:\*\*|[\s\S])+?)\*(?!\*)/,
	```

3. 按需加载 MathJax
	为了使无需渲染公式的页面不引入 `MathJax.js`,进行如下配置

	先在主题配置文件`theme/_config.yml`中，增加`MathJax`配置(有些主题默认有 MathJax 配置,修改为true即可)：
	```
	# MathJax Support
	mathjax:
	  enable: true
	  per_page: false
	  cdn: //cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML
	```

	增加 `layout/_partial/plugins/mathjax.ejs` 文件,内容如下

	```html
	<% if (theme.mathjax){ %>
	<!-- mathjax config similar to math.stackexchange -->

	<script type="text/x-mathjax-config">
	MathJax.Hub.Config({
		tex2jax: {
			inlineMath: [ ['$','$'], ["\\(","\\)"]  ],
			processEscapes: true,
			skipTags: ['script', 'noscript', 'style', 'textarea', 'pre', 'code']
		}
	});

	MathJax.Hub.Queue(function() {
		var all = MathJax.Hub.getAllJax(), i;
		for(i=0; i < all.length; i += 1) {
			all[i].SourceElement().parentNode.className += ' has-jax';
		}
	});
	</script>

	<script async src="//cdn.bootcss.com/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML" async></script>
	<% } %>

	```

	最后在head中引入此文件(我的head配置在`layout/_partial/head.ejs`,不同主题有可能会不一样)

	```html
	<%- partial('plugins/mathjax') %>
	```
4. 在文章的Front-matter里打开mathjax开关，如下：
	```
	---
	title: index.html
	date: 2018-07-05 12:01:30
	tags:
	mathjax: true
	--
	```