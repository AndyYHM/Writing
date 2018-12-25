
## 我的Blog
> 博客园 https://www.cnblogs.com/piggybaba/
> 个人网站 http://piggybaba.cn   
> GitHub https://github.com/AndyYHM/Writing/


## 简介信息
>摘要：Linux下，自动化创建SSH互信脚本
>Author: andy_yhm@yeah.net
>Date: 20181225
>关键字：Shell脚本, ssh, ssh trust ,auto，SSH互信,/bin/bash




## 脚本输出效果
单一节点上，用户python，执行脚本后，输入三台节点python用户密码，自动化创建SSH互信关系
```bash
$ sh SSH_Trust.sh
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
python@node11's password:
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
python@node12's password:
/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
python@node13's password:
Transfer authorized_keys
authorized_keys                                            100% 1185     1.2KB/s   00:00
known_hosts                                                100%  537     0.5KB/s   00:00
authorized_keys                                            100% 1185     1.2KB/s   00:00
known_hosts                                                100%  537     0.5KB/s   00:00
```

## 功能说明
- 默认支持3节点自动化创建SSH互信关系
- 支持多节点自动化创建SSH互信关系


## 使用说明
- 需要提前编辑好/etc/hosts文件
- 用户名所有主机设置为一致
- 使用前编辑脚本"config to do"部分,节点hostname和用户名
- othernodes参数需以空格” “隔开；
- 执行脚本后，需逐一输入节点用户的密码
- 若主机节点数规模庞大，建议使用`expect`工具，另行编辑脚本；


## 脚本内容

``` bash
#!/usr/bin/env bash


#########################################
# Author: andy_yhm@yeah.net
# Date: 20181225
# Key Word : Shell脚本, ssh, ssh trust ,auto，SSH互信,/bin/bash
#########################################
#
## Config to do
#
node1=node11
node2=node12
node3=node13
othernodes=
user=test

#
## Please Don't edit content below
#
ssh-keygen  -q -P ""  -f $HOME/.ssh/id_rsa > /dev/null
for node in ${node1} ${node2} ${node3} ${othernodes}
do
    if [ "`hostname`" == "$node" ]; then
        ssh-copy-id -o stricthostkeychecking=no $user@$node > /dev/null
    else
        ssh-copy-id -o stricthostkeychecking=no python@$node > /dev/null
        ssh $node 'ssh-keygen  -q -P ""  -f $HOME/.ssh/id_rsa' > /dev/null
        scp -rp $node:$HOME/.ssh/id_rsa.pub ./auth.$node > /dev/null
    fi
done

cat ./auth.* >> $HOME/.ssh/authorized_keys
rm -rf ./auth.*

echo "Transfer authorized_keys"
for node in ${node1} ${node2} ${node3} ${othernodes}
do
  if [ "`hostname`" != "$node" ]; then
        scp -rp $HOME/.ssh/authorized_keys $node:$HOME/.ssh/authorized_keys
        scp -rp $HOME/.ssh/known_hosts $node:$HOME/.ssh/known_hosts

  fi

done

exit 0
```
