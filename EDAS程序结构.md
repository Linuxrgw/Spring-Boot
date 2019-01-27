# Spring-Boot
极简EDAS程序结构

在EDAS中，服务调用方和服务提供方通过EDAS Config Center互相发现。一个最小的EDAS程序包含如下三个部分： 服务提供方 服务调用方 发现机制（EDAS Config Center）

EDAS<a href=开发入门">

EDAS Config Center可以在阿里官网下载并配置

 

定义服务接口

一个服务总是从接口定义开始 public   interface GreetingService   { 

public  String  say ( String  words ) ; 

}

服务提供方(Provider)

服务提供方提供服务接口的实现 public   class  GreetingServiceImpl  implements  GreetingService   { 

@Override 
public   String  say ( String  words )   { 
return   "Hello EDAS! "   +  words ; 
} 
}

EDAS通过xml文件(hsf-beans-provider.xml)声明接口和实现 <hsf:provider id="GreetingService" interface="com.example.service.GreetingService" 

ref="GreetingServiceImpl" version="1.0.0.demo1" group="service_demo"> 

</hsf:provider> 

<bean id="GreetingServiceImpl" class="com.example.service.impl.GreetingServiceImpl"/>

服务调用方(Consumer)

跟服务提供方类似，服务提供方也需要在xml文件(hsf-beans-consumer.xml)中声明要调用的服务接口 <hsf:consumer 

id="GreetingService" interface="com.example.service.GreetingService" 

version="1.0.0.demo1" group="service_demo"> 

</hsf:consumer>

 

远程调用的服务bean名字为”GreetingService”，在下面的代码中要用到 WebApplicationContext context   =  WebApplicationContextUtils. getWebApplicationContext (request. getServletContext ( ) ) ; 

LoginService loginService   = (LoginService )context. getBean ( "GreetingService" ) ;

讨论

EDAS Config Center安装后，可能会无法启动，目前推测，可能跟数据库驱动有关
