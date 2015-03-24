---
layout:        post
title:         web api调取数据配合app
description:   "web api调取数据配合app"
tags: [HTML5,Manifest]
image:
  background: body_bg.gif
comments: true
share: true
---

###前言

      为了配合app数据接口,需要开发数据接口,为了学习,无耻的使用了webapi.
      
###总结

	个人经验,欢迎打脸.
	
####命名

	在Asp.Net Web API中，一个控制器就是一个处理HTTP请求的类，控制器的public 方法被叫做action方法或者简单的Aciton。当Web API接收到一个请求的时候，它将这个请求路由到一个Action。 

　　为了确定那个Action被调用，这个框架使用了一个路由表。Visual Studio中Web API的项目模板会创建一个默认路由,这个路由是在WebApiConfig.cs文件中定义的，该文件位于App_Start目录：

{% highlight JavaScript %}
			config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );
{% endhighlight%}

	路由表中的每一个条目都包含一个路由模板。这个Web API默认的路由模版是"api/{controller}/{id}"。在这个模版中，“api”是一个文字式路径片段，而{controller}和{id}则是占位符变量。 

　　当Web API框架接收一个HTTP请求时，它会试图根据路由表中的一个路由模板来匹配其URI。如果无路由匹配，客户端会接收到一个404（未找到）错误。
	
	在路由中使用“api”的原因是为了避免与ASP.NET MVC的路由冲突。通过这种方式，可以用“/contacts”进入一个MVC控制器，而“/api/contacts”进入一个Web API控制器。当然，如果你不喜欢这种约定，你也可以修改这个默认路由表。 

 	一旦一个匹配的路由被发现，Web API便会选择相应的Controller和Action。 

　　1.为了找到Controller,Web API会把“控制器”加到{controller}变量的值。 

　　2.为了找到Action，Web API会查找HTTP方法，然后寻找一个名称以HTTP方法名开头的方法。例如，对于一个Get请求，Web API会查找一个以“Get…”开头的动作，如“GetContact”或“GetAllContacts”等。这种约定只应用于GET、POST、PUT和DELETE方法。通过在你的Controller上使用attributes,你可以启用其他的HTTP方法。

　　3.路由模版中其他的占位变量，例如｛id｝,将被映射成Action的参数。 

####Get请求

	一、无参数Get请求

　　一般的get请求我们可以使用jquery提供的$.get() 或者$.ajax({type:"get"}) 来实现：

{% highlight JavaScript %}
	$.get("api/Foodlist/Getlist", function (data) {
        console.log(JSON.stringify(data));
    });
{% endhighlight%}

	请求的后台Action方法仍为上篇文章中的Getlist() :

{% highlight JavaScript %}
	public List<Food> Getlist()
	{
	    return null;
	}
{% endhighlight%}
	
	二、传递一个参数的Get请求

　　通常我们需要传递参数只需要指定ajax方法的data属性即可： data:{"name":"张三","num":2}

{% highlight JavaScript %}
	$.ajax({
        url:"api/food",
        type:"get",
        data:{"name":"张三","num":2},
        success:function(data){
            console.log(data);
        }
    });
{% endhighlight%}

	后台正确的返回数据：

{% highlight JavaScript %}
	public string GetOrder(string name,int num)
	{
	    return name+"订购了"+num+"份菜";
	}
{% endhighlight%}

	三、无参数的Post请求

　　我们可以使用$.post() 或$.ajax({type:"post"}) 来发起post请求：

{% highlight JavaScript %}
	$.ajax({
		url:"api/Foodlist/Postlist",
		type:"Post",
		success:function (data) {
        	console.log(JSON.stringify(data));
    	}
    });
{% endhighlight%}

	请求的后台Action方法仍为上篇文章中的Getlist() :

{% highlight JavaScript %}
	public List<Food> Postlist()
	{
	    return null;
	}
{% endhighlight%}

	四、传递带参数的Post请求：

　　在Action 方法中我们需要使用 [FromBody] 属性标签来标明属性。在post请求中，方法的参数必须要用 [FromBody] 属性来修饰才可以， [FromBody] 就告诉Web API 要从post请求体重去获取参数的值。Web API 要求请求传递的 [FromBody] 参数，肯定是有一个特定的格式，才能被正确的获取到。而这种特定的格式并不是我们常见的 key=value 的键值对形式。Web API 的模型绑定器希望找到 [FromBody] 里没有键名的值，也就是说， 不是 key=value ，而是 =value 。
	
	请求:
	
{% highlight JavaScript %}
	$.ajax({
        url:"api/food",
        type:"post",
        data:{"":"张三"},
        success:function(data){
            console.log(data);
        }
    });
{% endhighlight%}

	后台处理:
	
{% highlight JavaScript %}
	public string Postname([FromBody]string name)
	{
	    return string.Format("姓名:{0}",name);
	}
{% endhighlight%}

	Post多个参数的处理:[FromBody] 修饰的参数只能有一个。我们需要对传递的多个参数进行封装才行。这里，我们可以将 Name和 Num 封装成一个User类,并且带一个对象数组：
	
{% highlight JavaScript %}
	public class User()
	{
	    public string name{set;get;}
	    public int num{set;get;}
	    public List<OrderList> orderlist { get; set; }
	}
	public class OrderList
    {
        public int id { get; set; }
        public int fd_amount { get; set; }
        public string rate { get; set; }
        public string cost { get; set; }
    }
{% endhighlight%}

	前台数据及请求:
	
{% highlight JavaScript %}
	var user={
		name:"张三",
		num:2,
		orderlist:[{id:1,fd_amount:1,rate:"100",cost:"88"},{id:2,fd_amount:1,rate:"100",cost:"88"}]
	
	};
	$.ajax({
        url:"api/food",
        type:"post",
        data:JSON.stringify(user),
        Content-type:"application/json",
        success:function(data){
            console.log(data);
        }
    });
{% endhighlight%}

	后台接收处理:
	
{% highlight JavaScript %}
		[HttpPost]
        public string Post([FromBody]User user)
        {
        	return string.Format("姓名:{0},订购:{1}份,其中一份为:{2}",user.name,user.num,user.orderlist[0].id);
        }
{% endhighlight%}

####Post请求中对于参数的处理是需要重点注意.