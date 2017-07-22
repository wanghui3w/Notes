# Nginx
## Nginx常用命令
```
nginx -t        #检查配置文件
nginx -s reload #重新载入配置文件
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
    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    include        fastcgi_params;
}
```
