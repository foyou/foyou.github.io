---
layout:        post
title:         移动APP调用微信支付
description:   "移动APP调用微信支付"
tags: [WeiXinAPI,WeiXinPay]
image:
  background: body_bg.gif
comments: true
share: true
---

###前言

     配合公司APP,微信里面也要做一套微网站.盯着WeiXinAPI各种看.
     微信JS-SDK地址:<a href="http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html" target="_blank">http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html</a>
     微信支付文档地址:<a href="https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=8_1" target="_blank">https://pay.weixin.qq.com/wiki/doc/api/app.php?chapter=8_1</a>
     微信JS-SDKdemo:<a href="http://203.195.235.76/jssdk/" target="_blank">http://203.195.235.76/jssdk/</a>
	
	
####此demo为C#调用统一下单接口生成支付订单供给移动APP
	
	APP获取的支付订单,统一下单的流程是一样的,只是微信内微网站交易类型trade_type是JSAPI,需要传openid,移动APP调用的话,trade_type是APP.
	
	调用方式:
{% highlight JavaScript %}
	var payment = new Payment("商户号", "appId", "AppKey","/Notify_Url");
    var payInfo = payment.Pay(TotalFee, "唯一订单编号", "订单支付", "127.0.0.1");//TotalFee 总金额，单位：分，不能为空
{% endhighlight%}

    
<!--more-->

{% highlight JavaScript %}
		private string WeiXinPayUrl = "https://api.mch.weixin.qq.com/pay/unifiedorder";
        private string packageStr = "Sign=WXPay";
        /// <summary>
        /// 微信支付商户号（从微信发给你的邮件中获得的）
        /// </summary>
        public string MchId
        {
            get;
            internal set;
        }
        /// <summary>
        /// 应用的APPID（微信发给你的邮件中也有这项内容，一般以wx开头，微信开放平台-管理中心-应用详情也可以看到这项内容）
        /// </summary>
        public string AppId
        {
            get;
            internal set;
        }
        /// <summary>
        /// 这里是API密钥，不是Appsecret，这里最容易出错了！请务必注意！
        /// 设置方法：微信商户平台(pay.weixin.qq.com)-->账户设置-->API安全-->密钥设置
        /// </summary>
        public string ApiKey
        {
            get;
            internal set;
        }

        /// <summary>
        /// 支付成功后，微信会请求这个路径，
        /// </summary>
        public string NotifyUrl
        {
            get;
            internal set;
        }

        /// <summary>
        /// 支付类构造函数，三个关键参数缺一不可，均不能为空
        /// </summary>
        /// <param name="MchId">微信支付商户号（从微信发给你的邮件中获得的）</param>
        /// <param name="AppId">应用的APPID（微信发给你的邮件中也有这项内容，一般以wx开头，微信开放平台-管理中心-应用详情也可以看到这项内容）</param>
        /// <param name="ApiKey">
        /// 这里是API密钥，不是Appsecret，这里最容易出错了！请务必注意！
        /// 设置方法：微信商户平台(pay.weixin.qq.com)-->账户设置-->API安全-->密钥设置
        /// </param>
        public Payment(string MchId, string AppId, string ApiKey, string NotifyUrl)
        {
            this.MchId = MchId;
            this.AppId = AppId;
            this.ApiKey = ApiKey;
            this.NotifyUrl = NotifyUrl;
        }
        
			  /// <summary>
        /// 开发发起支付
        /// </summary>
        /// <param name="TotalFee">总金额，单位：分，不能为空</param>
        /// <param name="TradeNo">订单号，你自己定就好了，不要重复，不能为空</param>
        /// <param name="Des">订单描述，不能为空</param>
        /// <param name="ClientIp">客户端的IP地址，不能为空</param>
        /// <param name="FeeType">货币类型，默认是CNY，人民币</param>
        /// <returns></returns>
        public string Pay(Int64 TotalFee, string TradeNo, string Des, string ClientIp, string FeeType = "CNY")
        {
            //为发送请求给微信服务器准备数据
            var nstr = MakeNonceStr();
            Hashtable packageParameter = new Hashtable();
            packageParameter.Add("appid", this.AppId);
            packageParameter.Add("body", Des);
            packageParameter.Add("mch_id", this.MchId);
            packageParameter.Add("notify_url", this.NotifyUrl);
            packageParameter.Add("nonce_str", nstr);
            packageParameter.Add("out_trade_no", TradeNo);
            packageParameter.Add("total_fee", TotalFee.ToString());
            packageParameter.Add("spbill_create_ip", ClientIp);
            packageParameter.Add("trade_type", "APP");
            packageParameter.Add("fee_type", FeeType);
            var sign = CreateMd5Sign(packageParameter);
            packageParameter.Add("sign", sign);
            var xe = PostDataToWeiXin(packageParameter);
            //为响应客户端的请求准备数据 
            var timeStamp = MakeTimestamp();
            var prepayId = xe.Element("prepay_id").Value;
            nstr = xe.Element("nonce_str").Value;
            Hashtable paySignReqHandler = new Hashtable();
            paySignReqHandler.Add("appid", this.AppId);
            paySignReqHandler.Add("partnerid", this.MchId);
            paySignReqHandler.Add("prepayid", prepayId);
            paySignReqHandler.Add("noncestr", nstr);
            paySignReqHandler.Add("package", packageStr);
            paySignReqHandler.Add("timestamp", timeStamp.ToString());
            var paySign = CreateMd5Sign(paySignReqHandler);
            var obj = new
            {
                appid = this.AppId,
                partnerid = this.MchId,
                prepayid = prepayId,
                package = packageStr,
                noncestr = nstr,
                timestamp = timeStamp,
                sign = paySign
            };
            var serializer = new JavaScriptSerializer();
            var result = serializer.Serialize(obj);
            return result;
    	}
    	
    	
    	return xe;
        }

        private string getXmlStr(Hashtable parameters)
        {
            var sb = new StringBuilder();
            sb.Append("<xml>");
            foreach (string k in parameters.Keys)
            {
                var v = (string)parameters[k];
                if (Regex.IsMatch(v, @"^[0-9.]$"))
                {
                    sb.Append("<" + k + ">" + v + "</" + k + ">");
                }
                else
                {
                    sb.Append("<" + k + "><![CDATA[" + v + "]]></" + k + ">");
                }
            }
            sb.Append("</xml>");
            return sb.ToString();
        }
        private string CreateMd5Sign(Hashtable parameters)
        {
            var sb = new StringBuilder();
            var akeys = new ArrayList(parameters.Keys);
            akeys.Sort();//排序，这是微信要求的
            foreach (string k in akeys)
            {
                var v = (string)parameters[k];
                sb.Append(k + "=" + v + "&");
            }
            sb.Append("key=" + ApiKey);
            string sign = GetMD5(sb.ToString());
            return sign;
        }

        private string MakeNonceStr()
        {
            var timestap = DateTime.Now.ToString("yyyyMMddhhmmssffff");
            return GetMD5(timestap);
        }
        private string GetMD5(string src)
        {
            MD5 md5 = new MD5CryptoServiceProvider();
            byte[] data = Encoding.UTF8.GetBytes(src);
            byte[] md5data = md5.ComputeHash(data);
            md5.Clear();
            var retStr = BitConverter.ToString(md5data);
            retStr = retStr.Replace("-", "").ToUpper();
            return retStr;
        }
{% endhighlight%}

