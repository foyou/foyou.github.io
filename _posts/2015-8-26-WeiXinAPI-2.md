---
layout:        post
title:         WeiXinAPI获取用户信息
description:   "WeiXinAPI获取用户信息"
tags: [WeiXinAPI]
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
	
	在关注者与公众号产生消息交互后，公众号可获得关注者的OpenID（加密后的微信号，每个用户对每个公众号的OpenID是唯一的。对于不同公众号，同一用户的openid不同）。公众号可通过本接口来根据OpenID获取用户基本信息，包括昵称、头像、性别、所在城市、语言和关注时间。
	
	我们可以看看官方的文档：<a href="http://mp.weixin.qq.com/wiki/14/bb5031008f1494a59c6f71fa0f319c66.html">获取用户的基本信息</a>。
	    
<!--more-->

####接口调用请求说明  

http请求方式: GET 
https://api.weixin.qq.com/cgi-bin/user/info?access_token=ACCESS_TOKEN&openid=OPENID&lang=zh_CN 

这里主要说通过网页授权获取用户基本资料，官方文档：<a href="http://mp.weixin.qq.com/wiki/17/c0f37d5704f0b64713d5d2c37b468d75.html">网页授权获取用户基本信息</a>
 
{% highlight JavaScript %}
	 <a class="btn_wx" href="https://open.weixin.qq.com/connect/oauth2/authorize?appid=你的appid&redirect_uri=信息跳转页面&response_type=code&scope=snsapi_userinfo&state=weixinscope#wechat_redirect">微信登录</a>
{% endhighlight%}

接收返回信息页面：
{% highlight JavaScript %}
[AllowAnonymous]
        public ActionResult Oauth_Response(string code, string state)
        {
            string logtxt = "";//自定义log记录，方便调试
            string url = "";
            string ret_wx_oauth = "";
            string turl = "";
            string ret_userinfo = "";
            if (!string.IsNullOrWhiteSpace(code))
            {
                //第二步 根据获取的code获取openid和token
                url = "https://api.weixin.qq.com/sns/oauth2/access_token?appid=xxx&secret=xxxx&code=" + code + "&grant_type=authorization_code";
                ret_wx_oauth = GetJsonStr(url);
                JsonData wx_oauth = JsonMapper.ToObject(ret_wx_oauth);
                var err = "";
                try
                {
                    if (!string.IsNullOrWhiteSpace(wx_oauth["openid"].ToString()))
                    {
                       //根据获取的openid判断数据库是否存在该openid

                        if (authmodel == null)
                        {
                            //如果该openid记录没找到，则保存微信OPenID等信息
                            Add(authmodel);

                            //第三步 根据token和openid拉取用户信息保存到用户表并登录
                            turl = "https://api.weixin.qq.com/sns/userinfo?access_token=" + wx_oauth["access_token"].ToString() + "&openid=" + wx_oauth["openid"].ToString() + "&lang=zh_CN";
                            ret_userinfo = GetJsonStr(turl);
                            JsonData wx_userinfo = JsonMapper.ToObject(ret_userinfo);

                            if (!string.IsNullOrWhiteSpace(wx_userinfo["nickname"].ToString()))
                            {
                               //保存获取到的基本信息到用户信息表并绑定authmodel
															 Add(usermodel); 
                            }
                            else
                            {                            		
                                return RedirectToAction("Login", "WeiXinApp");
                            }
                        }
                        else
                        {
	                           //存在记录直接获取用户信息跳转登录成功
	                           return RedirectToAction("Index", "WeiXinApp");
                        }
                    }
                    else
                    {

                    }
                }
                catch (Exception ex)
                {
                    try
                    {
                        string filePath = HttpContext.Server.MapPath("\\WeiXinLog\\log.txt");
                        string dir = Path.GetDirectoryName(filePath);
                        if (!Directory.Exists(dir))
                        {
                            Directory.CreateDirectory(dir);
                        }
                        using (StreamWriter sw = System.IO.File.AppendText(filePath))
                        {
                            sw.WriteLine(" 微信登录报错:" + DateTime.Now.ToLongTimeString() + " 返回值:" + err + "|||" + ex.Message.ToString());
                            sw.WriteLine("");
                            sw.Close();
                        }
                    }
                    catch
                    {
                    }
                } 
            }
            return RedirectToAction("Login", "WeiXinApp");
        }
{% endhighlight%}



