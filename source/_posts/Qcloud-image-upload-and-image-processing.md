---
title: 腾讯云上传图片并处理（数据万象）
date: 2019-05-09 16:26:38
tags:
---

#### 1. 先尝试不提供Auth进行上传，OK

```http
POST http://img-1255033804.pic.ap-chengdu.myqcloud.com/filename.jpg

abc123
```

<br>
#### 2. 带Auth后，出现错误。

`Authorization`使用的是[COS签名工具][1]生成的，API密钥使用的两个值填写的是`腾讯云`--`访问管理`--`用户`--`用户列表`--`minip用户`--`API密钥`，权限给了`QcloudCIFullAccess`和`QcloudCOSFullAccess`

```http
POST http://img-1255033804.pic.ap-chengdu.myqcloud.com/auth-test.txt
Authorization: q-sign-algorithm=sha1&q-ak=AKIDviMvO4SZMBtW2RQWvLvMetEZv7KijshS&q-sign-time=1557389209;1557392809&q-key-time=1557389209;1557392809&q-header-list=&q-url-param-list=&q-signature=85dfa83edeedf866770c33633f3d256969af0715

abc123
```

响应：

```xml
<?xml version='1.0' encoding='utf-8' ?>
<Error>
    <Code>QcloudApiRoleNotExist</Code>
    <Message>Qcloud api role not exist, need create role</Message>
    <Resource>img-1255033804.pic.ap-chengdu.myqcloud.com/auth-test.txt</Resource>
    <RequestId>NWNkM2UzMmZfOTA0MDYzNjRfMjMyNl8zY2M=</RequestId>
    <TraceId/>
</Error>
```

<br>
#### 3. 换用`访问密钥`，请求成功

新的`SecretId`和`SecretKey`是从`腾讯云`--`访问管理`--`访问密钥`--`API密钥管理`--`新建密钥`([链接][2])得到的。生成新的`Authorization`后，响应如下。

```xml
<UploadResult>
    <OriginalInfo>
        <Key>auth-test.txt</Key>
        <Location>img-1255033804.cos.ap-chengdu.myqcloud.com/auth-test.txt</Location>
    </OriginalInfo>
</UploadResult>
```

<br>
#### 4. 加入`Pic-Operations`的header之后，加入`pic-operations=...`生成Auth，验证不通过

```xml
<?xml version='1.0' encoding='utf-8' ?>
<Error>
    <Code>SignatureDoesNotMatch</Code>
    <Message>The Signature you specified is invalid</Message>
    <StringToSign>...</StringToSign>
    <Resource>img-1255033804.pic.ap-chengdu.myqcloud.com/pic-op-test.jpg</Resource>
    <RequestId>NWNkM2ZjN2ZfNDQ0NjYzNjRfMzVjMl81MTk=</RequestId>
    <TraceId/>
</Error>
```

<br>
#### 5. 实验得知签名不应该加入`HttpParameters`和`HttpHeaders`参数来生成，清除生成工具中的这两个选填字段

`?image_process` 可以进行`云上数据处理`  

请求：

```http
POST http://img-1255033804.pic.ap-chengdu.myqcloud.com/pic-op-test.jpg?image_process
Authorization:q-sign-algorithm=sha1&q-ak=AKIDMauCpHF4Zr360jrbs5c2LYotiL065zbj&q-sign-time=1557394879;1557398479&q-key-time=1557394879;1557398479&q-header-list=&q-url-param-list=as&q-signature=3a4482c12cb6ef86c92e853f0ecbf9e4a60562b5
Pic-Operations:{"is_pic_info":1,"rules":[{"fileid":"mytest.png","rule":"imageView2/format/png"}]}
```

响应:

```xml
<UploadResult>
    <OriginalInfo>
        <Key>pic-op-test.jpg</Key>
        <Location>img-1255033804.cos.ap-chengdu.myqcloud.com/pic-op-test.jpg</Location>
        <ImageInfo>
            <Format>JPEG</Format>
            <Width>1920</Width>
            <Height>1080</Height>
            <Quality>90</Quality>
            <Ave>0x473a2e</Ave>
            <Orientation>0</Orientation>
        </ImageInfo>
    </OriginalInfo>
    <ProcessResults>
        <Object>
            <Key>mytest.png</Key>
            <Location>img-1255033804.cos.ap-chengdu.myqcloud.com/mytest.png</Location>
            <Format>png</Format>
            <Width>1920</Width>
            <Height>1080</Height>
            <Size>2383168</Size>
            <Quality>90</Quality>
        </Object>
    </ProcessResults>
</UploadResult>
```

<br>
#### 6. 放在一个请求中，同时上传和处理


请求：

```http
PUT http://img-1255033804.pic.ap-chengdu.myqcloud.com/pic-one.jpg
Authorization: q-sign-algorithm=sha1&q-ak=AKIDMauCpHF4Zr360jrbs5c2LYotiL065zbj&q-sign-time=1557394879;1557398479&q-key-time=1557394879;1557398479&q-header-list=&q-url-param-list=&q-signature=652f539a23fedd67a97fa7dfd909de1aea6cce8f
Pic-Operations: {"is_pic_info":1,"rules":[{"fileid":"pic_one.240x180.jpg","rule":"imageView2/1/w/240/h/180/q/85"}]}
```

响应:

```xml
<UploadResult>
    <OriginalInfo>
        <Key>pic-one.jpg</Key>
        <Location>img-1255033804.cos.ap-chengdu.myqcloud.com/pic-one.jpg</Location>
        <ImageInfo>
            <Format>JPEG</Format>
            <Width>1920</Width>
            <Height>1080</Height>
            <Quality>90</Quality>
            <Ave>0x473a2e</Ave>
            <Orientation>0</Orientation>
        </ImageInfo>
    </OriginalInfo>
    <ProcessResults>
        <Object>
            <Key>pic_one.240x180.jpg</Key>
            <Location>img-1255033804.cos.ap-chengdu.myqcloud.com/pic_one.240x180.jpg</Location>
            <Format>JPEG</Format>
            <Width>240</Width>
            <Height>180</Height>
            <Size>12081</Size>
            <Quality>85</Quality>
        </Object>
    </ProcessResults>
</UploadResult>
```



[1]: https://cos5.cloud.tencent.com/static/cos-sign/
[2]: https://console.cloud.tencent.com/cam/capi
