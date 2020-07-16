**jango.jobcenter demo**

1. import Jango.JobCenter  
 https://www.nuget.org/packages/Jango.JobCenter
dotnet add package Jango.JobCenter --version 1.0.0

2. edit Starup.cs
   add using Jango.JobCenter;
   
 
            services.AddControllers();

            // add jango jobcenter
            // use memorystorage by  default
            services.AddJangoJobCenter();

            //or use Mongo /  sqlserver / mysql ...
            //services.AddJangoJobCenter(x => x.UseMongoStorage("...."));
        
       
3. add  app.UseJangoJobCenter()  before UseEndpoints;
   
    


            /// use jango jobcenter
            app.UseJangoJobCenter();

            app.UseEndpoints(endpoints =>
            {
                endpoints.MapControllers();
            });


[分布式系统架构之快速构建你的任务调度中心](https://www.cnblogs.com/Sages/p/13322317.html)

>github :https://github.com/jangocheng/Jango.JobCenter.demo.git

>码云	:https://gitee.com/sagecity/Jango.JobCenter.Demo.git



分布式系统中，我们经常会遇到定时执行任务，而这些定时任务中，多数情况都是需要执行一些http请求。
比如：

轮训支付结果（虽然第三方支付中心有支付回调，但有时候并不能有效保证你的业务系统能收到正确的结果）
未支付订单超时取消，电商系统订单，用户未支付订单，超时后取消订单
已支付已签收订单，超时后自动完成订单
同步微信公众号用户数据做分析
同步企业微信通讯录及客户信息
等等
很多业务场景都需要用到定时执行http请求的任务
本次，我们在netcore 环境，使用 Jango.JobCenter
来快速构建我们的任务调度中心

本次，我们在netcore 环境，使用 [Jango.JobCenter](https://www.nuget.org/packages/Jango.JobCenter)


>[Jango.JobCenter](https://www.nuget.org/packages/Jango.JobCenter)目前是基于Hangfire的 .NETStandard 2.0版本
Demo源码，请移步[https://github.com/jangocheng/Jango.JobCenter.demo](https://github.com/jangocheng/Jango.JobCenter.demo)

> dotnet new webapi 创建一个webapi项目

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716124801976-134545178.png)

> dotnet add package Jango.JobCenter --version 1.0.0.1 

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716125301805-782401913.png)

编辑StartUp文件
1. 引用 Jango.JobCenter
```
using Jango.JobCenter;
```
2. 修改ConfigureServices(IServiceCollection services)
```
services.AddJangoJobCenter();
```
3. 修改Configure(IApplicationBuilder app, IWebHostEnvironment env)
```
app.UseJangoJobCenter();
```


![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716125804135-1040593630.png)


还原依赖包后，dotnet run 运行

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716130713426-328903449.png)


可以看到，我们的任务调度中心已经运行起来了。

https://localhost:5001/hangfire 查看任务中心仪表盘

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716131019130-597320024.png)

https://localhost:5001/swagger/index.html 查看Jango.JobCenter 的文档

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716131047553-1265206232.png)

Jango.JobCenter
第一个接口为测试接口，仅仅Console输出

```
curl -X POST "https://localhost:5001/console" -H "accept: */*" -d ""
```
或者
postman post 调用https://localhost:5001/console
![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716132800411-1822677936.png)

可以看到结果输出 job name console is working now. 

第二个接口为http 创建定时任务接口
参数如下：
```
{
  "name": "",//job name
  "desc": "",//job 描述
  "url": "",//定时调用url
  "method": "", //http method ：get/post/put/delete
  "header": "",//http header 以kv形式，多个以,分割 比如: k1:v1,k2:v2
  "requestBody": "",//http 请求体
  "isRetry": true,//遇到错误是否重试
  "retryTimes": 0,//重试次数
  "isCircle": true,//是否周期性任务
  "cron": "*/5 * * * * ?"//cron 表达式  此项移除，则系统默认5s执行一次http请求
}
```

我们来测试下：
参数输入：
```
{
  "name": "demo", //demo
  "desc": "定时调用百度",//
  "url": "https://www.baidu.com",
  "method": "get",//get方式
  "header": "",
  "requestBody": "",
  "isRetry": false,// 错误后不重试
  "retryTimes": 0,
  "isCircle": true//周期性任务 
  //无Cron参数，系统默认每隔5s执行一次任务
}
```
postman执行：
返回结果：
![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716133707675-1940218962.png)
来看下console输出：

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716133746967-1792504612.png)

黄色背景的内容，即为定时执行的结果。
我们来看下任务中心面板
![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716134034656-1508498040.png)

我们来看下任务中心仪表盘
![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716133912042-618763538.png)
可以看到已经开始执行任务了

至此，你的任务调度中心，已经构建完成。可以任意创建相关任务了。

如需技术支持：请加微信联系

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716134700432-1308048964.jpg)

或加qq：649205176



## **获取Jango.Center源码及技术支持，请加微信，打赏一杯咖啡哈**

![image](https://img2020.cnblogs.com/blog/161781/202007/161781-20200716135239662-417455125.png)

# 未来计划
- **分布式系统架构之构建你的网关中心**
- **分布式系统架构之构建基于事件的消息引擎**
- **分布式系统架构之构建基于事件的规则调度引擎**
- **微服务系统架构与设计**
- ... 
