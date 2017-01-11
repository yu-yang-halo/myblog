---
title: 解决同源策略问题之Apache实现反向代理
date:  2016-12-30 16:22:08
tags:  web前端
---
### 解决同源策略问题之Apache实现反向代理
<img src="http://ojlxovd29.bkt.clouddn.com/apache_proxy.png" width="1815" height="224" border="0" alt="">
我们在没有服务端或者有服务端的时候需要访问不同源数据的时候会出现同源策略问题
因此通过设置Apache的反向代理即可轻松的解决同源策略问题。
有反向代理自然有正向代理，如下图:
<img src="http://ojlxovd29.bkt.clouddn.com/proxy.png" width="716" height="453" border="0" alt=""></img>
理解了反向代理和正向代理我们接下来就可以配置Apache服务器了.
##### 从Apache官网下载apache服务器，解压后打开conf/httpd.conf文件
解除下面的注释
```
######################apache代理解除注释 start......########
LoadModule mime_module modules/mod_mime.so
#LoadModule mime_magic_module modules/mod_mime_magic.so
#LoadModule negotiation_module modules/mod_negotiation.so
LoadModule proxy_module modules/mod_proxy.so
#LoadModule proxy_ajp_module modules/mod_proxy_ajp.so
#LoadModule proxy_balancer_module modules/mod_proxy_balancer.so
LoadModule proxy_connect_module modules/mod_proxy_connect.so
#LoadModule proxy_express_module modules/mod_proxy_express.so
#LoadModule proxy_fcgi_module modules/mod_proxy_fcgi.so
LoadModule proxy_ftp_module modules/mod_proxy_ftp.so
#LoadModule proxy_html_module modules/mod_proxy_html.so
#LoadModule proxy_hcheck_module modules/mod_proxy_hcheck.so
LoadModule proxy_http_module modules/mod_proxy_http.so
#LoadModule proxy_http2_module modules/mod_proxy_http2.so
#######################apache代理解除注释 end..........#######
```

##### 更改需要映射的本地根目录,并开放访问权限
```

#
# Note that from this point forward you must specifically allow
# particular features to be enabled - so if something's not working as
# you might expect, make sure that you have specifically enabled it
# below.
#

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "C:\Users\Administrator\Documents\HBuilderProject\WebJSApi"
<Directory "C:\Users\Administrator\Documents\HBuilderProject\WebJSApi">
    #
    # Possible values for the Options directive are "None", "All",
    # or any combination of:
    #   Indexes Includes FollowSymLinks SymLinksifOwnerMatch ExecCGI MultiViews
    #
    # Note that "MultiViews" must be named *explicitly* --- "Options All"
    # doesn't give it to you.
    #
    # The Options directive is both complicated and important.  Please see
    # http://httpd.apache.org/docs/2.4/mod/core.html#options
    # for more information.
    #
    #Options Indexes FollowSymLinks

    #
    # AllowOverride controls what directives may be placed in .htaccess files.
    # It can be "All", "None", or any combination of the keywords:
    #   Options FileInfo AuthConfig Limit
    #
    AllowOverride None

    #
    # Controls who can get stuff from this server.
    #
    Require all granted
</Directory>
```
##### 最后一步，在conf\extra\httpd-vhosts.conf文件中配置代理服务器
```
ProxyRequests Off
#ProxyPass / http://www.baidu.com
#ProxyPassReverse / http://www.baidu.com
```
任何访问代理服务/的请求都会转发到http://www.baidu.com 下面
##### 下面介绍一下Apache的几个常用命令：
httpd.exe -k install   安装Apache服务
httpd.exe -k uninstall 卸载Apache服务
httpd.exe -k start     启动服务
httpd.exe -k stop      停止服务

注意使用这些命令之前切记以管理员身份运行CMD
