# 银行账户对接

## 支付宝二维码识别登录
- 支付宝扫码登录文档：https://docs.open.alipay.com/65/103570
- 支付宝的扫码登录交互流程：https://docs.open.alipay.com/65/103705/

## 支付宝账单拉取流程交互图：
![](file:///C:/Users/LR/Desktop/1.jpg)
## 支付宝账单下载：
### 工具准备：
- 下载生成RSA秘钥的工具

### 具体步骤：
- 初始化支付宝应用的开发环境，获取app_id、支付宝公钥<br>
  使用沙箱环境进行开发<br>
  生产环境下调用接口需要修改该app_id下的权限（部分功能需要签约，如拉取账单接口需要支付功能）。

  <b>*若干概念解析：*</b>

  AppID:

          应用ID，与之较为混淆的是PID：PID是商户的唯一标识，同一个商户下面可以有多个应用ID。
          即PID和APP_ID是一对多的关系。对账接口调用采用的是应用ID级别的访问，这样可以保证同
          一个商户之间的各个应用相互独立，可以配置不同的密钥对。

  签名：
      密钥：

          应用公钥：由商户自己生成的RSA公钥（与应用私钥必须匹配），商户需上传应用公钥到支付
          宝开放平台，以便支付宝使用该公钥验证该交易是否是商户发起的。

          应用私钥：由商户自己生成的RSA私钥（与应用公钥必须匹配），商户开发者使用应用私钥对请求字符串进行加签。

          支付宝公钥：支付宝的RSA公钥，商户使用该公钥验证该结果是否是支付宝返回的。

      总结：

          开发者利用密钥工具生成私钥和公钥，然后进入支付宝上传公钥换取支付宝公钥。

  对账接入模式：
      收款账号接入模式：

          签约账号即为收款账号，下载接口中指定appid所对应PID下所有交易记录的对账单，目前demo采用的为该种。

      主账号签约接入模式：

          包括ISV接入模式、一个主账号签约+N个收款账号接入模式。



- 本地服务端的私钥配置（java开发需要转成pkcs8格式）等，<br>
  使用支付宝提供的secret_key_tools_RSA_win工具生成配对的秘钥和公钥。<br>
  将rsa_public_key公钥拷贝到支付宝生成支付宝公钥，并将支付宝公钥拷贝回代码ALIPAY_PUBLIC_KEY中。<br>

- 在本地服务器调用接口实现账单业务，https://docs.open.alipay.com/204/106262/

- 商户系统通过HTTP方式后台访问账单下载链接，将账单csv文件下载到本地后自行处理。注意该下载链接仅30秒，<br>
  在得到链接后系统需要立刻请求下载账单文件。超过30秒之后再调用该接口会超时。超时报错见图：
![](file:///C:/Users/LR/Desktop/2.png)<br>
  具体使用JavaCSV操作csv文件，maven依赖：

  ``` xml
  <!-- https://mvnrepository.com/artifact/net.sourceforge.javacsv/javacsv -->
  <dependency>
      <groupId>net.sourceforge.javacsv</groupId>
      <artifactId>javacsv</artifactId>
      <version>2.1</version>
  </dependency>
  ```

- <b>额外要点</b>：

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
### 用途：读取银行U盾、税盘
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
- 方案一：使用软件http://47.75.57.145:7027/files/?tdsourcetag=s_pctim_aiomsg
