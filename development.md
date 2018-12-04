# 银行账户对接

## 支付宝二维码识别登录
- 支付宝扫码登录文档：https://docs.open.alipay.com/65/103570
- 支付宝的扫码登录交互流程：https://docs.open.alipay.com/65/103705/

## 支付宝账单拉取流程交互图：
![](file:///C:/Users/LR/Desktop/1.jpg)
## 支付宝账单拉取开发步骤：
- 初始化支付宝应用的开发环境，获取app_id、公钥等信息，并且等待审核完成。审核完成后，转入使用沙箱环境进行开发。生产环境下调用接口可能会有异常情况，需要修改支付宝的权限。（只有添加对应的功能才能调用该功能下对应的接口，部分功能需要签约）。

- 本地服务端的私钥配置（java开发需要转成pkcs8格式）等，使用支付宝提供的secret_key_tools_RSA_win工具生成配对的秘钥和公钥。将rsa_public_key公钥拷贝到支付宝生成支付宝公钥，并将支付宝公钥拷贝回代码ALIPAY_PUBLIC_KEY中。

- 在本地服务器调用接口实现账单业务，https://docs.open.alipay.com/204/106262/

- 商户系统通过HTTP方式后台访问账单下载链接，将账单csv文件下载到本地后自行处理。注意该下载链接仅30秒，在得到链接后系统需要立刻请求下载账单文件。超过30秒之后再调用该接口会超时。超时报错见图：
![](file:///C:/Users/LR/Desktop/2.png)
具体使用JavaCSV操作csv文件，maven依赖：

``` xml
<!-- https://mvnrepository.com/artifact/net.sourceforge.javacsv/javacsv -->
<dependency>
    <groupId>net.sourceforge.javacsv</groupId>
    <artifactId>javacsv</artifactId>
    <version>2.1</version>
</dependency>
```

- 补充要点：
Csv文件格式与excel文件格式的对比参考：https://blog.csdn.net/weixin_39198406/article/details/78705016
Javacsvapi文档及示例代码：https://www.csvreader.com/java_csv_samples.php

## 微信二维码识别登录
### 快捷登录总体流程
- 创建网站应用
通过填写网站名称、简介和图标、以及官网地址等信息，开发者可以创建网站应用
- 提交审核
开发者提交网站应用创建申请后，微信团队将对网站应用信息进行审核，确保网站应用质量
- 审核通过上线
审核通过后，开发者得到<b>AppID</b>，可通过AppID进行微信登录等功能的开发

### 快捷登录总体设计思路
- 内部标识：系统中用于标识用户唯一性的标志，当内部标识建立后，用户所有的数据资产都会绑定到该标识上。建议系统内部使用user_id（唯一且不可更改）标识用户。
- 外部标识：用来标识用户身份，可以是用户名、手机号、邮箱，外部标识随时会更新，如手机号

## 微信账单拉取：
## 微信账单拉取开发步骤：
### XXX
### 下载账单
- 下载对账单https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=9_6
- 下载资金账单https://pay.weixin.qq.com/wiki/doc/api/native.php?chapter=9_18&index=7

## 各大银行账户对接：
https://www.cnblogs.com/ityouknow/p/7015879.html
## 需要研究的技术点 ：
- ftp服务器

- 对账单的下载服务
- 登录网银后台手动下载对账单

## USB分散器
- 方案一：使用http://usb4java.org/quickstart/libusb.html api<br>

  1.初始化LibUsb<br>

  2.根据short vendorId, short productId找到特定的设备<br>

      idVendor:供应商id（2Bytes);
      
      idProduct:产品id(2Bytes);

      iSerialNumber:序列号(1Byte)；

      正规的优盾设备制造商应该持有the USB specification committee签发的供应商id

      该网址可以查询供应商id:http://www.linux-usb.org/usb.ids

  3.建立通讯需要的device handle<br>

  4.读取usb设备内存储的信息

- 方案二：

## 税盘
