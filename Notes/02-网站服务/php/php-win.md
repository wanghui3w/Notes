# PHP安装
参考：http://www.cnblogs.com/wuzhenbo/p/3493518.html

## 系统环境
* 系统：windows10
* PHP: php-5.6.31

## 下载
http://windows.php.net/download#php-5.6
## 各版本区别
操作系统是32位还是64位？如果是32位的，就选择带“x86”的版本，如果是64位的，就选择带“x64”的版本。然后看WEB服务器是什么？如果是IIS系列，就选择带“Non Thread Safe”的版本，如果是Apache httpd系列，就选择带“Thread Safe”的版本。如果是Apache httpd还要看这个二进制版本是怎么编译的，有三种编译类型，分别是：VC6、VC9、VC11（分别代表Visual C++ 6、Visual C++ 2008、Visual C++ 2012）。

 Windows版的PHP从版本5.2.1开始有ThreadSafe(线程安全)和None Thread Safe(NTS，非线程安全)之分，windows iis使用非线程安全版本。

## 安装
### 解压至目录
```
D:\app\php-5.6.31-nts-Win32-VC11-x64
```
### 检查版本
```
D:\app\php-5.6.31-nts-Win32-VC11-x64>php -v
PHP 5.6.31 (cli) (built: Jul  5 2017 22:24:47)
Copyright (c) 1997-2016 The PHP Group
Zend Engine v2.6.0, Copyright (c) 1998-2016 Zend Technologies
```
### 配置php.ini文件
```
extension_dir = "D:\app\php-5.6.31-nts-Win32-VC11-x64\ext"
date.timezone = Asia/Shanghai
enable_dl = On
cgi.force_redirect = 0
fastcgi.impersonate = 1
cgi.rfc2616_headers = 1
```
### 启动
```
php-cgi.exe -b 127.0.0.1:9000 -c D:\app\php-5.6.31-nts-Win32-VC11-x64\php.ini
```
### 集成nginx环境

[参考nginx](../nginx/nginx.md#集成php环境)

### php.ini配置文件参考如下：
```
[PHP]
engine = On
short_open_tag = Off
asp_tags = Off
precision = 14
output_buffering = 4096
zlib.output_compression = Off
implicit_flush = Off
unserialize_callback_func =
serialize_precision = 17
disable_functions =
disable_classes =
zend.enable_gc = On
expose_php = On
max_execution_time = 30
max_input_time = 60
memory_limit = 128M
error_reporting = E_ALL
display_errors = On
display_startup_errors = On
log_errors = On
log_errors_max_len = 1024
ignore_repeated_errors = Off
ignore_repeated_source = Off
report_memleaks = On
track_errors = On
html_errors = On
variables_order = "GPCS"
request_order = "GP"
register_argc_argv = Off
auto_globals_jit = On
post_max_size = 8M
auto_prepend_file =
auto_append_file =
default_mimetype = "text/html"
default_charset = "UTF-8"
doc_root =
user_dir =
extension_dir = "D:\app\php-5.6.31-nts-Win32-VC11-x64\ext"
enable_dl = On
cgi.force_redirect = 0
rfc2616_headersfastcgi.impersonate = 1
cgi.rfc2616_headers = 1
file_uploads = On
upload_max_filesize = 2M
max_file_uploads = 20
allow_url_fopen = On
allow_url_include = Off
default_socket_timeout = 60
extension=php_mysql.dll
extension=php_mysqli.dll
[CLI Server]
cli_server.color = On
[Date]
date.timezone = Asia/Shanghai
[filter]
[iconv]
[intl]
[sqlite3]
[Pcre]
[Pdo]
[Pdo_mysql]
pdo_mysql.cache_size = 2000
pdo_mysql.default_socket=
[Phar]
[mail function]
SMTP = localhost
smtp_port = 25
mail.add_x_header = On
[SQL]
sql.safe_mode = Off
[ODBC]
odbc.allow_persistent = On
odbc.check_persistent = On
odbc.max_persistent = -1
odbc.max_links = -1
odbc.defaultlrl = 4096
odbc.defaultbinmode = 1
[Interbase]
ibase.allow_persistent = 1
ibase.max_persistent = -1
ibase.max_links = -1
ibase.timestampformat = "%Y-%m-%d %H:%M:%S"
ibase.dateformat = "%Y-%m-%d"
ibase.timeformat = "%H:%M:%S"
[MySQL]
mysql.allow_local_infile = On
mysql.allow_persistent = On
mysql.cache_size = 2000
mysql.max_persistent = -1
mysql.max_links = -1
mysql.default_port =
mysql.default_socket =
mysql.default_host =
mysql.default_user =
mysql.default_password =
mysql.connect_timeout = 60
mysql.trace_mode = Off
[MySQLi]
mysqli.max_persistent = -1
mysqli.allow_persistent = On
mysqli.max_links = -1
mysqli.cache_size = 2000
mysqli.default_port = 3306
mysqli.default_socket =
mysqli.default_host =
mysqli.default_user =
mysqli.default_pw =
mysqli.reconnect = Off
[mysqlnd]
mysqlnd.collect_statistics = On
mysqlnd.collect_memory_statistics = On
[OCI8]
[PostgreSQL]
pgsql.allow_persistent = On
pgsql.auto_reset_persistent = Off
pgsql.max_persistent = -1
pgsql.max_links = -1
pgsql.ignore_notice = 0
pgsql.log_notice = 0
[Sybase-CT]
sybct.allow_persistent = On
sybct.max_persistent = -1
sybct.max_links = -1
sybct.min_server_severity = 10
sybct.min_client_severity = 10
[bcmath]
bcmath.scale = 0
[browscap]
[Session]
session.save_handler = files
session.use_strict_mode = 0
session.use_cookies = 1
session.use_only_cookies = 1
session.name = PHPSESSID
session.auto_start = 0
session.cookie_lifetime = 0
session.cookie_path = /
session.cookie_domain =
session.cookie_httponly =
session.serialize_handler = php
session.gc_probability = 1
session.gc_divisor = 1000
session.gc_maxlifetime = 1440
session.referer_check =
session.cache_limiter = nocache
session.cache_expire = 180
session.use_trans_sid = 0
session.hash_function = 0
session.hash_bits_per_character = 5
url_rewriter.tags = "a=href,area=href,frame=src,input=src,form=fakeentry"
[MSSQL]
mssql.allow_persistent = On
mssql.max_persistent = -1
mssql.max_links = -1
mssql.min_error_severity = 10
mssql.min_message_severity = 10
mssql.compatibility_mode = Off
mssql.secure_connection = Off
[Assertion]
[COM]
[mbstring]
[gd]
[exif]
[Tidy]
tidy.clean_output = Off
[soap]
soap.wsdl_cache_enabled=1
soap.wsdl_cache_dir="/tmp"
soap.wsdl_cache_ttl=86400
soap.wsdl_cache_limit = 5
[sysvshm]
[ldap]
ldap.max_links = -1
[mcrypt]
[dba]
[opcache]
[curl]
[openssl]

```
