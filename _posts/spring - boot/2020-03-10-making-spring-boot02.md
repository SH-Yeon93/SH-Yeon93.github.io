---
layout: post
title:  "[springBoot]  인터셉터 등록하기"
subtitle:   "Interceptor"
date: 2020-03-10. 12:56 +0900
categories: SpringBoot
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---

## 1. 설명
인터셉터는 MVC 프로젝트에서 모든 Controller 요청에 대해여 요청을 가로챈다는 뜻입니다 



## 2. 소스(Interceptor Main)

```
package com.example.demo.interceptor;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

import org.springframework.web.servlet.handler.HandlerInterceptorAdapter;

public class CommonInteceptor extends HandlerInterceptorAdapter{

	@Override
	public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
			throws Exception {
		
		System.out.println("인터셉터 호출");

		return true;
	}

}

```

기본적인 디펜더시 입니다 이것을 pom.xml 에 넣어도 되지만 프로젝트 만들때 설정할 수 있습니다 

## 2.1 소스 (webConfig)

```
package com.example.demo.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import com.example.demo.interceptor.CommonInteceptor;

@Configuration //시작할때 bean 으로 스캔을 해둠 
public class WebConfig  implements WebMvcConfigurer{

	@Override
	public void addInterceptors(InterceptorRegistry registry) { //인터셉터 등록
			registry.addInterceptor(new CommonInteceptor()) //인터셉터가 될 클래스
			.addPathPatterns("/**") // 어떤 path 호출시 인터셉터를 호출할지
			.excludePathPatterns("/MM/excludePath"); // 인터셉터를 제외할 path 설정
	}

}


```
2.webConfig 작성 

boot 는 xml 이 없기때문에 인터셉터 같은 bean 같은 경우는 등록을 해주어야 합니다 그런 등록을 해주는 역활을 하는게 webConfig 입니다

정확하게는 webConfig 가 아니라 상단에 @Configuration 을 등록함으로서 bean 을 자동적으로 baen - scan 범위에 들어가 있고 bean 을 등록을 합니다 


그러면 간단하게 컨트롤러를 작성해보겠습니다 


## 2.2 소스 (xml 예전 소스 )
```
<mvc:interceptors>
		<mvc:interceptor>
			<mvc:mapping path="/**"/>
			<mvc:exclude-mapping path="/registerPage"/>
			<mvc:exclude-mapping path="/loginPage"/>
			<mvc:exclude-mapping path="/registerData"/>
			<mvc:exclude-mapping path="/login"/>
			<beans:bean id = "userBean" class = "com.com.interceptor.UserInteceptor"></beans:bean>
		</mvc:interceptor>
	</mvc:interceptors>

```
예전에는 intereptor 을 xml 로 등록을 할때 이렇게 등록을 했었습니다 그에 비하면 java 는 완전 쉽게 등록을 할 수 있는 것이죠 

## 3.3 소스 (controller))
```
@GetMapping("/MM/interceptorTest")
@ResponseBody
public String InteceptorTest1() { // 인터셉터 호출됨
	return "interceptorTest";
}
	
@GetMapping("/MM/excludePath")
@ResponseBody
public String InteceptorTest2() { // 호출되지 않음
	return "excludePath";
}
```
이렇게 작성을 하고 서버를 돌리게 되면 인터셉터로 인해서 위에는 System.out.print 가 호출되고 안되고를 볼 수 있습니다 