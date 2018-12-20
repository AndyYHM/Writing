#20180831-Linux环境下Python 3.6.6 的安装说明

_摘要：Python3 安装部署，普通用户，编译安装_  
_Author: andy_yhm@yeah.net_  
_Date: 20180831_  
_关键字：python,python3,ssl,安装，pip_    

# 1. openssl的下载与安装
_python 3若无或未指定openssl，则会报错“pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.”_
```shell
wget https://www.openssl.org/source/openssl-1.1.1-pre9.tar.gz
tar xzf openssl-1.1.1-pre9.tar.gz
cd openssl-1.1.1-pre9/
./config shared --prefix=/home/python/python36/SSL && make && make install
```

# 2. python环境的准备与安装
```shell
wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz
tar -xzf Python-3.6.6.tgz
cd Python-3.6.6/
export LDFLAGS="-L/home/python/python36/SSL/lib/"
export LD_LIBRARY_PATH="/home/python/python36/SSL/lib/"
export CPPFLAGS="-I/home/python/python36/SSL/include -I/home/python/python36/SSL/include/openssl"
./configure --prefix=/home/python/python36/  && make && make install
```

# 3. 优化环境配置
## 3.1 修改.bash\_profile，添加如下内容
```
#
## ENV Settings for python366
#
export LDFLAGS="-L/home/python/python36/SSL/lib/"
export LD_LIBRARY_PATH="/home/python/python36/SSL/lib/"
export CPPFLAGS="-I/home/python/python36/SSL/include -I/home/python/python36/SSL/include/openssl"
PYHOME=/home/python/python36/bin
export PATH=$PYHOME:$PATH
```
## 3.2 建立软连接并使变量生效
```shell
cd /home/python/python36/bin
ln -s pip3 pip
ln -s python3.6 python
source ~/.bash_profile
```

# 4. 验证环境正确安装
```shell
pip install --upgrade pip
pip install virtualenv
pip list
显示结果如下：
Package    Version
---------- -------
pip        18.0
setuptools 39.0.1
virtualenv 16.0.0
```
