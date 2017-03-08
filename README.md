
#支持微信和支付宝 (女神派目前项目使用)

## 引入

### gradle
对应的项目中的build.gradle文件添加依赖：

```xml
dependencies {
    //添加支付库
    compile 'com.github.yeyuqingchen:yc_1.2.1:-SNAPSHOT'
}
```

### 支付宝
```java

        //1.创建支付宝支付配置
        AliPayAPI.Config config = new AliPayAPI.Config.Builder()
                .setRsaPrivate(rsa_private) //设置私钥 (商户私钥，pkcs8格式)
                .setRsaPublic(rsa_public)//设置公钥(// 支付宝公钥)
                .setPartner(partner) //设置商户
                .setSeller(seller) //设置商户收款账号
                .create();

        //2.创建支付宝支付请求
        AliPayReq aliPayReq = new AliPayReq.Builder()
                .with(activity)//Activity实例
                .apply(config)//支付宝支付通用配置
                .setOutTradeNo(outTradeNo)//设置唯一订单号
                .setPrice(price)//设置订单价格
                .setSubject(orderSubject)//设置订单标题
                .setBody(orderBody)//设置订单内容 订单详情
                .setCallbackUrl(callbackUrl)//设置回调地址
                .create()//
                .setOnAliPayListener(null);//

        //3.发送支付宝支付请求
        PayAPI.getInstance().sendPayRequest(aliPayReq);
        
        //关于支付宝支付的回调
        //aliPayReq.setOnAliPayListener(new OnAliPayListener);

```

### 微信
```java
        //1.创建微信支付请求
        WechatPayReq wechatPayReq = new WechatPayReq.Builder()
                .with(this) //activity实例
                .setAppId(appid) //微信支付AppID
                .setPartnerId(partnerid)//微信支付商户号
                .setPrepayId(prepayid)//预支付码
//								.setPackageValue(wechatPayReq.get)//"Sign=WXPay"
                .setNonceStr(noncestr)
                .setTimeStamp(timestamp)//时间戳
                .setSign(sign)//签名
                .create();
        //2.发送微信支付请求
        PayAPI.getInstance().sendPayRequest(wechatPayReq);
        
        
        //关于微信支付的回调
        //wechatPayReq.setOnWechatPayListener(new OnWechatPayListener);
        

```

## 混淆

```xml

#pay_library
-dontwarn io.github.mayubao.pay_library.**
-keep class io.github.mayubao.pay_library.** {*;}

#wechat pay
-dontwarn com.tencent.**
-keep class com.tencent.** {*;}


#alipay
-dontwarn com.alipay.**
-keep class com.alipay.** {*;}

-dontwarn  com.ta.utdid2.**
-keep class com.ta.utdid2.** {*;}

-dontwarn  com.ut.device.**
-keep class com.ut.device.** {*;}

-dontwarn  org.json.alipay.**
-keep class corg.json.alipay.** {*;}
```

