---
layout:        post
title:         asp.net+Ajax如何实现Highcharts图表组件支持大数据（上万个数据点)
description:   "asp.net+Ajax如何实现Highcharts图表组件支持大数据（上万个数据点)"
tags: [Highcharts,.NET,Ajax]
image:
  background: body_bg.gif
comments: true
share: true
---

###摘要

前面我已经总结了几个关于Highcharts图表组件的常用问题：<a href="http://www.stepday.com/topic/?272" target="_blank">关于Highcharts高性能图表组件使用问题汇总大集合</a> ，此处特介绍一下如何在asp.net环境下采用Ajax动态加载Highcharts图表，且支持大数据量。

    
<!--more-->

###新建一个Index.aspx页面 

{% highlight JavaScript %}
<html xmlns="http://www.w3.org/1999/xhtml">
<head runat="server">
    <title>HighCharts纯JS图表如何应对大数据量</title>
    <script type="text/javascript" src="/js/jquery.min.js"></script>
    <script type="text/javascript">
        var chart;
        var options;

        $(function () {            
            $(document).ready(function () {
                options = {
                    chart: {
                        renderTo: 'container',
                        type: 'line'
                    },
                    title: {
                        text: 'HighCharts大数据量支持情况',
                        x: -20 //center
                    },
                    subtitle: {
                        text: '',//副标题
                        x: -20
                    },
                    xAxis: {
                        categories: ['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12'],
                        labels: {
                            step: 200,
                            rotation: 0,//坐标旋转角度 为0：表示横放置
                            staggerLines: 0, //标签的交错显示（上下)
                            enable:true //是否显示坐标值
                        },
                        title: {
                            text:'月份'
                        },
                        //标线属性
                        plotLines: [{
                            value: 0,
                            width: 1,
                            color: '#808080'
                        }]
                    },
                    yAxis: {
                        title: {
                            text: '百分比(%)'
                        },
                        //标线属性
                        plotLines: [{
                            value: 0,
                            width: 1,
                            color: '#808080'
                        }]
                        //,
                        //max:'50', //y轴最大值
                        //min:'10' //y轴最小值
                    },
                    tooltip: {
                        formatter: function () {
                            return '<b>' + this.series.name + '</b><br/>' +
                        this.x + ': ' + this.y + '';
                        }
                    },
                    plotOptions: {
                        series: {
                            marker: {
                                enabled: false,
                                states: {
                                    hover: {
                                        enabled: true
                                    }
                                }
                            }
                        }
                    },
                    credits: {
                        enabled: false   //右下角不显示LOGO
                    },  
                    series: [{
                        name: 'Tokyo',
                        data: [7.0, 6.9, 9.5, 14.5, 18.2, 21.5, 25.2, 26.5, 23.3, 18.3, 13.9, 9.6]
                    }, {
                        name: 'New York',
                        data: [-0.2, 0.8, 5.7, 11.3, 17.0, 22.0, 24.8, 24.1, 20.1, 14.1, 8.6, 2.5]
                    }, {
                        name: 'Berlin',
                        data: [-0.9, 0.6, 3.5, 8.4, 13.5, 17.0, 18.6, 17.9, 14.3, 9.0, 3.9, 1.0]
                    }, {
                        name: 'London',
                        data: [3.9, 4.2, 5.7, 8.5, 11.9, 15.2, 17.0, 16.6, 14.2, 10.3, 6.6, 4.8]
                    }]
                }; 
                Query();              
            });
        });
        function Query()
        {
            var MaxNum = document.getElementById("txtMaxNum").value;
             $.ajax({
                    type: 'POST',
                    async: false,
                    url: "/Ajax/Index_Ajax.aspx?" + "RequestTime=" + (new Date().getTime()).toString() + "&type=series&MaxNum="+MaxNum,
                    dataType: "json",
                    beforeSend: function () {
                    },
                    success: function (msg) {
                        if (msg) {
                            //设置X轴
                            options.xAxis.categories = eval(msg.categories);
                            //设置Y轴
                            options.series = eval(msg.series);                            
                            //设置图表
                            chart = new Highcharts.Chart(options);
                        }
                    },
                    error: function (errorMsg) {
                    }
                });
        }
    </script>
</head>
<body>
    <form id="form1" runat="server">
    <script type="text/javascript" language="javascript" src="/js/highcharts.js"></script>
    <script type="text/javascript" language="javascript" src="/js/exporting.js"></script>
    <div id="container" style="min-width: 400px; width:600px; height: 400px; margin: 0 auto">
    </div>
    <div>
        设置数据点<input type="text" id="txtMaxNum" value="1000" />
        &nbsp;&nbsp;
        <input type="button" value="查询" onclick="Query()" />
    </div>
    <div>
        实际产生的数据点个数=设置的数据点*10(默认设置了10个类别) 
    </div>
    </form>
</body>
</html>
{% endhighlight%}

###创建一个/Ajax/Index_Ajax.aspx页面 

核心代码如下： 

{% highlight JavaScript %}
			int MaxNum;
        ////// 初始化参数
        /////////protected void Page_Load(object sender, EventArgs e)
        {
            string getType = Request["type"];
            MaxNum = Convert.ToInt32(Request["MaxNum"]);
            if (!string.IsNullOrEmpty(getType))
            {
                switch (getType)
                { 
                    case "series":
                        GetSeries();
                        break;
                    case "xAxis":
                        GetxAxisCategories();
                        break;
                }
            }
        }

        ////// 获取横轴表的标签
        ///protected void GetxAxisCategories()
        {
            string xAxisCategories = "";//"['北京市', '上海市', '重庆市', '天津市','抚州市','长沙市','常德市','济南市']";
            xAxisCategories += "[";
            for (int i = 0; i < 1000; i++)
            {
                if (!xAxisCategories.Equals("["))
                    xAxisCategories += ",'" + i.ToString() + "月'";
                else
                    xAxisCategories += "'" + i.ToString() + "月'";
            }
            xAxisCategories += "]";
            Response.Write(xAxisCategories);
            //Response.End();
        }

        ////// 获取横坐标标签以及数据点
        ///protected void GetSeries()
        {
            
            string returnValue = "";//"[{name: '人员体系城市分布情况',data: [15,18,13,13,10,16,23,23]}]";
            string xAxisCategories = "";//"['北京市', '上海市', '重庆市', '天津市','抚州市','长沙市','常德市','济南市']";
            xAxisCategories += "[";
            returnValue = "[";
            for (int i = 0; i < MaxNum; i++)
            {
                if (!xAxisCategories.Equals("["))
                    xAxisCategories += ",'" + i.ToString() + "月'";
                else
                    xAxisCategories += "'" + i.ToString() + "月'";
            }
            for(int i =0;i<10;i++)
            {
                if (!returnValue.Equals("["))
                    returnValue += ",{name:'" + i.ToString() + "君',data:["+GetRandomData(i)+"]}";
                else
                    returnValue += "{name:'" + i.ToString() + "君',data:[" + GetRandomData(i) + "]}";
            }
            returnValue += "]";
            xAxisCategories += "]";
            var rel = new {
                categories = xAxisCategories,
                series = returnValue
            };
            Response.Write(rel.ToJson());
            Response.End();
        }

        ////// 获取随机数据，仅仅用于测试之用
        /////////protected string GetRandomData(int MinNum)
        {
            string dataStr = "";
            for (int i = 0; i < MaxNum; i++)
            {
                if (string.IsNullOrEmpty(dataStr))
                    dataStr += new Random().Next(MinNum, MaxNum).ToString();
                else
                    dataStr += "," + new Random().Next(MinNum, MaxNum).ToString();
            }
            return dataStr;
        }
{% endhighlight%}

###特别说明 

1）、为了将其List转换为Json方便，工程内引用了Newtonsoft.Json.dll和一个公用类JsonHelper.cs。这两个均可以在网上直接下载到。 

2）、Demo内的数据均为随机数据，后期可自行修改从数据库内获取数据； 

3）、务必需要保证sereis内接收的数据位非字符串类型才是，否则会报出Highcharts errors 14的错误。

转自:<a href="http://www.stepday.com/topic/?273" target="_blank">http://www.stepday.com/topic/?273</a>


