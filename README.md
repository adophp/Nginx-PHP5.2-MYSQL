# Nginx PHP5.2 MYSQL配置

linux下配置nginx php5.2 mysql环境

想必大多数的情况都是因为程序在高版本中不兼容，修改起来比较麻烦，所以要安装5.2的版本

安装过程中需要的部分软件已经下载到install文件夹里面了。

假定install的软件都放到服务器的数据盘上的，根目录文件夹名称dutuwang

安装过程我们所在位置为/dutuwang/install

站点目录为：/dutuwang/wwwroot/shop,可能服务器上有很多网站需要放这里的目录就取名shop

1. 安装必须的一些组件
```
#apt-get install gcc g++ make libssl-dev bison build-essential libncurses5-dev cmake autoconf m4
```

2. 编译安装pcre zlib

```
#tar -xzvf pcre-8.00.tar.gz
#cd pcre-8.00
#./configure --prefix=/usr/local/pcre-8.00
#make && make install
#make clean

#tar -xzvf zlib-1.2.11.tar.gz
#cd zlib-1.2.11
#./configure --prefix=/usr/local/zlib-1.2.11
#make && make install
#make clean
```









