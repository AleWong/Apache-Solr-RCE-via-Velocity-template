# Apache-Solr-RCE-via-Velocity-template
https://gist.githubusercontent.com/s00py/a1ba36a3689fa13759ff910e179fc133/raw/fae5e663ffac0e3996fd9dbb89438310719d347a/gistfile1.txt

漏洞描述
该漏洞的产生是由于两方面的原因：
1.当攻击者可以直接访问Solr控制台时，可以通过发送类似/节点名/config的POST请求对该节点的配置文件做更改。

2.Apache Solr默认集成VelocityResponseWriter插件，在该插件的初始化参数中的params.resource.loader.enabled这个选项是用来控制是否允许参数资源加载器在Solr请求参数中指定模版，默认设置是false。

当设置params.resource.loader.enabled为true时，将允许用户通过设置请求中的参数来指定相关资源的加载，这也就意味着攻击者可以通过构造一个具有威胁的攻击请求，在服务器上进行命令执行。

漏洞复现 ：Recurring vulnerability
方法一：手动检测
随后访问http://x.x.x.x:8983/solr/#/进入主界面,点击左侧的Core Selector查看集合名称
![step 1](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/1.png)

使用如下两个payload 
Using these payloads

1.修改集合设置
![step 2](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/2.png)

2.命令执行 
Exec
![step 3](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/3.png)

命令执行成功!
Success!

exp.py描述
难点在于
Point :
1.需要 search node 并且 遍历判断可以POST的node
1.We need to search nodes and reverse which could be POSTde

2.需要判断操作系统版本输入命令执行操作
2.Also ,We need to determine the operating system version and decide to enter the command to perform the operation.

result
![step4](https://github.com/AleWong/Apache-Solr-RCE-via-Velocity-template/blob/master/4.png)
