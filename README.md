# Nginx PHP5.2 MYSQL配置

linux下配置nginx php5.2 mysql环境

想必大多数的情况都是因为程序在高版本中不兼容，修改起来比较麻烦，所以要安装5.2的版本

安装过程中需要的部分软件已经下载到install文件夹里面了。

假定install的软件都放到服务器的数据盘上的，根目录文件夹名称dutuwang

安装过程我们所在位置为/dutuwang/install

站点目录为：/dutuwang/wwwroot/shop,可能服务器上有很多网站需要放这里的目录就取名shop

1. 安装必须的一些组件
```
#apt-get install gcc g++ make libssl-dev bison build-essential libncurses5-dev cmake autoconf m4 libxml2 libxml2-dev  libjpeg-dev libpng12-dev libfreetype6-dev libmysqlclient-dev libcurl4-gnutls-dev
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









