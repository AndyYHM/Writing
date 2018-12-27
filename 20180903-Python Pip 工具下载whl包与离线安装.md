## 20180903 - Python Pip 工具下载whl包与离线安装


## 1. 我的Blog
> 博客园 https://www.cnblogs.com/piggybaba

> 个人网站 http://piggybaba.cn

> GitHub https://github.com/AndyYHM/Writing


## 2. 简介信息
> 摘要：Linux下，python，pip工具离线安装包

> Author: andy_yhm@yeah.net

> Date: 20180903

> 关键字：python，python3,pip,pip3,requirements.txt,freeze


## 3. 查看当前环境
> 项目建议使用virtualenv，方便后期部署于发布

```bash
(vir_test) [python@piggy ~]$ pip freeze > requirements.txt
```
## 4. 联网环境下载相关安装包
```bash
(vir_test) [python@piggy ~]$ mkdir  /tmp/whl
(vir_test) [python@piggy ~]$ pip download  -d /tmp/whl -r requirements.txt
(vir_test) [python@piggy ~]$ ls -l /tmp/whl/
total 18128
-rw-rw-r-- 1 python python   101571 Sep  3 16:31 asn1crypto-0.24.0-py2.py3-none-any.whl
...
```
## 5. 待打包文件目录
```bash
requirements.txt
/tmp/whl/
```
## 6. 查看离线新环境
```bash
(vir_test1) [python@piggy Project]$ pip list
Package    Version
---------- -------
pip        18.0
setuptools 40.2.0
wheel      0.31.1
```
## 7. 安装到离线环境
待打包文件目录,转载到离线环境
```bash
(vir_test1) [python@piggy ~]$ pip install --no-index  -f /tmp/whl -r requirements.txt
```
## 8. 结果检查
```bash
(vir_test1) [python@piggy ~]$ pip list
Package      Version
------------ -------
asn1crypto   0.24.0
bcrypt       3.1.4
cffi         1.11.5
cryptography 2.3.1
ecdsa        0.13
fabric       2.3.1
idna         2.7
invoke       1.1.1
numpy        1.15.1
paramiko     2.4.1
pip          18.0
pyasn1       0.4.4
pycparser    2.18
pycrypto     2.6.1
PyNaCl       1.2.1
setuptools   40.2.0
six          1.11.0
wheel        0.31.1
```
