title: HEXO博客搭建
date: 2015-10-21 10:29:11
tags:
---
从新浪博客迁移到Github以后遇到了自己的电脑系统崩溃的问题，每次都要在新装系统中重新部署HEXO环境，这里记录下HEXO博客的搭建过程。
HEXO博客是由HEXO框架生成的，而HEXO的安装需要node.js的支持，因此需要首先安装node.js。
在Fedora22系统中，源码安装过程如下。

 1. 从官网下在源码包，目前最新的是node-v4.2.1.
 2. tar -vxzf解压缩.
 3. ./configure --prefix=/usr/local/node    这是我指定的安装位置
 4. make 时间较长，完成之后然后make install即可
 5. 创建软连接。ln -s   /usr/local/node/bin/node  /usr/sbin/node和ln -s   /usr/local/node/bin/npm  /usr/sbin/npm.现在可以输入node - v和npm -v输出版本号，说明node.js安装成功了。npm是node.js的包管理工具。
 6. npm install hexo-cli -g安装hexo.
 7. hexo安装不能直接使用，也需要创建软连接ln -s   /usr/local/node/bin/hexo  /usr/sbin/hexo
 8. 如果要新建博客的话，安装HEXO官网首页的教程继续就可以.这里我把我以前已经搭建过的目录拷贝过来直接操作即可.
 9. 在hexo/目录下（这个是我自己的博客目录）操作即可.

