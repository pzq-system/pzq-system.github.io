# windows 高效虚拟机

1,WSL:Windows subsystem for linux   （账号peng 密码 1054257517） 安装 wsl --install -d Ubuntu/

无法安装：修改DNS 114.114.114.114 / 8.8.8.8

查看本地文件 cd /mnt

定期更新和升级包：sudo apt update && sudo apt upgrade

2,SandBox

# .NET6

## 杂项

1. 文档 [.NET 文档 | Microsoft Learn](https://learn.microsoft.com/zh-cn/dotnet/)
2. Zack.EFCore.Batch是一个支持在Entity Framework Core中高效删除和更新数据的开源库

# ASP.NET CORE 笔记

## 过滤器 Filters

1. [Authorization filters]([ASP.NET Core 中的筛选器 | Microsoft Learn](https://learn.microsoft.com/zh-cn/aspnet/core/mvc/controllers/filters?view=aspnetcore-7.0&viewFallbackFrom=aspnetcore-2.2#authorization-filters))，授权过滤器是最先执行并且决定请求用户是否经过授权认证，如果请求未获授权，授权过滤器可以让管道短路。
2. [Resource filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-2.2#resource-filters)，资源过滤器在Authorization filter执行完后执行，它有两个方法，OnResourceExecuting可以在filter管道的其余阶段之前运行代码，例如可以在模型绑定之前运行。OnResourceExecuted则可以在filter管道的其余阶段之后运行代码
3. [Action filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-2.2#action-filters)，操作过滤器可以在调用单个操作方法之前和之后立即运行代码，它可以用于处理传入某个操作的参数以及从该操作返回的结果。 需要注意的是，Aciton filter不可以在 Razor Pages 中使用。
4. [Exception filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-2.2#exception-filters)，异常过滤器常常被用于在向响应正文写入任何内容之前，对未经处理的异常应用全局策略。
5. [Result filters](https://docs.microsoft.com/en-us/aspnet/core/mvc/controllers/filters?view=aspnetcore-2.2#result-filters)，结果过滤器可以在执行单个操作结果之前和之后运行代码。 但是只有在方法成功执行时，它才会运行。

## 运行方式

第一种   直接在项目目录下面执行dotnet run

第二种   直接在项目目录下面\bin\Debug\netcoreapp3.1

dotnet 项目名称.dll --urls="比如http://localhost:8080"

## aop思想

不同于asp.net 全家桶，冗余，你要不要都加载进来

asp.net core 可以自由选择，需要用什么就可以直接去加载

比如使用session

在startup.cs  ConfigureServices里面 注册服务 services.AddSession();

在startup.cs  ConfigureServices里面 注册服务  app.UseSession();

## 使用日志插件

引用包名Microsoft.Extensions.Logging.Log4Net.AspNetCore 

添加配置文件log4net.config

```
<?xml version="1.0" encoding="utf-8" ?>
<log4net>
	<appender name="DebugAppender" type="log4net.Appender.DebugAppender" >
		<layout type="log4net.Layout.PatternLayout">
			<conversionPattern value="%date [%thread] %-5level %logger - %message%newline" />
		</layout>
	</appender>
	<!--指定日记记录方式，以滚动文件的方式（文件记录）-->
	<appender name="RollingFile" type="log4net.Appender.RollingFileAppender">
		<!--日志路径-->
		<file value="log\log.txt" />
		<!--是否是向文件中追加日志-->
		<appendToFile value="true" />

		<!--防止多线程时不能写log,官方说线程非安全-->
		<lockingModel type="log4net.Appender.FileAppender+MinimalLock"/>
		
		<!--log保留天数-->
		<param name= "MaxSizeRollBackups" value= "20"/>
		<!--每个文件最大1M-->
		<param name="maximumFileSize" value="3MB" />
		<!--日志根据日期滚动-->
		<param name="RollingStyle" value="Date" />
		<!--日志文件名格式为:logs_20080831.log-->
		<param name="DatePattern" value="&quot;logs_&quot;yyyyMMdd&quot;.log&quot;" />
		<!--日志文件名是否是固定不变的-->
		<param name="StaticLogFileName" value="false" />
		<!--布局-->
		<layout type="log4net.Layout.PatternLayout">
			<conversionPattern value="%date %5level %logger.%method [%line] - MESSAGE: %message%newline %exception" />
		</layout>
	</appender>
	<root>
		<level value="ALL"/>
		<appender-ref ref="DebugAppender" />
		<appender-ref ref="RollingFile" />
	</root>
</log4net>
```

在program.cs 添加

.ConfigureLogging((context, ILoggingBuilder) =>

 {
        ILoggingBuilder.AddFilter("System", LogLevel.Warning);//过滤规则
        ILoggingBuilder.AddFilter("Microsoft", LogLevel.Warning);
        ILoggingBuilder.AddLog4Net();
 })

## 发布部署

部署到IIS 需要安装 **AspNetCoreModuleV2模块** 下载地址 https://dotnet.microsoft.com/download/dotnet-core/3.1 

## asp.net core获取真实客户端IP地址

1、新增一个依赖注入

```c#
services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
```

2、控制器

```C#
private readonly IHttpContextAccessor _httpContextAccessor;
   public TestController( IHttpContextAccessor httpContextAccessor) 
    {
            _httpContextAccessor = httpContextAccessor;    
    }
```

3、使用

```C#
 string ipaddress = _httpContextAccessor.HttpContext.Connection.RemoteIpAddress.ToString();
```

通过上面的方法可以正常获取到IP地址。

如果有使用Nginx做反向代理的话，使用上面的方式获取到的IP会是127.0.0.1

```C#
 if (Request.Headers.ContainsKey("X-Real-IP"))
  {
       sb.AppendLine($"X-Real-IP：{Request.Headers["X-Real-IP"].ToString()}");
     }

     if (Request.Headers.ContainsKey("X-Forwarded-For"))
     {
       sb.AppendLine($"X-Forwarded-For：{Request.Headers["X-Forwarded-For"].ToString()}");
     }   
```

因为实际使用，我们是无法通过RemoteIpAddress直接获取到真实的客户端地址的。

如果一定要使用，那么可以通过添加中间件的方式

```C#
public class RealIpMiddleware
{
    private readonly RequestDelegate _next;

    public RealIpMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public Task Invoke(HttpContext context)
    {
        var headers = context.Request.Headers;
        if (headers.ContainsKey("X-Forwarded-For"))
        {
            context.Connection.RemoteIpAddress=IPAddress.Parse(headers["X-Forwarded-For"].ToString().Split(',', StringSplitOptions.RemoveEmptyEntries)[0]);
        }
        return _next(context);
    }
}
```

在Startup中的Configura添加下面一句

```
app.UseMiddleware<RealIpMiddleware>();
```

# ChatGPT

账号：gqjksylkoxat25a@hotmail.com账号密码：3TC9AFFi75邮箱密码：t8uo9gqQ6w key：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
