+++
title="linux备份数据库到七牛云"
description="linux备份数据库到七牛云"
tags=["base command"]
categories=["linux"]
date="2022-03-15T21:55:00+08:00" 
url="Linux-backup-database-to-Qiniu-cloud.html"
toc=true
+++
>**郑重声明**: 本文首发于[人工博客](https://www.94rg.com)


### 1、导读

现在有些it从业者越来越没底线，经常干一些损人不利己的蠢事。比如扫描端口，获取服务器的操作权限，进行挖矿，对数据的数据进行加密，然后丢个地址，要你去付费解密数据。数据就是一个服务的生命线。一方面我们会吐槽对方没道德，另一方面我们可以加强一下自身。做好服务器的安全措施，重要数据备份工作，这样就算被人攻击了，直接回滚数据就好了，损失可以降到最低。这里我为大家介绍的是基于七牛云的免费oss以及官方的qshell工具对文件夹进行备份。不得不说七牛还是业界比较良心的。

### 2、前置步骤

 + 注册七牛云，实名后可领取10G的空间。获取访问的key和密钥
 + 下载qshell对应的版本。官方提供的都是zip压缩后的二进制版本。这里我遇到了一个坑是下载到windows后解压上传到linux,提示不可用，然后直接在linux上下载解压是ok的。
 + 环境变量配置 /etc/profile中增加就好了 

### 3、关键步骤

#### 3.1 密钥设置
 qshell account ak sk name

其中name表示该账号的名称, 如果ak, sk, name首字母是"-", 需要使用如下的方式添加账号, 这样避免把该项识别成命令行选项:

qshell account -- ak sk name

命令执行完毕会在~/qshell目录下生成对应的配置文件



#### 3.2、创建配置文件
配置文件格式支持json，文件名称不限

```
{
    "src_dir": "/usr/local/xxx/dbbackup", 
    "log_file": "/usr/local/xx/upload.log", 
    "key_prefix": "dataBase/", 
    "bucket": "111"
}
```
    
+ src_dir：服务器的本地目录
+ log_file: 同步的日志的目录
+ key_prefix：七牛云上的存储的前缀，利用这个可以变量处理成文件夹的模式
+ buket 个人的buket名称 

#### 3.2、编写同步的脚本
qshell qupload  /usr/local/xx/upload.conf (第二个参数是步骤二的配置文件的路径)

#### 3.3、加入到定时任务
30 23 * * *   qshell.sh

不懂crontab的可以参考：[crontab表达式的参数说明](https://www.94rg.com/article/1733)

### 4、重要参数说明
`qupload` 功能需要配置文件的支持，配置文件支持的全部参数如下：

```
{
   "src_dir"            :   "<LocalPath>",
   "bucket"             :   "<Bucket>",
   "file_list"          :   "<FileList>",
   "key_prefix"         :   "<Key Prefix>",
   "up_host"            :   "<Upload Host>",
   "ignore_dir"         :   false,
   "overwrite"          :   false,
   "check_exists"       :   false,
   "check_hash"         :   false,
   "check_size"         :   false,
   "rescan_local"       :   true,
   "skip_file_prefixes" :   "test,demo,",
   "skip_path_prefixes" :   "hello/,temp/",
   "skip_fixed_strings" :   ".svn,.git",
   "skip_suffixes"      :   ".DS_Store,.exe",
   "log_file"           :   "upload.log",
   "log_level"          :   "info",
   "log_rotate"         :   1,
   "log_stdout"         :   false,
   "file_type"          :   0
}
```


|参数名|描述|可选参数|
|-----------|------------|------------|
|src_dir|本地同步路径，为全路径格式，工具将同步该目录下面所有的文件；不支持本地路径下的目录软连接。在Windows系统下面使用的时候，注意`src_dir`的设置遵循`D:\\jemy\\backup`这种方式。也就是路径里面的`\`要有两个（`\\`）|N|
|bucket|同步数据的目标空间名称，可以为公开空间或私有空间|N|
|file_list|待同步文件列表，该文件列表内容必须是相对于`src_dir`的文件相对路径列表，可以不指定，工具将自动获取`src_dir`下面的文件列表。请使用 `dircache` 命令生成这个文件列表，生成之后可以手动删除不需要的行|Y|
|up_host|上传域名，可选设置，一般情况下不需要指定|Y|
|ignore_dir|保存文件在七牛空间时，使用的文件名是否忽略本地路径，默认为false|Y|
|key_prefix|在保存文件在七牛空间时，使用的文件名的前缀，默认为空字符串|Y|
|overwrite|是否覆盖空间中已有的同名文件，默认不覆盖。|Y|
|check_exists|每个文件上传之前是否检查空间中是否存在同名文件，默认为false，不检查|Y|
|check_hash|在`check_exists`设置为`true`的情况下生效，是否检查本地文件hash和空间文件hash一致，默认不检查，节约同步时间|Y|
|check_size|在`check_exists`设置为`true`的情况下，如果`check_hash`为`false`，那么你可以设置`check_size`为`true`做简单的大小检查，以查看本地文件和空间文件大小是否一致，默认不检查|Y|
|skip_file_prefixes|跳过所有文件名（不带相对路径）以该前缀列表里面字符串为前缀的文件|Y|
|skip_path_prefixes|跳过所有文件路径（相对路径）以该前缀列表里面字符串为前缀的文件|Y|
|skip_fixed_strings|跳过所有文件路径（相对路径）中包含该字符串列表中字符串的文件|Y|
|skip_suffixes|跳过所有以该后缀列表里面字符串为后缀的文件或者目录|Y|
|rescan_local|默认情况下，本地新增的文件不会被同步，需要手动设置为true才会去检测新增文件。|Y|
|log_level|上传日志输出级别，可选值为`debug`,`info`,`warn`,`error`,默认`info`|Y|
|log_file|上传日志的输出文件，如果不指定会输出到qshell工作目录下默认的文件中，文件名可以在终端输出看到|Y|
|log_rotate|上传日志文件的切换周期，单位为天，默认为1天即切换到新的上传日志文件|Y|
|log_stdout|上传日志是否同时输出一份到标准终端，默认为false，主要在调试上传功能时可以指定为true|Y|
|file_type|文件存储类型，默认为`0`(标准存储） `1`为低频存储|Y|
|delete_on_success|上传成功的文件，同时删除本地文件，以达到节约磁盘的目的，比如日志归档的场景，默认为`false`，如果需要开启功能，设置为`true`即可。|Y|



	
![人工博客](http://static.gzcx.net/oneblog/20200314112023114.jpg-94rg002)
    

------

> 版权声明：本文为人工博客的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
本文链接：[https://www.94rg.com/article/1751](https://www.94rg.com/article/1751)
