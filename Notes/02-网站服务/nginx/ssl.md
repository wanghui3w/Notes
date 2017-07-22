
# Nginx配置SSL证书部署HTTPS网站

调试环境：

* 系统：win10

* Apache： Apache/2.4.26 (Win64)

* nginx:nginx/1.12.1


### 1.生成一个RSA密钥(密码abcd)
`$ openssl genrsa -des3 -out abcd.key 1024`
```
D:\app\Apache24\conf>openssl genrsa -des3 -out abcd.key 1024
WARNING: can't open config file: /apache24/conf/openssl.cnf
Generating RSA private key, 1024 bit long modulus
.....++++++
.++++++
e is 65537 (0x10001)
Enter pass phrase for abcd.key:
Verifying - Enter pass phrase for abcd.key:

D:\app\Apache24\conf>
```
### 2. 拷贝一个不需要输入密码的密钥文件
`$ openssl rsa -in abcd.key -out abcd_nopass.key`
```
D:\app\Apache24\conf>openssl rsa -in abcd.key -out abcd_nopass.key
WARNING: can't open config file: /apache24/conf/openssl.cnf
Enter pass phrase for abcd.key:
writing RSA key

D:\app\Apache24\conf>
```

### 3.生成一个证书请求
提示找不到`openssl.cnf`时，需配置环境变量`set OPENSSL_CONF=openssl.cnf`。
`$ openssl req -new -key abcd.key -out abcd.csr`
```
D:\app\Apache24\conf>set OPENSSL_CONF=openssl.cnf

D:\app\Apache24\conf>openssl req -new -key abcd.key -out abcd.csr
Enter pass phrase for abcd.key:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:CN
State or Province Name (full name) [Some-State]:Beijing
Locality Name (eg, city) []:Beijing
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Abcd Ltd
Organizational Unit Name (eg, section) []:abcd
Common Name (e.g. server FQDN or YOUR name) []:*.abcd.com
Email Address []:admin@abcd.com

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:

D:\app\Apache24\conf>
```
### 4.自己签发证书
`$ openssl x509 -req -days 365 -in abcd.csr -signkey abcd.key -out abcd.crt`
```
D:\app\Apache24\conf>openssl x509 -req -days 365 -in abcd.csr -signkey abcd.key -out abcd.crt
Signature ok
subject=/C=CN/ST=Beijing/L=Beijing/O=Abcd Ltd/OU=abcd/CN=*.abcd.com/emailAddress=admin@abcd.com
Getting Private key
Enter pass phrase for abcd.key:

D:\app\Apache24\conf>
```

### 5.编辑配置文件nginx.conf
```
#HTTPS server

server {
    listen       443 ssl;
    server_name  abcd.com;

    ssl_certificate      keys/abcd.crt;
    ssl_certificate_key  keys/abcd_nopass.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
        root   html/ssl;
        index  index.html index.htm;
    }
}
```
`ssl_certificate_key  keys/abcd_nopass.key;`配置为`abcd.key`时，每次启动nginx时需要输入密码，可能windows的问题，无法启动；配置为`abcd_nopass.key`即可正常启动;在win10环境下使用openssl制作ca的证书文件时出现问题：Unable to write ‘random state’，需要在“管理员”模式下运行。
