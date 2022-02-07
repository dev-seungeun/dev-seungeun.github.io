---
title: Spring Framework Swagger 적용
author: SeungEun Baek
date: 2022-02-07 11:18 
description: Spring MVC Swagger 적용 방법 작성
category: bse
layout: post
---
[![Hits](https://hits.seeyoufarm.com/api/count/incr/badge.svg?url=https%3A%2F%2Fdev-seungeun.github.io%2F1study%2Fnetflix_zuul%2F&count_bg=%23FEC8E6&title_bg=%23B2ADAD&icon=&icon_color=%23515050&title=hits&edge_flat=false)](https://hits.seeyoufarm.com)   


## 0. 시작

MSA를 공부하며 Netflix Zuul을 알게되었다. 
Spring cloud Zuul을 적용하며 공부한 내용을 정리한다.
Spring cloud Zuul은 annotation과 yml설정만으로 간단히 NetflixOSS를 사용할 수 있다.
<br><br>     

## 1. MSA, 그리고 API GATEWAY란?

#### MSA - MicroService Architecture
``` 
MSA개념이 나오기 전에는 하나의 API서버에 모든 서비스가 존재했다. (Monolithic Architecture)  
MSA는 큰 어플리케이션을 여러개의 작은 어플리케이션으로 나누어 조합과 변경이 가능하도록 만들어진 아키텍쳐이다.
```

#### API GATEWAY
```
MSA에서 언급되는 컴포넌트 중 하나이다.
모든 요청에 대한 end point를 통합하는 서버이며 프록시 서버처럼 동작한다. (요청을 각 Micro Service에 토스해준다)
각 서비스의 end point를 여기서 통합 관리하기 때문에 서비스들의 도메인이 변경되었을때 쉽게 관리할 수 있다.
서비스들이 여러 도메인으로 나누어져있지만 클라이언트 입장에서는 Gateway으로 요청하면되기 때문에 클라이언트에서는 각 서비스의 도메인 관리를 할 필요가 없다.
```
<br>

## 2. Netflix Zuul 

### Zuul 정의
#### Netflix WIKI 발췌 - [Netflix WIKI](https://github.com/Netflix/zuul/wiki)
``` 
Zuul is the front door for all requests from devices and web sites to the backend of the Netflix streaming application. 
As an edge service application, Zuul is built to enable dynamic routing, monitoring, resiliency and security. 
It also has the ability to route requests to multiple Amazon Auto Scaling Groups as appropriate.
```

Netflix Zuul은 Filter에 기능을 정의하고, 이슈사항에 발생시 적절한 filter을 추가함으로써 이슈사항을 대비할 수 있다.
Filter File Manager에서는 일정 주기(정해진 시간) 마다 정해진 directory에서 groovy로 정의된 filter 파일을 가져온다.
request 요청이 들어 올때마다 preRoute(), route(), postRoute()에서 ZuulFilter Runner를 실행한다.

### Zuul Filter
```
Zuul Filter는 크게 4가지 Filter로 나뉜다.

PreFilter      – 라우팅전에 실행되며 필터이다. 주로 logging, 인증등이 pre Filter에서 이루어 진다.
RoutingFilter  – 요청에 대한 라우팅을 다루는 필터이다. Apache httpclient를 사용하여 정해진 Url로 보낼수 있고, Neflix Ribbon을 사용하여 동적으로 라우팅 할 수도 있다.
PostFilter     – 라우팅 후에 실행되는 필터이다. response에 HTTP header를 추가하거나, response에 대한 응답속도, Status Code, 등 응답에 대한 statistics and metrics을 수집한다.
ErrorFilter    – 에러 발생시 실행되는 필터이다.
CustomFilter   - PreFilter에서 호출하여 사용할 수 있다.
```
![image](https://user-images.githubusercontent.com/80504390/152724574-12175930-59e3-4e05-a096-63d6070bb773.png)   
출처 - https://netflixtechblog.com/announcing-zuul-edge-service-in-the-cloud-ab3af5be08ee
<br><br>

## 3. Spring cloud Zuul

spring boot 프로젝트에 artifactId 'spring-cloud-starter-zuul'를 추가하고(아래참고) main class에 @EnableZuulProxy또는 @EnableZuulServer라고 명시해주면 zuul 서버가 구축된다.
@EnableZuulProxy와 @EnableZuulServer 두개의 annotation은 완전히 다른 것이 아니고 @EnableZuulProxy가 @EnableZuulServer을 포함하고 있다. 
@EnableZuulServer에서 PreDecorationFilter, RibbonRoutingFilter, SimpleHostRoutingFilter를 추가하면, @EnableZuulProxy가 된다.

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-zuul</artifactId>
    <version>2.0.2.RELEASE</version>
</dependency>
```

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.security.servlet.SecurityAutoConfiguration;
import org.springframework.cloud.client.discovery.EnableDiscoveryClient;
import org.springframework.cloud.netflix.zuul.EnableZuulProxy;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

import com.wjthinkbig.gateway_swagger.filters.ErrorFilter;
import com.wjthinkbig.gateway_swagger.filters.PostFilter;
import com.wjthinkbig.gateway_swagger.filters.PreFilter;
import com.wjthinkbig.gateway_swagger.filters.RouteFilter;

@Configuration
@SpringBootApplication(exclude= {SecurityAutoConfiguration.class})
@EnableZuulProxy
@EnableDiscoveryClient
@EnableAutoConfiguration 
@ComponentScan(basePackages = {"com.my.gateway"})
public class GatewayApplication {
    public static void main(String[] args) { 
      SpringApplication.run(GatewayApplication.class, args); 
    }

    @Bean
    public PreFilter preFilter() {
      return new PreFilter();
    }
    @Bean
    public PostFilter postFilter() {
      return new PostFilter();
    }
    @Bean
    public ErrorFilter errorFilter() {
      return new ErrorFilter();
    }
    @Bean
    public RouteFilter routeFilter() {
      return new RouteFilter();
    }      
}
```
<br>

## ETC

내가 적용한 Gateway를 예를 들자면 
PreFilter에서 Request Url을 체크하고 Auth-Token의 유효성을 체크한다. 그리고 Request시작시간을 RequestContext에 저장한다.
PostFilter에서는 PreFilter에서 저장한 Request시작시간을 사용하여 응답시간을 계산하여 로그로 적재하고 있다.)   
<br><br>
<hr>

##### [참고사이트]<br> 
[배민 API GATEWAY – spring cloud zuul 적용기](https://techblog.woowahan.com/2523/)<br>
[Netflix Tech Blog](https://netflixtechblog.com)
<br><br>
