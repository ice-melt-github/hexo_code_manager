# hexo_code_manager
hexo 博客代码管理

## 项目运行

1. 搭建nodejs环境
2. 安装hexo-cli,`npm install -g hexo-cli`
3. 本地创建博客代码目录
4. 进入目录,初始化bolg,`hexo init blog`
5. 检测我们的网站雏形
```bash
hexo new test_my_site

hexo g

hexo s
```

## 常用的Hexo 命令
```
npm install hexo -g #安装Hexo
npm update hexo -g #升级 
hexo init #初始化博客
```
命令简写
```
hexo n "我的博客" == hexo new "我的博客" #新建文章
hexo g == hexo generate #生成
hexo s == hexo server #启动服务预览
hexo d == hexo deploy #部署

hexo server #Hexo会监视文件变动并自动更新，无须重启服务器
hexo server -s #静态模式
hexo server -p 5000 #更改端口
hexo server -i 192.168.1.1 #自定义 IP
hexo clean #清除缓存，若是网页正常情况下可以忽略这条命令
```
刚刚的三个命令依次是新建一篇博客文章、生成网页、在本地预览的操作。
## Hexo 配置
- Hexo与GitHub关联

	打开站点的配置文件_config.yml，翻到最后修改为：

	deploy: 
	type: git
	repo: 这里填入你之前在GitHub上创建仓库的完整路径，记得加上 .git
	branch: master
- 安装Hexo Git部署插件

	`npm install hexo-deployer-git --save`
	