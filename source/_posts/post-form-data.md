---
title: SpringBoot上传文件临时目录
date: 2020-02-25 19:15:20
---

## 背景

今天下午我正在办公室写代码，测试反馈说线上的社区不能发帖了

{% img https://image.hualihai.cn/blog/8557911927c8404fb5385538401b0f22 455 760 %}

{% blockquote %}
Failed to parse multipart servlet request;
nested exception is {% label danger@org.apache.commons.fileupload.FileUploadBase$IOFileUploadException %}:

{% label danger@Processing of multipart/form-data request failed. %}
/tmp/tomcat.829531430751626168.8183/work/Tomcat/localhost/ROOT/upload_dc6bdd3a_1dd6_46b4_9bf6_56785f.tmp(没有那个文件或目录)
{% endblockquote %}

```json
{
    "timestamp": "2020-03-27T01:21:52.672+0000",
    "status": 500,
    "error": "Internal Server Error",
    "message": "Failed to parse multipart servlet request; nested exception is org.apache.commons.fileupload.FileUploadBase$IOFileUploadException: Processing of multipart/form-data request failed. /tmp/tomcat.4814228184880909528.8080/work/Tomcat/localhost/ROOT/upload_7c2eca70_15af_46b3_ae5e_332ea528cd42_00000119.tmp (没有那个文件或目录)",
    "path": "/image/upload/"
}
```

单从异常分析，这是因为发帖的时候上传图片失败导致的，是因为某个文件目录不存在了导致了这个异常。

## 测试环境复现

先登录到测试环境的服务器上,看下{%label info@tmp %}文件夹有这种目录

{% img https://image.hualihai.cn/blog/55cd11bcd3ba457cb7ea0e7ee0cd4c84 %}

进入这个目录,这个目录下什么都没有

```bash
cd /tmp/tomcat.6241131038564227560.8183/work/Tomcat/localhost/ROOT
```

因为{%label info@tmp %}是用来存放临时文件的,所以图片上传的时候会生成一个临时文件,上传完成这个临时文件就被清理了.

我们来验证下

当上传图片的时候一直查看这个目录,会发现这个目录下会生成一个临时文件,上传完成就删掉了.

{% img https://image.hualihai.cn/blog/baf154853cc8470c9d5b4cb080e563c1 %}

## 解决问题

因为这个{%label info@tmp %}文件夹是一个临时文件夹,系统会自动清理.

当这个文件夹被清理掉了,再上传文件时,会在这个临时目录下生成临时文件,但是又找不到这个目录了,所以就报错了.

我们只需要重新指定这个临时目录就可以了,在启动命令加上如下参数`--server.tomcat.basedir=tmp`

```bash
$JAVA_HOME/bin/java -jar community-server.jar --server.tomcat.basedir=tmp
```