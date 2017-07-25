# Nginx
## Nginx常用命令
```
nginx -t        #检查配置文件
nginx -s reload #重新载入配置文件
curl -I www.baidu.com
```
## 搭建虚拟主机步骤
1. 增加一个完整的server标签段到结尾处
2. 更改server_name及对应的root根目录
3. 创建server_name域对应的网页根目录，并建立测试文件
4. 检查nginx配置语法，平滑重启
5. 做host解析或DNS解析
6. 浏览器输入网址、curl、wget进行检查

## 主配置文件nginx.conf
```
Nginx配置段
//全局区
Worker_processes 1 ;//有1个工作的子进程，可以进行修改，意义不大，需要争抢CPU资源，一般设置为cpu核心数（cpu*单颗核心）

Event{
//一般是配置nginx链接特性
//如1个work能同时允许多少连接
Worker_connections 1024;//是指一个子进程最大允许连1024个链接
}

http{//配置http服务器的主要段
	server {//这是虚拟主机段
		location{//定位，把特殊的路径或文件再次定位，如image目录单独处理，
			//如php单独处理
    }
  }
}
```


## Nginx虚拟主机配置
### 基于域名的虚拟机
```
server{
  listen  80;
  server_name www.bacd.com;
  location /{
    root html/www;
    index index.html index.htm;
  }
}

server{
  listen  80;
  server_name bbs.bacd.com;
  location /{
    root html/bbs;
    index index.html index.htm;
  }
}

server{
  listen  80;
  server_name chat.bacd.com;
  location /{
    root html/chat;
    index index.html index.htm;
  }
}

```
### 基于端口的虚拟机
```
server{
  listen  80;
  server_name www.bacd.com;
  location /{
    root html/www;
    index index.html index.htm;
  }
}

server{
  listen  81;
  server_name bbs.bacd.com;
  location /{
    root html/bbs;
    index index.html index.htm;
  }
}
```
### 基于IP的虚拟机
```
server{
  listen  10.0.0.10:80;
  server_name www.bacd.com;
  location /{
    root html/www;
    index index.html index.htm;
  }
}

server{
  listen  10.0.0.11:80;
  server_name bbs.bacd.com;
  location /{
    root html/bbs;
    index index.html index.htm;
  }
}
```


### 重启nginx后的检测

## 集成php环境
[配置php环境](../php/php-win.md)，修改nginx.conf,支持php
```
# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

location ~ \.php$ {
    root           html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME  D:/app/nginx-1.12.1/html$fastcgi_script_name;
    include        fastcgi_params;
}
```
`nginx.conf`中`fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name`修改为`fastcgi_param  SCRIPT_FILENAME  D:\app\nginx-1.12.1\html$fastcgi_script_name;`不能访问，提示‘No input file specified’，nginx access.log中出现404，将`\`替换为`/  `正常，`fastcgi_param  SCRIPT_FILENAME  D:/app/nginx-1.12.1/html$fastcgi_script_name`。

编辑index.php文件
```
<?php phpinfo(); ?>
```
执行命令`nginx -s reload`

访问http://localhost/phpinfo.php查看是否可以打开phpinfo页面


## 虚拟主机的别名配置
`server_name`可以添加多个主机名，别名之间用空格分隔
```
server{
  listen  10.0.0.11:80;
  server_name bbs.bacd.com b.abcd.com abcd.com;
  location /{
    root html/bbs;
    index index.html index.htm;
  }
}
```
>集群中多个服务器提供相同的服务，通过前端负载进行分发，为了对每个主机进行检测，可以使用别名分别进行监测
>```
>    ...
>    server_name www.bacd.com www1.abcd.com;
>    ...
>    server_name www.bacd.com www2.abcd.com;
>    ...
>```

## Nginx 日志
### Nginx error_log
`error_log logs/error.log [debug|info|crit|alert|emerg]`

可以设置的标签段为

`#content:main,http,server,location`

### Nginx access_log

**Module:** ngx_http_log_module

http://nginx.org/en/docs/http/ngx_http_log_module.html

`access_log logs/access.log main`

### Nginx 日志切割

1. 创建脚本`runlog.sh`
```
#!/bin/bash
LOGPATH=/usr/local/nginx/logs/abcd.com.access.log
BASEPATH=/data

bak=$BASEPATH/$(date -d yesterday +%Y%m%d%H%M).abcd.com.access.log

mv $LOGPATH $bak
touch $LOGPATH
kill -USR1 `cat /usr/local/nginx/logs/nginx.pid`
```
或者使用`nginx -s reload` 替换kill -USR1 \`cat /usr/local/nginx/logs/nginx.pid\`

2. 添加计划任务（每分钟执行一次）

    `crontab -e`添加计划任务

    `*/1 * * * * sh /data/runlog.sh >/dev/null 2>&1`

    `crontab -l` 查看计划任务

## location
### location语法
```
location[=|~|~*|^~|@] uri{
	...
}
```
```
=：精确匹配
~:大小写敏感匹配
~*:不区分大小写
!：取反
^~:不做正则检查，常规检查
@：
```
### location 测试

```
server {
    listen       1006;
    server_name 192.168.0.121;

    location = / {
        return 401;
    }

    location / {
        return 402;
    }

    location /documents/ {
        return 403;
    }

    location ^~ /images/ {
        return 404;
    }

    location ~* \.(gif|jpg) {
        return 500;
    }

}
```
`curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1006/`
```
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080
401
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/
401
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/abc
402
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/documents
402
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/documents/
403
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/documents/abc
403
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/images/
404
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/IMAGES/
402
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/images/abc.jpg
404
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/images/ABC.JPG
404
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/abc.jpg
500
[root@ops-83 conf.d]# curl -s -o /dev/null -I -w "%{http_code}\n" http://192.168.0.121:1080/abc.JPG
500
[root@ops-83 conf.d]#
```
```
/:默认匹配
=:优先，和顺序无关
^~ 和 ~*：有限匹配^~
```
