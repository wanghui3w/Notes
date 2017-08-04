# a
```

```
# b
```

```
# c
```
curl -I 查看响应header信息，-s 安静，-w 获取状态吗
```
# d
```
diff file1 file2
```
# e
```

```
# f
```

```
# g
```
grep -v "#|^$" nginx.conf #排除注释‘#’和空行；
     -r 递归
     -n 显示行号
grep -rn wanghui ./
```
# h
```

```
# i
```
# 临时添加ip
ip addr add 10.0.0.10/24 dev eth0
```
# j
```

```
# k
```

```
# l
```
# list open files
lsof  
```
# m
```
# md5校验
md5sum * > tmp.md5
md5sum -c tmp.md5
# md5批量校验
find ./ -type f -print0 | xargs -0 md5sum > ./backup.md5
md5sum -c ./backup.md5 |grep -nv OK
```
# n
```

```
# o
```

```
# p
```
ps -ef
```
# q
```

```
# r
```
# 查看文件属于哪个安装包
rpm -qf /usr/bin/htpasswd
# 查看包是否安装
rpm -qa httpd
rpm -ivh 包名 #安装
rpm -e --nodeps 包名 #
rpm -Uvh 包名 #升级
rpm -ql 包名 #查看包里包含的文件
```
# s
```

```
# t
```
#查看文件末尾
tail -n 100 file
tail -f file
tailf file
```
# u
```

```
# v
```

```
# w
```
wget -q 安静，-O 指定文件名
```
# x
```

```
# y
```

```
# z
```

```
