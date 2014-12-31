---
layout:        post
title:         JDK工具KEYTOOL常用命令
description:   "JDK工具KEYTOOL常用命令"
tags: [分享]
image:
  background: body_bg.gif
comments: true
share: true
---

-genkey         在用户主目录中创建一个默认文件".keystore",还会产生一个mykey的别名，mykey中包含用户的公钥、私钥和证书
-alias          产生别名
-keystore       指定密钥库的名称(产生的各类信息将不在.keystore文件中
-keyalg         指定密钥的算法   
-validity       指定创建的证书有效期多少天
-keysize        指定密钥长度
-storepass      指定密钥库的密码
-keypass        指定别名条目的密码
-dname          指定证书拥有者信息例如：     "CN=firstName,OU=org,O=bj,L=bj,ST=gd,C=cn"
-list           显示密钥库中的证书信息         keytool -list -v -keystore 别名 -storepass ....
-v              显示密钥库中的证书详细信息
-export         将别名指定的证书导出到文件     keytool -export -alias 别名 -file 文件名.crt
-file           参数指定导出到文件的文件名
-delete         删除密钥库中某条目             keytool -delete -alias 别名 -keystore sage
-keypasswd      修改密钥库中指定条目口令       keytool -keypasswd -alias 别名 -keypass .... -new .... -storepass ... -keystore 别名
-import         将已签名数字证书导入密钥库     keytool -import -alias 别名 -keystore 证书名-file 文件名(可以加.crt 后缀)

###命令:

####生成证书

keytool -genkey -keystore 文件名(可包含路径) -keyalg rsa -alias 别名 -validity 有效期

####查看证书

keytool -list -v -keystore 路径

####把证书导出到文件

keytool -export -alias 别名 -keystore 证书名 -rfc -file 文件名(可包含路径)

####修改密码

keytool -keypasswd -alias 别名 -keypass 旧密码 -new 新密码

####导出证书到新的TrustStore

keytool -import -alias 别名 -file 文件名 -keystore truststore


