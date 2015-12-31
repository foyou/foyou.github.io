---
layout:        post
title:         微信支付统一下单接口生成微信内支付订单
description:   "微信支付统一下单接口生成微信内支付订单"
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
	
	
####此demo为C#调用统一下单接口生成微信内支付订单
	
	调用方式:
{% highlight JavaScript %}
	GetCreatPay(TotalFee, "唯一订单编号", 自用产品ID, 自用用户ID);//TotalFee 总金额，单位：分，不能为空 
{% endhighlight%}

    
<!--more-->

{% highlight JavaScript %}
				public object GetCreatPay(string _price, string _poid, int _tsid, int _uid)
        {
            string prepay_id = "";
            string request_data = "";
            var timestamp = TenpayUtil.getTimestamp();
            UnifiedOrder order = new UnifiedOrder();
            TenpayUtil tenpay = new TenpayUtil();
            string nonce_str = TenpayUtil.getNoncestr();
            string return_string = string.Empty;
            try
            {
                Model.PO_Main model = BLL.PO_Main.Instance.GetModel(_poid);
                if (!string.IsNullOrWhiteSpace(model.TP_ID))
                {
                    prepay_id = model.TP_ID;
                }
            }
            catch
            {
            }
            if (string.IsNullOrWhiteSpace(prepay_id))
            {
                RequestHandler rh = new RequestHandler(HttpContext.Current);
                rh.SetParameter("appid", order.appid);
                rh.SetParameter("attach", _poid);
                rh.SetParameter("body", order.body);
                rh.SetParameter("device_info", order.device_info);
                rh.SetParameter("mch_id", order.mch_id);
                rh.SetParameter("nonce_str", nonce_str);
                rh.SetParameter("notify_url", order.notify_url);
                rh.SetParameter("out_trade_no", _poid);
                rh.SetParameter("spbill_create_ip", WebRequests.GetIP());
                rh.SetParameter("total_fee", "10");//单位是分
                string openid = "";
                try
                {
                    openid = DBUtility.DbHelperSQL.GetSingle("select [AUTH_OpenID] from [SS_User_AUTH] where [User_ID]=" + _uid + " and User_Type='W' and Enabled=1 order by [AUTH_ID] desc").ToString();
                }
                catch
                {
                }
                if (string.IsNullOrWhiteSpace(openid))
                {
                    return toJson("{\"Status\":0,\"Msg\":\"您尚未绑定微信用户,请前往会员中心绑定.\"}");
                }
                rh.SetParameter("openid", openid);
                rh.SetParameter("trade_type", order.trade_type);//order.trade_type
                order.sign = rh.CreateMd5Sign("key", order.appkey);
                rh.SetParameter("sign", order.sign);

                return_string = rh.ParseXML();
                request_data = Senparc.Weixin.MP.AdvancedAPIs.TenPayV3.Unifiedorder(return_string);//.Replace("<![CDATA[", "").Replace("]]>", "")

                SortedDictionary<string, string> requestXML = GetInfoFromXml(request_data);
                foreach (KeyValuePair<string, string> k in requestXML)
                {
                    if (k.Key == "prepay_id")
                    {
                        prepay_id = k.Value;
                        break;
                    }
                }
                SqlTransaction trans = null;
                using (SqlConnection conn = new SqlConnection(DBUtility.DbHelperSQL.ConnectionString))
                {
                    conn.Open();
                    using (trans = conn.BeginTransaction())
                    {
                        BLL.PO_Main.Instance.UpdateField(conn, trans, "PO_ID='" + _poid + "'", "TP_ID='" + prepay_id + "'");
                        trans.Commit();
                    }
                }
            }

            SortedDictionary<string, string> sParams = new SortedDictionary<string, string>();
            sParams.Add("appId", order.appid);
            sParams.Add("timeStamp", timestamp);
            sParams.Add("nonceStr", nonce_str);
            sParams.Add("package", "prepay_id=" + prepay_id);
            sParams.Add("signType", "MD5");
            var paySign = TenpayUtil.getsign(sParams, order.appkey);
 
            return toJson("{\"Status\":1,\"appid\":\"" + order.appid + "\",\"timeStamp\":\"" + timestamp + "\",\"noncestr\":\"" + nonce_str + "\",\"package\":\"" + "prepay_id=" + prepay_id + "\",\"signType\":\"MD5\",\"paySign\":\"" + paySign + "\"}");
        }
{% endhighlight%}



