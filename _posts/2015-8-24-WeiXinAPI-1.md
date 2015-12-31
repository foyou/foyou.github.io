---
layout:        post
title:         微网站结合weixinapi
description:   "微网站结合weixinapi"
tags: [HTML5,WeiXinAPI,WeiXinPay]
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
	
	
####踩过的坑

	微信支付的最后一步，总是提示商户签名错误.试过几次后,发现是微信文档有问题.支付正确格式:
		
	在后台获取prepay_id之后要进行签名，签名的字段是 appId, timeStamp, nonceStr, package, signType。签名之后得到 paySign。在JSSDK去调支付接口的时候(wx.chooseWXPay)，也要上传各个字段.这个时候的字段是 timestamp, nonceStr, package, signType, paySign。一个是timeStamp，一个是timestamp。大小写有问题,还少参数,官方接口说appId要参与签名,上传参数却里面没有传appId.


    
<!--more-->

{% highlight JavaScript %}
	function topay()
  {
      if (typeof WeixinJSBridge == "undefined") {
          if (document.addEventListener) {
                    document.addEventListener('WeixinJSBridgeReady', jsApiCall, false);
          } else if (document.attachEvent) {
                    document.attachEvent('WeixinJSBridgeReady', jsApiCall);
                    document.attachEvent('onWeixinJSBridgeReady', jsApiCall);
          }
      } else {
                jsApiCall();
      }
  }
  function jsApiCall() {
  		//timeStamp等这些内容是后台生成传值过来的,后台生成订单步骤见前文
  		WeixinJSBridge.invoke('getBrandWCPayRequest', {
                        "appId": "wx0f98c5cd802334d1",
                        "timeStamp": timeStamp,
                        "nonceStr": noncestr,
                        "package": packages,
                        "signType": 'MD5',
                        "paySign": paySign
                    }, function (res) {
                        // 支付成功后的回调函数
                        if (res.err_msg == "get_brand_wcpay_request:ok") { 
                        		//写自己的逻辑代码,最好是根据订单号去微信查询一下订单是否支付成功在进行后面的步骤
                        		
                        } else if (res.err_msg == "get_brand_wcpay_request:cancel")  //支付过程中用户取消
                        {
                            //TODO：支付过程中用户取消的商户处理逻辑。
                            alert("支付过程中用户取消");
                            return false;
                        }
                        else if (res.err_msg == "get_brand_wcpay_request:fail")  //支付失败
                        {
                            //alert(res.err_code + res.err_desc + res.err_msg);
                            //TODO：支付失败的商户处理逻辑。
                            alert("支付失败:" + res.err_desc);
                            return false;
                        }
                        else {
                            //alert("交易取消");
                            //alert(res.err_code + res.err_desc + res.err_msg);
                            return false;
                        }

                    });
                }
                else if (data.Status == 0) {
                    alert(data.Msg);
                }
            });
        }
  }
{% endhighlight%}

