# Tomcat && JAVA
**所有web服务器上线前需要修改limit**

## 基本概念
* jdk：java开发工具包（包含jre）
* jre：java运行环境
* SDK: 软件开发工具包
* j2ee:企业应用类库
* j2se:标准应用类库
* j2me:手机微型应用类库
* jdk: JDK是 Java 语言的软件开发工具包
## 下载
tomcat：\
http://tomcat.apache.org/download-80.cgi#8.0.44

jdk：\
http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

## 创建用户，统一uid，方便以后运维
```
[root@ops-82 ~]# useradd -u 601 tomcat
[root@ops-82 ~]# passwd tomcat
Changing password for user tomcat.
New password:
BAD PASSWORD: The password is shorter than 8 characters
Retype new password:  
passwd: all authentication tokens updated successfully.
[root@ops-82 ~]# id tomcat
uid=601(tomcat) gid=1000(tomcat) groups=1000(tomcat)
[root@ops-82 ~]#

不确定uid、gid不相等原有， 使用下述方法创建
[root@ops-81 ~]# groupadd -g 601 tomcat
[root@ops-81 ~]# useradd -u 601 -g 601 tomcat
```

## 安装
```
[root@ops-82 backup]# tar zxf jdk-8u131-linux-x64.tar.gz
root@ops-82 backup]# mv jdk
jdk1.8.0_131/               jdk-8u131-linux-x64.tar.gz
[root@ops-82 backup]# mv jdk
jdk1.8.0_131/               jdk-8u131-linux-x64.tar.gz  
[root@ops-82 backup]# mv jdk1.8.0_131/ /usr/local/
[root@ops-82 backup]# ln -s /usr/local/jdk1.8.0_131/ /usr/local/jdk
[root@ops-82 backup]#
[root@ops-82 backup]# tar zxf apache-tomcat-8.0.44.tar.gz
[root@ops-82 backup]# mv apache-tomcat-8.0.44 /usr/local
[root@ops-82 backup]# ln -s /usr/local/apache-tomcat-8.0.44/ /usr/local/tomcat
[root@ops-82 backup]#
```

## 设置环境变量
```
[root@ops-82 local]# vim /etc/profile
export JAVA_HOME=/usr/local/jdk
export PATH=$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH
export CLASSPATH=.$CLASSPATH:$JAVA_HOME/lib:$JAVA_HOME/jre/lib:$JAVA_HOME/lib/tools.jar
export TOMCAT_HOME=/usr/local/tomcat
[root@ops-82 local]# source /etc/profile
[root@ops-82 local]# java -version
java version "1.8.0_131"
Java(TM) SE Runtime Environment (build 1.8.0_131-b11)
Java HotSpot(TM) 64-Bit Server VM (build 25.131-b11, mixed mode)
[root@ops-82 local]#
```

##设置权限并切换用户
```
[root@ops-82 local]# chown -R tomcat.tomcat /usr/local/jdk
[root@ops-82 local]# chown -R tomcat.tomcat /usr/local/tomcat/
[root@ops-82 local]# su - tomcat
Last login: Wed Jun 28 11:09:05 CST 2017 on pts/0
[tomcat@ops-82 ~]$
```

## 启动
```
[tomcat@ops-82 ~]$ cd /usr/local/tomcat/bin/
[tomcat@ops-82 bin]$ ./startup.sh
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/jdk
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
Tomcat started.
[tomcat@ops-82 bin]$
```
默认8080端口，检查端口\
`[tomcat@ops-82 bin]$ netstat -nltp`\
检查日志`/usr/local/tomcat/logs`
```
[tomcat@ops-82 logs]$ ls
catalina.2017-06-28.log  host-manager.2017-06-28.log  localhost_access_log.2017-06-28.txt
catalina.out             localhost.2017-06-28.log     manager.2017-06-28.log
[tomcat@ops-82 logs]$
```

## 配置文件
```
[tomcat@ops-82 conf]$ pwd
/usr/local/tomcat/conf
[tomcat@ops-82 conf]$ ls
Catalina         catalina.properties  logging.properties  tomcat-users.xml  web.xml
catalina.policy  context.xml          server.xml          tomcat-users.xsd
[tomcat@ops-82 conf]$
```
默认根路径，一般情况下不修改，默认放在root下是不需要加路径的
```
[tomcat@ops-82 webapps]$ pwd
/usr/local/tomcat/webapps
[tomcat@ops-82 webapps]$ ls
docs  examples  host-manager  manager  ROOT
```
配置用户名密码，增加内容
```
[tomcat@ops-82 conf]$ vim tomcat-users.xml
  <role rolename="manager-gui"/>
  <role rolename="admin-gui"/>
  <user username="tomcat" password="wanghui" roles="manager-gui,admin-gui"/>
```

## 重启f服务
### 临时目录每次重启要清空
`/usr/local/tomcat/temp`
`/usr/local/tomcat/work`

### 关闭
```
[tomcat@ops-82 work]$ cd /usr/local/tomcat/bin/
[tomcat@ops-82 bin]$ ./shutdown.sh
Using CATALINA_BASE:   /usr/local/tomcat
Using CATALINA_HOME:   /usr/local/tomcat
Using CATALINA_TMPDIR: /usr/local/tomcat/temp
Using JRE_HOME:        /usr/local/jdk
Using CLASSPATH:       /usr/local/tomcat/bin/bootstrap.jar:/usr/local/tomcat/bin/tomcat-juli.jar
[tomcat@ops-82 bin]$
```

### 重启、启动、关闭脚本
```
[root@ops-82 bin]# cat tomcat.sh
#!/bin/bash
TOMCAT_PATH=/usr/local/tomcat

usage(){
echo "Usage: $0 [start|stop|status|restart]"
}

status_tomcat(){
ps aux|grep java|grep tomcat |grep -v 'grep'
}

start_tomcat(){
/usr/local/tomcat/bin/startup.sh
}

stop_tomcat(){
TPID=$(ps aux | grep java |grep tomcat | grep -v 'grep' | grep -v 'su' | awk '{print $2}')
kill -9 $TPID
sleep 5;

TSTAT=$(ps aux | grep java |grep tomcat | grep -v 'grep' | grep -v 'su' | awk '{print $2}')
    if [ -Z $TSTAT];then
        echo "tomcat stopped"
    else
        kill -9 $TSTAT
    fi

cd $TOMCAT_PATH

rm temp/* -rf
rm work/* -rf
}


main(){
case $1 in
    start)
        start_tomcat;;
    stop)
        stop_tomcat;;
    status)
        status_tomcat;;
    restart)
        stop_tomcat && start_tomcat;;
    *)
        usage;;
esac
}

main $1
[root@ops-82 bin]#chmod +x tomcat.sh
```

## 安全设置规范
参见《[tomcat-安全设置规范](./tomcat-安全设置规范.md)》
