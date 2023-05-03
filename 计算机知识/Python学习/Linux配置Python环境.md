本次安装环境为 Centos7.6

## 1. 安装第三方依赖

`yum install wget zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make zlib zlib-devel libffi-devel -y`

## 2. 编译安装Python

### 2.1 下载 python 源代码

打开 Python 官网，复制目标源代码的下载链接。

`wget 链接地址`下载源码包。

### 2.2 编译源代码

`tar -xvf python3.10.4.tgz`完成源代码解压缩。

进入解压完成的 python3.10.4文件夹，使用 configure 命令检查并配置 python 安装路径。

`./configure --prefix=/usr/local/python3.10.4`

检查配置完成，开始编译：

`make && make install`

编译完成的 python 可执行文件都在 /usr/local/python3.10.4 文件夹中。

## 3. 配置 Python3 为默认版本

删除默认python软链接并 Python3 软链接至 python

`rm -f /usr/bin/python`

`ln -s /usr/local/python3.10.4/bin/python3.10 /usr/bin/python`

修改 yum 配置的 python 版本号为 python2（原始版本号）.

`vim /usr/libexec/urlgrabber-ext-down`将首行 /usr/bin/python 修改为 /usr/bin/python2

`vim /usr/bin/yum`将首行 /usr/bin/python 修改为 /usr/bin/python2

## 4. 设置 pip3 为默认版本

此时 pip3 在 python 安装目录 bin 下，路径：/usr/local/python3.10.4/bin/pip3

建立软链接至 /usr/bin/pip `ln -s /usr/local/python3.10.4/pip3 /usr/bin/pip`

使用 `pip --version`验证版本号。

## 5. pip 更换为国内源

```bash
阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/

豆瓣(douban) http://pypi.douban.com/simple/

清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/
```

### 5.1 仅此软件包更改源

`pip install requests -i http://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com`

### 5.2 永久更改默认源

`vim ~/.pip/pip.conf`

```plain
[global]
index-url = http://mirrors.aliyun.com/pypi/simple/
[install]
trusted-host=mirrors.aliyun.com
```

## 6. 遇到问题

### 6.1 open-ssl 报错

在我编译完成 python，运行时出现报错：Can't connect to HTTPS URL because the SSL module is not available.

大致原因是系统安装的 openssl 默认配置是 python2 的，并且在Python3.7之后的版本，依赖的openssl，必须要是1.1或者1.0.2之后的版本，或者安装了2.6.4之后的libressl。

**解决方案：**

在 openssl 官网（https://www.openssl.org/source/）下载1.0.2或者1.1之后的openssl包，编译安装。

以下安装命令以安装 openssl-1.0.2r 为例：

```bash
[root@localhost ~]# wget http://www.openssl.org/source/openssl-1.0.2r.tar.gz
[root@localhost ~]# tar  zxvf openssl-1.0.2r.tar.gz
[root@localhost ~]# ./config --prefix=/opt/openssl1.0.2r --openssldir=/opt/openssl1.0.2r/openssl no-zlib
[root@localhost ~]# make && make install
[root@localhost ~]#  echo "/opt/openssl1.0.2r/lib" >> /etc/ld.so.conf
[root@localhost ~]#  ldconfig -v
```