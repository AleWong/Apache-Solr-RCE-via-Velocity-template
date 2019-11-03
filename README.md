# Apache-Solr-RCE-via-Velocity-template

[参考链接](https://gist.githubusercontent.com/s00py/a1ba36a3689fa13759ff910e179fc133/raw/fae5e663ffac0e3996fd9dbb89438310719d347a/gistfile1.txt "参考链接")

## 漏洞产生原因：
1. 当攻击者可以直接访问Solr控制台时，可以通过发送类似/节点名/config的POST请求对该节点的配置文件做更改。
1. Apache Solr默认集成VelocityResponseWriter插件，在该插件的初始化参数中的params.resource.loader.enabled这个选项是用来控制是否允许参数资源加载器在Solr请求参数中指定模版，默认设置是false。当设置params.resource.loader.enabled为true时，将允许用户通过设置请求中的参数来指定相关资源的加载，这也就意味着攻击者可以通过构造一个具有威胁的攻击请求，在服务器上进行命令执行。

## 漏洞复现 ：Recurring vulnerability
方法一：手动检测
访问http://x.x.x.x:8983/solr/#/ 进入主界面,点击左侧的Core Selector查看集合名称

![step 1](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/1.png)

使用如下两个payload 
1. 修改集合设置
![step 2](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/2.png)

1. 命令执行 
![step 3](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/3.png)
命令执行成功!

方法二：使用脚本检测
1. ![step4](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/4.png)
1. ![step5](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/5.png)

## 漏洞修复建议:
1. 建议确保网络设置只允许可信的流量与Solr进行通信。
It is recommended to ensure that the network settings only allow trusted traffic to communicate with Solr. 
2. 把params.resource.loader.enabled 指定为false， 然后把配置文件设置成只读。
Specify params.resource.loader.enabled as false, then set the configuration file to read-only. 
默认情况下params.resource.loader.enabled为false是没有配置文件的，如果要修改其对应的值或强制修改，就需要配置文件
By default, params.resource.loader.enabled is false. There is no configuration file. If you want to modify its corresponding value or force modification, you need to configure the file. 
默认情况下，node的配置文件configoverlay.json为读写权限
By default, the node configuration file configoverlay.json is the read-write permission configuration file. 
配置文件路径：(根据实际情况而定)
Path: (depending on the actual situation)
./example/example-DIH/solr/db/conf/configoverlay.json:
./example/example-DIH/solr/mail/conf/configoverlay.json:
./example/example-DIH/solr/solr/conf/configoverlay.json:
把所有的configoverlay.json中的params.resource.loader.enabled设置为false，如果没有configoverlay.json，建议手动创建一个，然后将其权限设置成只读即可。
Set all params.resource.loader.enabled in configoverlay.json to false. If there is no configoverlay.json, it is recommended to create one manually and then set its permissions to only Read it.

EXP描述：
1. 需要 search node 并且 遍历判断可以POST的node
We need to search nodes and reverse which could be POSTde
1. 需要判断操作系统版本输入命令执行操作
Also ,We need to determine the operating system version and decide to enter the command to perform the operation.


