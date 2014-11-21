# 个人前端博客 http://foyou.github.io/
# 修改过的模版地址:<a href="https://github.com/foyou/Foyou_By_jekyll/">https://github.com/foyou/Foyou_By_jekyll/</a>
<a href="https://github.com/hacke2/hpstr-jekyll-theme" >套用hpstr jekyll模板</a>

##将此模版Fork时候,需要修改如下配置
_config.yml 看到'你的'应该明白那是你要修改的.
about.md    此页面是关于我们页面,内容可以先预览在修改.
CNAME		此处只能有一行数据,里面是'你的github用户名.github.io' 或者你要绑定的个人域名
index.html	修改'你的'信息
_data/navigation.yml
_includes/disqus_comments.html  如果你设置了disqus的用户名,可以去掉上下的if标签,提高速度
_includes/footer.html			把下面的百度统计代码替换成你自己的,不需要可以直接去掉
_includes/navigation.html		这是菜单栏

_posts		里面就是你的博客文章了,采用的静态文件,所以最好每次在本地修改后在用git命令推送到服务器上
			命名规则  2014-11-20-Hello World!.md  年月日-文章标题.md


##在以上基础上，做了如下修改：
* 将google cdn换成 baidu cdn
* 去掉分享到twitter、facebook等国外社区，加入百度分享
* 加入百度站长助手，方便您的统计
* disqus评论
* Read More功能，不想像以前一样文章全显示出来



