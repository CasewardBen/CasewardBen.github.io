---
layout: post
title: linux系统下mysql安装及mysql workbench安装配置
category: big data
tages: [linux][datebase]
---
系统环境：ubuntu16

<br/>

# mysql安装
下载mysql源安装包
```python
curl -LO http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
```
安装mysql源
```python
sudo yum localinstall mysql57-community-release-el7-11.noarch.rpm
```
检查yum源是否安装成功
```python
sudo yum repolist enabled | grep "mysql.*-community.*"
mysql-connectors-community           MySQL Connectors Community              21
mysql-tools-community                MySQL Tools Community                   38
mysql57-community                    MySQL 5.7 Community Server             130
```
安装mysql
```python
sudo yum install mysql-community-server
```
安装服务
```python
sudo systemctl enable mysqld
```
启动服务
```python
sudo systemctl start mysqld
```
查看服务状态
```python
sudo systemctl status mysqld
```
mysql默认端口3306
查看mysql端口号
```python
sudo mysql -u root
mysql> show global variables like 'port';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| port | 3306 |
+---------------+-------+
```

<br/>
<br/>
<br/>

# 安装mysql workbench
```python
sudo aptitude install -y mysql-workbench
```
运行mysql workbench
```python
sudo mysql-workbench
```

<br/>

**现在sparl workbench连接mysql数据库会报错**

解决方法如下：

更改mysql的root密码
```python
mysql -u root -p
```
在mysql>后输入
```python
update mysql.user set authentication_string=password('newpassword') where user='root' and host='127.0.0.1' or host='localhost';
```

更改mysql默认UNIX auth_socket plugin插件
```python
sudo mysql -u root

mysql>USE mysql;
mysql>SELECT User, Host, plugin FROM mysql.user;
+------------------+-----------------------+
| User             | plugin                |
+------------------+-----------------------+
| root             | auth_socket           |
| mysql.sys        | mysql_native_password |
| debian-sys-maint | mysql_native_password |
+------------------+-----------------------+

mysql> USE mysql;
mysql> UPDATE user SET plugin='mysql_native_password' WHERE User='root';
mysql> FLUSH PRIVILEGES;
mysql> exit;
```


>备注：过程中如果报错，注意检查自己的密码是否输入错误
