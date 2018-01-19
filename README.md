##聚合收银台技术对接文档
* [概览](#%E6%A6%82%E8%A7%88)
* [使用效果](#%E4%BD%BF%E7%94%A8%E6%95%88%E6%9E%9C)
* [适用范围](#%E9%80%82%E7%94%A8%E8%8C%83%E5%9B%B4)
* [使用前准备](#%E4%BD%BF%E7%94%A8%E5%89%8D%E5%87%86%E5%A4%87)
* [主要流程](#%E4%B8%BB%E8%A6%81%E6%B5%81%E7%A8%8B)
* [嵌入jssdk](#%E5%B5%8C%E5%85%A5jssdk)
    * [参数说明](#%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E)
    * [嵌入示例](#%E5%B5%8C%E5%85%A5%E7%A4%BA%E4%BE%8B)
* [接口说明](#%E6%8E%A5%E5%8F%A3%E8%AF%B4%E6%98%8E)
    * [Paymax\.charge方法定义](#%E6%96%B9%E6%B3%95%E5%AE%9A%E4%B9%89)
    * [Paymax\.charge参数说明](#%E5%8F%82%E6%95%B0%E8%AF%B4%E6%98%8E1)
    * [Paymax\.charge调用示例](#%E8%B0%83%E7%94%A8%E7%A4%BA%E4%BE%8B)
* [签名](#%E7%AD%BE%E5%90%8D)
    * [参与签名的字段及顺序](#%E5%8F%82%E4%B8%8E%E7%AD%BE%E5%90%8D%E7%9A%84%E5%AD%97%E6%AE%B5%E5%8F%8A%E9%A1%BA%E5%BA%8F)
    * [签名步骤](#%E7%AD%BE%E5%90%8D%E6%AD%A5%E9%AA%A4)
* [请求数据对象](#%E8%AF%B7%E6%B1%82%E6%95%B0%E6%8D%AE%E5%AF%B9%E8%B1%A1)
    * [支付渠道编码](#%E6%94%AF%E4%BB%98%E6%B8%A0%E9%81%93%E7%BC%96%E7%A0%81)
    * [支付渠道附加参数](#%E6%94%AF%E4%BB%98%E6%B8%A0%E9%81%93%E9%99%84%E5%8A%A0%E5%8F%82%E6%95%B0)
* [响应错误码](#%E5%93%8D%E5%BA%94%E9%94%99%E8%AF%AF%E7%A0%81)
    * [参数错误提示详情](#%E5%8F%82%E6%95%B0%E9%94%99%E8%AF%AF%E6%8F%90%E7%A4%BA%E8%AF%A6%E6%83%85)

###<span id="概览">概览</span>
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;聚合收银台，是 Paymax 推出的一个轻量级产品，只需一行代码即可完成支付，商户可自定义支付渠道和显示顺序，兼容 PC 端页面和移动 H5 浏览器。不但降低了开发成本，同时优化了用户的支付体验。

###<span id="使用效果">使用效果</span>&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;点击立即支付，网页上出现支付渠道选择菜单，点击其中渠道跳转到指定渠道的支付页面，PC端和移动H5端效果分别如下图：
PC端如下：![pc_charge](https://raw.githubusercontent.com/paymax/paymax-doc/master/image/pc_charge.png)![pc_channel](https://raw.githubusercontent.com/paymax/paymax-doc/master/image/pc_channel.png)![pc_cashier](https://raw.githubusercontent.com/paymax/paymax-doc/master/image/pc_cashier.png)
移动H5端如下：
![h5](https://raw.githubusercontent.com/paymax/paymax-doc/master/image/h5.png)


###<span id="适用范围">适用范围</span>1.	聚合收银台目前支持的支付渠道有：
	**PC端：**
		*	拉卡拉 PC 快捷支付	*	拉卡拉 PC 网关支付	*	微信公众号（C2B扫码）支付
	* 	支付宝扫码	*	支付宝即时到账	**移动H5端：**   
	*	拉卡拉H5支付	*	微信公众号支付（仅在微信浏览器中展示）2.	不需要自己写样式；3.	不需要自己做移动端适配；4.	如果只有一个支付渠道，可以直接跳转付款页进行支付；5.	可以自定义展示支付渠道以及展示顺序。
（注：不支持定制收银台页面UI）

###<span id="使用前准备">使用前准备</span>1.	注册Paymax账号，并完成验证公司信息；2.	创建应用，并申请或配置支付渠道参数；3.	联系客服（95016）或销售，开通聚合收银台功能；4.	对接联调测试。
###<span id="主要流程">主要流程</span>
1.	在页面上嵌入 Paymax jssdk,见[嵌入jssdk](#嵌入jssdk)；
2.	在页面触发支付时调用jssdk中的 Paymax.charge(data,event)方法,见[接口说明](#接口说明)。
![flow](flow.png)

###<span id="嵌入jssdk">嵌入jssdk</span>
####<span id="参数说明">参数说明</span>
| 变量名 | 类型  | 必填  | 描述    |
|----------|-------|-----|------------|
| config    | String  | 是   | 配置参数,不能有空格,分两种方式，见[配置参数方式说明](#配置参数方式说明) |

####<span id="嵌入示例">嵌入示例</span>
若为移动端H5页面，页面头部需加上
```
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
``` 做移动适配。

*	**通用方式**
```
<script type="text/javascript" src="https://www.paymax.cc/merchant-api/js/config?config=appid"></script>
```
*	**自定义方式**
```
<script type="text/javascript" src="https://www.paymax.cc/merchant-api/js/config?config=appid1|channelcode1,appid2|channelcode2,appid1|channelcode3"></script>
```

#####<span id="配置参数方式说明">配置参数方式说明</span>
| 方式 | 描述 | 示例 | 渠道显示顺序 |
|----------|-------|-------|-------|
| appid | 应用的App ID，只能传一个,从 Paymax 商户后台的应用信息页面获取 | app_Tt639X1Lp0Db240N | 默认顺序，见[默认顺序](#默认顺序) |
| appid1|channelcode1,appid2|channelcode2,appid1|channelcode3 | 传一组应用id和支付渠道编码；以"|"拼接应用id和支付渠道编码；以","拼接多组appid|channelcode;其中"|"、","是英文状态下的符号;支付渠道编码见[支付渠道编码](#支付渠道编码) | app\_Tt639X1Lp0Db240N|wechat\_csb,app\_Tt639X1Lp0DuEds3|lakala\_web, app\_Tt639X1Lp0Db240N|lakala\_h5 | 以传入的支付渠道编码的顺序显示，如示例中的渠道显示顺序是：wechat\_csb，lakala\_web，lakala\_h5 |

*其中appid和channelcode需要替换成商户的真实数据。*

**示例**
	某商户在 Paymax 创建的应用APP ID为app_Tt639X1Lp0Db240N,该应用开通了拉卡拉 PC 快捷支付、拉卡拉PC网关支付两个渠道,则config参数值可以是
	
	通用方式:config=app_Tt639X1Lp0Db240N
	自定义方式1:config=app_Tt639X1Lp0Db240N|lakala_web
	自定义方式2:config=app_Tt639X1Lp0Db240N|lakala_web_fast
	自定义方式3:config=app_Tt639X1Lp0Db240N|lakala_web_fast,app_Tt639X1Lp0Db240N|lakala_web
	自定义方式4:config=app_Tt639X1Lp0Db240N|lakala_web,app_Tt639X1Lp0Db240N|lakala_web_fast

#####<span id="默认顺序">默认顺序</span>
######PC网页支付	1.	拉卡拉 PC 快捷支付 	2.	拉卡拉 PC 网关支付	3.	微信公众号（C2B扫码）支付
	4. 	支付宝扫码	5.	支付宝即时到账######移动网页支付	1.	拉卡拉 H5 支付 	2.	微信公众号支付 (仅在微信浏览器中展示)

######渠道显示顺序示例
config如下所示

```
config=app_Tt639X1Lp0Db240N|lakala_web_fast,app_Tt639X1Lp0Db240N|alipay_web,app_Tt639X1Lp0Db240N|lakala_web,app_Tt639X1Lp0Db240N|wechat_wap
```
在页面上显示的渠道顺序如下所示：
![channel_sort](channel_sort.png)

###<span id="接口说明">接口说明</span>
####<span id="方法定义">Paymax.charge方法定义</span>
```
Paymax.charge(data,event)
```
####<span id="参数说明1">Paymax.charge参数说明</span>

| 变量名 | 类型  | 必填  | 来源 | 描述 |
|----------|-------|-----|-----|-------|
| data | Object | 是 | 商户服务器端 | 商户在服务器端准备好data数据后,传递给前端页面;data结构见[data对象结构](#data对象结构),示例见[data对象示例](#data对象示例) |
| event | Object | 否 | jssdk内置 | Paymax jssdk内部定义的事件,event结构见[event对象结构](#event对象结构) |

#####<span id="data对象结构">data对象结构</span>
	
| 变量名 | 类型  | 必填  | 来源 | 描述 | 示例 |
|----------|-------|-----|-----|-------|-------|
| debug | boolean | 否 | 商户传入 | 调试信息开关,开启后将alert一些提示信；默认是关闭状态:false | true |
| Authorization | String | 是 | 商户服务器端 | 商户secret key,Paymax 提供给商户的唯一标识,从Paymax商户后台的个人中心获取 | 5b97b3138041437587646b37f52dc7f7  |
| nonce | String | 是 |	商户服务器端 | 8~128位的随机字符串,由字母数字组成,不允许特殊字符 | 7650d33c9b6f4e8a8025465061937376 |
| timestamp | String | 是 | 商户服务器端 | 13位的时间戳,标准北京时间，时区为东八区，自1970年1月1日 0点0分0秒 以来的毫秒数 | 1466404370089  |
| sign | String | 是 | 商户服务器端 | 签名后的值,需要在商户服务器端处理,签名方法见[签名](#签名) | BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA= |
| request_data | Object | 是 | 商户服务器端 | 请求数据对象,JSON格式，见[请求数据对象](#请求数据对象) | {"amount":1,"body":"Your Body","description":"description","subject":"Your Subject","client\_ip":"127.0.0.1","order\_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363","extra":{ "return\_url":"http://dev.paymax.cc/",   "user\_id":"40","show\_url":"http://dev.paymax.cc/123"},"metadata":{"metadata\_key1":"metadata\_value1","metadata\_key2":"metadata\_value2"},"time\_expire":1486369350829,"currency":"CNY"}|


#####<span id="data对象示例">data对象示例</span>

```json
{
    "debug":true,
    "Authorization":"5b97b3138041437587646b37f52dc7f7",
    "nonce":"7650d33c9b6f4e8a8025465061937376",
    "timestamp":"1466404370089",
    "sign":"BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA=",
    "request_data":{
        "amount":1,
        "body":"Your Body",
        "description":"description",
        "subject":"Your Subject",
        "client_ip":"127.0.0.1",
        "order_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363",
        "extra":{
            "return_url":"http://dev.paymax.cc/",
            "user_id":"40",
            "show_url":"http://dev.paymax.cc/123"
        },
        "metadata":{
            "metadata_key1":"metadata_value1",
            "metadata_key2":"metadata_value2"
        },
        "time_expire":1486369350829,
        "currency":"CNY"
    }
}
```

#####<span id="event对象结构">event对象结构</span>
1.	只有在支付授权目录下支付时,微信才会调用jsapi中注册的函数；
2.	测试授权目录下的支付不会触发wxJsapiFinish等事件；
3.	有关微信jsapi的返回结果res,请参考 [https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_7](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_7)。

| 变量名 | 类型  | 必填  | 描述    |
|----------|-------|-----|------------|
| wxJsApiFinish    | function(response) | 否 |  微信jsapi的支付接口调用完成后将调用此接口; 只传递一个参数,为微信原生的结果Object |
| wxJsApiSuccess   | function(response) | 否 | 微信jsapi的接口支付成功后将调用此接口; 只传递一个参数,为微信原生的结果Object |
| wxJsApiFail      | function(response) | 否 | 微信jsapi的接口支付非成功都将调用此接口; 只传递一个参数,为微信原生的结果Object |
| dataError   | function(msg) | 否 | 数据获取出错,将调用此接口; 只传递一个参数为Object,其中有错误描述 |

####<span id="调用示例">Paymax.charge调用示例</span>

```json
Paymax.charge(
	{
	    "debug":true,
	    "Authorization":"5b97b3138041437587646b37f52dc7f7",
	    "nonce":"7650d33c9b6f4e8a8025465061937376",
	    "timestamp":"1466404370089",
	    "sign":"BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA=",
	    "request_data":{
	        "amount":1,
	        "body":"Your Body",
	        "description":"description",
	        "subject":"Your Subject",
	        "client_ip":"127.0.0.1",
	        "order_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363",
	        "extra":{
	            "return_url":"http://dev.paymax.cc/",
	            "user_id":"40",
	            "show_url":"http://dev.paymax.cc/123"
	        },
	        "metadata":{
	            "metadata_key1":"metadata_value1",
	            "metadata_key2":"metadata_value2"
	        },
	        "time_expire":1486369350829,
	        "currency":"CNY"
	    }
	},{
		wxJsApiFinish: function(response) {},
	    wxJsApiSuccess: function(response) {},
	    wxJsApiFail: function(response) {},
	    dataError: function(msg) {}
	}
)
```
Paymax.charge调用成功后，跳转到支付页面；
Paymax.charge调用失败后，返回相关错误信息，见[响应错误码](#响应错误码)；详细错误信息请在浏览器的控制台查看。

###<span id="签名">签名</span>
1. 采用RSA签名;
2. 商户在调用 Paymax.charge方法前,需要使用商户的私钥对请求数据进行签名;
3. 商户在Paymax网站设置商户公钥,Paymax 使用该公钥验签商户请求。

####<span id="参与签名的字段及顺序">参与签名的字段及顺序</span>

| 顺序   | 参数            | 描述                             | 示例                                       |
| ---- | ---------- | ---------- | ----------- |
| 1    | method | 请求方法,小写(post) | post                                     |
| 2    | uri | 统一资源标识符[API方法]                 | /js/charges                              |
| 3   | nonce | 8~128位的随机字符串,由字母数字组成,不允许特殊字符  | 049e73d6e0744a7491cda2a0a537b90d         |
| 4   | timestamp | 13位的时间戳,标准北京时间，时区为东八区，自1970年1月1日 0点0分0秒 以来的毫秒数  | 1466399895704                            |
| 5  | Authorization | 商户secret key,Paymax 提供给商户的唯一标识,从Paymax商户后台的个人中心获取 | 5b97b3138041437587646b37f52dc7f7         |
| 6  | request_data | 请求数据对象,JSON格式，见[请求数据对象](#请求数据对象)  | {"amount":1,"body":"Your Body","description":"description","subject":"Your Subject","client\_ip":"127.0.0.1","order\_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363","extra":{ "return\_url":"http://dev.paymax.cc/",   "user\_id":"40","show\_url":"http://dev.paymax.cc/123"},"metadata":{"metadata\_key1":"metadata\_value1","metadata\_key2":"metadata\_value2"},"time\_expire":1486369350829,"currency":"CNY"} |

####<span id="签名步骤">签名步骤</span>
1. 签名前以 换行符（"\n"） 拼接各个字段:

   ```
method+"\n"+uri+"\n"+nonce+"\n"+timestamp+"\n"+Authorization+"\n"+request_data
   ```
   **不同的语言,换行符可能有所不同**
   
   组装成要签名的数据,例如:
   
   ```json
   post
   /js/charges
   7650d33c9b6f4e8a8025465061937376
   1466404370089
   5b97b3138041437587646b37f52dc7f7
   {"amount":1,"body":"Your Body","description":"description","subject":"Your Subject","client_ip":"127.0.0.1","order_no":"d0a4877e-39fa-4704-a3e1-ca484a7f1363","extra":{ "return_url":"http://dev.paymax.cc/",   "user_id":"40","show_url":"http://dev.paymax.cc/123"},"metadata":{"metadata_key1":"metadata_value1","metadata_key2":"metadata_value2"},"time_expire":1486369350829,"currency":"CNY"}
   ```

2. 以UTF-8编码将待签名字符串转换成字节数组,然后使用**商户私钥**对其签名,签名算法为SHA1WithRSA,签名后的值为sign。

   ```
BkBa8OLkU2KzRIrWA4swP5WCSouVwXHFAUM6NlJlgDGbMQYKKvVveE30pXppPcesRLPPlpTctFdCt+nY4czX79efg19FDEizq94d+HAsE/3dtzUMiiFyxOWFH50FGO+gJZDAEpYQRCdJlOs6D380ta+y0wRfUhlfgX1+RXtcDnA=
   ```

###<span id="请求数据对象">请求数据对象</span>

| 参数          | 类型      | 必须    | 描述                                       |
| :---------- | ------- | ----- | ---------------------------------------- |
| order_no    | String  | true  | 商户订单号,在商户系统内唯一,8-20位数字或字母,不允许特殊字符        |
| amount      | Decimal | true  | 订单总金额,大于0的数字,单位是该币种的货币单位                 |
| subject     | String  | true  | 购买商品的标题,最长32位                            |
| body        | String  | true  | 购买商品的描述信息,最长128个字符                       |
| client_ip   | String  | true  | 发起支付的客户端IP                               |
| description | String  | false | 订单备注,限制300个字符内                           |
| time_expire | Long    | false | 订单失效时间,13位Unix时间戳,默认1小时,微信公众号支付对其的限制为3分钟 |
| currency    | String  | false | 三位ISO货币代码,只支持人民币cny,默认cny                |
| extra       | Map     | true | 特定渠道需要的的额外附加参数,参考[支付渠道附加参数](#支付渠道附加参数)   |
| metadata    | Map     | false | 用户自定义元数据                                 |


####<span id="支付渠道编码">支付渠道编码</span>
| 渠道             | 编码              |
| :------------- | :-------------- |
| 微信公众号          | wechat_wap      |
| 微信公众号（C2B扫码）支付 | wechat_csb      |
| 支付宝扫码 | alipay_csb |
| 支付宝即时到账        | alipay_web      |
| 拉卡拉 PC 网关支付    | lakala_web      |
| 拉卡拉 PC 快捷支付    | lakala\_web_fast |
| 拉卡拉 H5 支付      | lakala_h5       |

####<span id="支付渠道附加参数">支付渠道附加参数</span>

##### 微信公众号

open_id：必填，用户在公众号下的唯一标识。

**示例**：

```json
{"open_id":"obc-jswk25IUGp3q8RPTYu083rmk"}
```
#####微信公众号（C2B扫码）支付
无

#####支付宝扫码
无

##### 支付宝即时到账

* return_url：必填，支付完成后回跳地址；
* show_url：非必填，商品在支付宝支付页面的展示“链接地址”。

```json
{
  "show_url":"http://www.abc.cn/charge",
  "return_url":"http://www.abc.cn/"
}
```

##### 拉卡拉 PC 网关支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回跳地址。

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/"
}
```

##### 拉卡拉 PC 快捷支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回跳地址。

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/"
}
```

##### 拉卡拉 H5 支付

* user_id: 必填，用户在商户系统中的唯一标识，请使用纯数字格式；
* return_url: 必填，支付完成后的回跳地址；
* show_url: 必填，支付界面的返回按钮跳转的地址。

```json
{
  "user_id":"123",
  "return_url":"http://www.abc.cn/",
  "show_url":"http://www.abc.cn/charge"
}
```

*如果开通了多个渠道，附加参数取合集*

**示例一：只开通了支付宝即时到账：**

```json
{
  "show_url":"http://www.abc.cn/charge",
  "return_url":"http://www.abc.cn/"
}
```

**示例二：开通了支付宝即时到账、微信公众号、拉卡拉 H5 支付三个渠道：**

```json
{
  "open_id":"obc-jswk25IUGp3q8RPTYu083rmk",
  "user_id":"123",
  "show_url":"http://www.abc.cn/charge",
  "return_url":"http://www.abc.cn/"
}
```

###<span id="响应错误码">响应错误码</span>

当调用Paymax.charge接口失败时，会返回一个json对象，标明失败的原因和具体描述。

例如：

```json
{
  "failure_code": "SECRET_KEY_IS_BLANK",
  "failure_msg": "SecretKey为空"
}
```

* failure\_code是对失败原因的唯一标识，failure\_msg是对失败原因的进一步的解释和描述；


* 对于同一种failure\_code，可能会有多种failure\_msg。



**详细的错误代码表及示例：**


| 错误码(failure_code)          | 描述示例(failure_msg) | 说明                                       |
| -------------------------- | ----------------- | ---------------------------------------- |
| SECRET\_KEY\_IS\_BLANK        | SecretKey为空       |                                          |
| SECRET\_KEY\_IS\_INVALID      | 非法的SecretKey      |                                          |
| SIGN\_CHECK\_FAILED          | 签名校验失败            | 签名校验的原因                                  |
| APP\_NOT\_EXISTED            | 应用不存在             |                                          |
| ORDER\_NO\_NOT\_EXIST         | 订单不存在             |                                          |
| CHANNEL\_NOT\_AVAILABLE      | 该支付渠道未申请或者未开通     |                                          |
| ORDER\_NO\_DUPLICATE         | 商户订单号已存在          | 商户使用相同的订单号重复下单                           |
| ILLEGAL\_ARGUMENT           | 参数错误              | 调用接口的参数错误 ;详见[参数错误提示详情](#参数错误提示详情)       |
| ILLEGAL\_CHANNEL\_PARAMETERS | 支付渠道参数配置有误        |                                          |
| CHANNEL\_CHARGE\_FAILED      | 向支付渠道下单失败         | Paymax向支付渠道下单时失败，会在failure_msg中显示失败原因    |
| CHANNEL\_FREEZE             | 该支付渠道已经被冻结交易      |                                          |
| SYSTEM\_ERROR               | 系统内部错误            | 由于某些原因造成系统处理异常，请联系Paymax处理               |

####<span id="参数错误提示详情">参数错误提示详情</span>
**发起支付（下单）公共参数错误提示**

| 字段        | 错误原因         | 错误提示                       |
| --------- | ------------ | -------------------------- |
| order_no  | 为空           | order_no 参数格式不正确！          |
| amount    | 为空           | amount 不能为空                |
| amount    | 小于等于0        | amount 参数格式不正确,请使用大于0的数字！  |
| subject   | 为空           | subject 不能为空               |
| subject   | 长度超过32位      | subject 参数格式不正确,长度不能超过32位！ |
| body      | 为空           | body 参数格式不正确！              |
| client_ip | 为空或IP地址格式不正确 | client ip 参数格式不正确！         |

**各支付渠道附加参数（extra）错误提示**

| 支付渠道类型       | 附加参数字段     | 错误原因 | 错误提示             |
| ------------ | ---------- | ---- | ---------------- |
| 微信公众号        | open_id    | 为空   | 缺少附加参数open_id    |
| 支付宝即时到账      | return_url | 为空   | 缺少附加参数return_url |
| 拉卡拉 PC 网关支付  | user_id    | 为空   | 缺少附加参数user_id    |
| 拉卡拉 PC 网关支付  | return_url | 为空   | 缺少附加参数return_url |
| 拉卡拉 PC 快捷支付  | user_id    | 为空   | 缺少附加参数user_id    |
| 拉卡拉 PC 快捷支付  | return_url | 为空   | 缺少附加参数return_url |
| 拉卡拉 H5 支付    | user_id    | 为空   | 缺少附加参数user_id    |
| 拉卡拉 H5 支付    | return_url | 为空   | 缺少附加参数return_url |
| 拉卡拉 H5 支付    | show_url   | 为空   | 缺少附加参数show_url   |
