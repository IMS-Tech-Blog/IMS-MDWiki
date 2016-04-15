# php7+nginx1.8+mysql5.6分用户部署

### linux用户结构

- mysql无论是源码编译还是yum安装都作为系统包存在，即直接安装在/usr/local下面
- nginx使用单独的用户去安装和启动。
- php安装在系统中，可以访问所有用户的文件，为所有用户服务。
- 个人用户作为独立用户存在，给真实用户登录用。



**创建用户和用户组**

```bash
useradd www
passwd www
```

```bash
useradd -M -s /sbin/nologin mysql # -M不创建home目录,-s指定shell为不登录
passwd mysql
```


## 系统部署

通过[这个](https://www.centos.org/download/)网站可以获得最新的CentOS镜像，
安装好最新CentOS镜像记得第一件事是`sudo yum -y update` 更新系统包


## 安装相关依赖包

以下是CentOS的库命名，若是Ubuntu的话则一般是`libxxx-dev `

```bash
yum -y install zlib-devel pcre-devel ncurses-devel libmcrypt mhash mcrypt-devel libxml2-devel.x86_64 curl-devel libpng-devel openldap-devel cmake wget gcc-c++ make perl git bison bzip2-devel
```
注意，这里没有安装`openssl`，因为使用系统`openssl`后面会遇到些问题

解压缩源码包:`nginx-1.8.1, mysql-5.6, php-7.0.3, openssl-1.0.1s`

源码安装nginx
`su www` 切换到www用户下，然后
```bash
cd nginx-1.8.1
./configure --prefix=/home/www/nginx-1.8.1\
--with-openssl=/usr/include/openssl\
--with-pcre\
--with-http_stub_status_module
make install
```

## 源码安装mysql

> mysql从5.5开始使用cmake编译系统。

切换到mysql的安装目录
```bash
cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql
sudo make install
```
安装成功之后
```bash
cp support-files/my-default.cnf /etc/my.cnf # 复制配置文件
cp support-files/mysql.server /etc/init.d/mysqld # 复制启动脚本
chmod +x /etc/init.d/mysqld # 给启动脚本执行权限
ln -s /usr/local/mysql/bin/* /usr/local/bin/ # 为可执行的二进制文件做软连接
ln -s /usr/local/mysql/lib/mysql/lib* /usr/lib/ # 为动态链接库做一个软连接
```
至此，mysql已经安装完毕到系统里，下面是mysql的初始化工作。

## mysql初始化

root用户下
```bash
cd /usr/local/mysql
scripts/mysql_install_db --user=mysql # 用MySQL用户安装数据库

chown -R root.mysql /usr/local/mysql/ # 更改安装目录属主为root,属组为mysql
chown -R mysql.mysql /usr/local/mysql/var/ # 更改数据库目录属主和属组都为mysql  /etc/my.cnf 里的datadir
```

最好把/etc/my.cnf下面的所有路径都放在同一个目录下面，然后进行：
```
chown -R mysql.mysql /data/mysql/
```
这样确保权限没有问题。
最后，`service mysqld start` 启动mysql

然后`/usr/local/bin/mysqladmin -u root password '*******'` 创建mysql账号以及密码。

通过mysql的root账号给其他mysql账号授予数据表的权限：
```bash
[prehawk@localhost:~]$ mysql -uroot -p
mysql> create database laravel;
mysql> insert into mysql.user(Host,User,Password) values("localhost","prehawk",password("prehawk"));
mysql> grant all privileges on laravel.* to prehawk@localhost identified by 'prehawk';
mysql> flush privileges;
```

## 源码安装openssl
因为系统安装的openssl不能被php编译时识别，所以要单独源码安装`openssl`
```bash
./config --prefix=/usr/local/openssl
make depend
sudo make install
```

## 源码安装php
```bash
./configure --prefix=/usr/local/php-7.0.2 --enable-fpm --with-fpm-user=php --with-fpm-group=php --with-mysql-sock=/tmp/mysql.sock --without-sqlite3 --without-pdo-sqlite --with-openssl --with-openssl-dir=/usr/local/openssl --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --enable-mysqlnd --with-zlib --with-bz2
```
