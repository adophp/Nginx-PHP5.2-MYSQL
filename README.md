# Nginx PHP5.2 MYSQL安装

linux下配置nginx php5.2 mysql环境

想必大多数的情况都是因为程序在高版本中不兼容，修改起来比较麻烦，所以要安装5.2的版本

安装过程中需要的部分软件已经下载到install文件夹里面了。

假定install的软件都放到服务器的数据盘上的，根目录文件夹名称dutuwang

安装过程我们所在位置为/dutuwang/install

站点目录为：/dutuwang/wwwroot/shop,可能服务器上有很多网站需要放这里的目录就取名shop

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
#./configure --prefix=/usr/local/nginx --with-pcre=../pcre-8.00 --with-zlib=../zlib-1.2.11
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
#./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-mbstring \
 --with-mcrypt=/usr/local/libmcrypt-2.5.8 --enable-ftp --with-gd --with-jpeg-dir=/usr \
 --with-png-dir=/usr --with-mysql=/usr/bin/ --with-mysqli=/usr/bin/mysql_config --with-openssl-dir=/usr \
 --with-openssl --with-pdo-mysql --with-pear --enable-sockets --with-freetype-dir=/usr --enable-gd-native-ttf \
 --with-zlib --with-libxml-dir=/usr --with-xmlrpc --enable-zip --enable-fastcgi --enable-fpm --enable-xml \
 --enable-sockets --with-gd --with-zlib --with-iconv=/usr/local/libiconv-1.13.1 --enable-zip \
 --with-freetype-dir=/usr/lib/ --enable-soap --enable-pcntl --enable-cli --with-curl
//注意看过程是否有组件未安装，及时补上
#make
#make install
#strip /usr/local/php/bin/php-cgi
#cp php.ini-recommended /usr/local/php/etc/php.ini

```

主体安装完成，下面就是一些配置信息









