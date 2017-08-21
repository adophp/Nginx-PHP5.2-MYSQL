# Nginx PHP5.2.17 MYSQL安装

linux下配置nginx php5.2 mysql环境

想必大多数的情况都是因为程序在高版本中不兼容，修改起来比较麻烦，所以要安装5.2的版本

安装过程中需要的部分软件已经下载到install文件夹里面了。

假定install的软件都放到服务器的数据盘上的，根目录文件夹名称dutuwang

安装过程我们所在位置为/dutuwang/install

站点目录为：/dutuwang/wwwroot/shop,可能服务器上有很多网站需要放,所以我在这里就随便取了一个名shop

```
apt-get update
```

1. 安装必须的一些组件
```
#apt-get install gcc g++ make libssl-dev bison build-essential libncurses5-dev cmake autoconf m4 libxml2 libxml2-dev
libjpeg-dev libpng12-dev libfreetype6-dev libmysqlclient-dev libcurl4-gnutls-dev
```

2. 编译安装pcre zlib libiconv

```
#tar -xzvf pcre-8.00.tar.gz
#cd pcre-8.00
#./configure --prefix=/usr/local/pcre-8.00
#make && make install
#make clean
#cd ..

#tar -xzvf zlib-1.2.11.tar.gz
#cd zlib-1.2.11
#./configure --prefix=/usr/local/zlib-1.2.11
#make && make install
#make clean
#cd ..

#tar -zxvf libiconv-1.13.1.tar.gz
#cd libiconv-1.13.1
#./configure --prefix=/usr/local/libiconv-1.13.1
#make && make install
#make clean
#cd ..

#tar -zxvf libmcrypt-2.5.8.tar.gz  
#cd libmcrypt-2.5.8
#./configure --prefix=/usr/local/libmcrypt-2.5.8
#make && make install
#make clean
#cd ..
```

3. 添加安装过程中需要的软链接

```
#ln -s /usr/lib/x86_64-linux-gnu/libssl.so  /usr/lib

#ln -s /usr/lib/x86_64-linux-gnu/libjpeg.so  /usr/lib

#ln -s /usr/lib/x86_64-linux-gnu/libpng.so  /usr/lib

#ln -sf /usr/include/freetype2 /usr/include/freetype2/freetype

#ln -s /usr/lib/x86_64-linux-gnu/libmysqlclient.so /usr/lib

```

4. 安装nginx

```
#tar -xzvf nginx-1.12.1.tar.gz
#cd nginx-1.12.1
#./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-pcre=../pcre-8.00 --with-zlib=../zlib-1.2.11
#make
#make install
#make clean
#cd ..

```

5. mysql安装,要输入两次密码,这里没有源码安装有点遗憾

```
apt-get install mysql-server
```

6. php5.2.17安装

```
#tar -xzvf php-5.2.17.tar.gz

//添加php5.2.17补丁,就是增加php-fpm
#wget http://php-fpm.org/downloads/php-5.2.17-fpm-0.5.14.diff.gz
#gzip -cd php-5.2.17-fpm-0.5.14.diff.gz | patch -d php-5.2.17 -p1

//报错：php-5.2.17/ext/dom/node.c:1953:21: error: dereferencing pointer to incomplete type     ret = buf->bu
#curl -o php-5.2.17.patch https://mail.gnome.org/archives/xml/2012-August/txtbgxGXAvz4N.txt
#cd php-5.2.17/
#patch -p0 -b < ../php-5.2.17.patch
#cd ..

//报错：php-5.2.17/ext/openssl/xp_ssl.c:357: undefined reference to `SSLv2_server_method'
//php-5.2.17/ext/openssl/xp_ssl.c:337: undefined reference to `SSLv2_client_method'
//补丁禁用openssl的SSLv2_client_method
#wget http://www.centos.bz/wp-content/uploads/2012/06/debian_patches_disable_SSLv2_for_openssl_1_0_0.patch
#cd php-5.2.17/
#patch -p1 < ../debian_patches_disable_SSLv2_for_openssl_1_0_0.patch

//主角
5.2.17
#./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-mbstring \
 --with-mcrypt=/usr/local/libmcrypt-2.5.8 --enable-ftp --with-gd --with-jpeg-dir=/usr \
 --with-png-dir=/usr --with-mysql=/usr/bin/ --with-mysqli=/usr/bin/mysql_config --with-openssl-dir=/usr \
 --with-openssl --with-pdo-mysql --with-pear --enable-sockets --with-freetype-dir=/usr --enable-gd-native-ttf \
 --with-zlib --with-libxml-dir=/usr --with-xmlrpc --enable-zip --enable-fastcgi --enable-fpm --enable-xml \
 --enable-sockets --with-gd --with-zlib --with-iconv=/usr/local/libiconv-1.13.1 --enable-zip \
 --with-freetype-dir=/usr/lib/ --enable-soap --enable-pcntl --enable-cli --with-curl

5.5.38
#wget http://cn2.php.net/distributions/php-5.5.38.tar.gz
#./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-mbstring \
 --with-mcrypt=/usr/local/libmcrypt-2.5.8 --enable-ftp --with-gd --with-jpeg-dir=/usr \
 --with-png-dir=/usr --with-mysql --with-mysqli --with-openssl-dir=/usr \
 --with-openssl --with-pdo-mysql --with-pear --enable-sockets --with-freetype-dir=/usr --enable-gd-native-ttf \
 --with-zlib --with-libxml-dir=/usr --with-xmlrpc --enable-zip --enable-fpm --enable-xml \
 --enable-sockets --with-gd --with-zlib --with-iconv=/usr/local/libiconv-1.13.1 --enable-zip \
 --with-freetype-dir=/usr/lib/ --enable-soap --enable-pcntl --enable-cli --with-curl --enable-opcache

//注意看过程是否有组件未安装，及时补上
#make
#make install
#strip /usr/local/php/bin/php-cgi
#cp php.ini-recommended /usr/local/php/etc/php.ini

```

主体安装完成，下面就是一些配置信息

vi /etc/php5/etc/php-fpm.conf
<!-- <value name="user"></value> -->
<!-- <value name="group">nobody</value> -->  
默认与nginx.conf 用户名 一致，都为nobody修改为：
<value name="user">nobody</value>
<value name="group">nobody</value>

添加php加速器

```
#tar -xzvf eaccelerator-eaccelerator-42067ac.tar.gz -C /dutuwang/install/eaccelerator
#cd eaccelerator
#/usr/local/php/bin/phpize
#./configure --enable-shared --with-php-config=/usr/local/php/bin/php-config
#make && make install
#make clean

```

在php.ini中添加eaccelerator配置信息,注意extensio的位置

```
extension="lib/php/extensions/no-debug-non-zts-20060613/eaccelerator.so"
eaccelerator.shm_size="128"
eaccelerator.cache_dir="/tmp/eaccelerator"
eaccelerator.enable="1"
eaccelerator.optimizer="1"
eaccelerator.check_mtime="1"
eaccelerator.debug="0"
eaccelerator.filter="*.php"
eaccelerator.shm_max="0"
eaccelerator.shm_ttl="0"
eaccelerator.shm_prune_period="0"
eaccelerator.shm_only="0"

;这里是control.php所有的目录,control.php在安装包里面需要拷贝到此目录，用户与密码在文件内
eaccelerator.allowed_admin_path="/dtw/wwwroot/shop/"
```


mysql php-fpm nginx的配置参数就需要根据自己的服务器来设定了，请查看“原始安装记录文件.txt”里面的部分链接


相当启动位置
```
//nginx
#/usr/local/nginx/sbin/nginx

//php 5.3.3 下的php-fpm 不再支持 php-fpm 以前具有的 /usr/local/php/sbin/php-fpm (start|stop|reload)等命令，需要使用信号控制：http://blog.csdn.net/heirenheiren/article/details/8057506
//php-fpm
#/usr/local/php/sbin/php-fpm start
5.5重启php-fpm
#killall php-fpm
#/usr/local/php/sbin/php-fpm
平滑重启:
#kill -USR2 `cat /usr/local/php/var/run/php-fpm.pid`

//mysql
#service mysql start

```


添加ftp
```
apt-get install vsftpd
```
添加用户：
```
useradd -d /opt/reconciliation -s /sbin/nologin -g ftpGroup -G root ftpUser
```
修改用户主目录：
```
chown -R ftpUser /dtw/wwwroot
```

主目录限制参考：http://blog.csdn.net/bluishglc/article/details/42398811
```
#chroot_local_user=YES
chroot_list_enable=YES
# (default follows)
chroot_list_file=/etc/vsftpd.chroot_list
```

登录权限限制：
```
write_enable=YES
userlist_enable=YES
userlist_deny=NO
userlist_file=/etc/vsftpd.user_list
allow_writeable_chroot=YES
```
vsftpd.chroot_list与vsftpd.user_list添加用户

ftpUser

修改完成后重启vsftpd