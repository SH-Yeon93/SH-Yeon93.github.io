---
layout: post
title:  "[SpringBoot] @SpringBootApplication"
subtitle:   "@SpringBootApplication"
date: 2020-02-28 10:30 +0900
categories: SpringBoot
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---

## 1. 설명
SpringBoot 에서 가장 기본적인 애노테이션이다 그 원리를 알아보자 


## 2. 의존성

```
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-thymeleaf</artifactId>		
</dependency>
	
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

기본적인 디펜더시 입니다 이것을 pom.xml 에 넣어도 되지만 프로젝트 만들때 설정할 수 있습니다 

## 3. 소스 (main)

```
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class MySpringBootMvcApplication {

	public static void main(String[] args) {
		SpringApplication.run(MySpringBootMvcApplication.class, args);
	}

}

```

부트 프로젝트를 만들면 제일 먼저 보이는 페이지입니다 이전의 mvc를 만들면 제일 먼저 보이는것이 

HomeController 이었는데 지금은 이 페이지가 먼저 보이게 됩니다 

그리고 기존에 있었던 bean 을 설정할 수 있는 xml 파일도 안보입니다 boot 프로젝트는 기본적으로 xml 을 통해서 bean 을 설정하지 않습니다 오로지 java @Configuration 으로 bean 을 설정을 합니다 

​

제일 처음에 보이는 이 애노테이션에 대해서 알아보자면 모든 스프링 boot 프로젝트는 이 프로젝트를 기반으로 

동작을 하게 됩니다 


그러면 간단하게 컨트롤러를 작성해보겠습니다 


## 3.1 소스 (controller)
```
package com.example.demo.controller;

import java.text.DateFormat;
import java.util.Date;
import java.util.Locale;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {
	
	@GetMapping("/home") // @RequestMapping(method = RequestMethod.GET) 새롭게 정의된 애노테이션 컴포지드 애노테이션
	public String home(Model model , Locale locale) {
		
		Date date = new Date();
		DateFormat dateFormat = DateFormat.getDateTimeInstance(DateFormat.LONG, DateFormat.LONG, locale);
		
		String formattedDate = dateFormat.format(date);
		
		model.addAttribute("localTime", formattedDate );
		
		return "/view/home"; //기본적인 view 의 위치는 root/templates/ 입니다 
		
		
	}
```

## 3.2 소스 (html)
```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>

	<P th:text="${localTime}">localTime</P> //타임리프 엔진에서 값을 꺼내는 방법 

</body>
</html>
```

실행을 하면 우리가 처음 mvc를 만들었을때 화면이 나오게 됩니다  우리의 현재 시간이 나오는 화면을 볼 수 있습니다 

​
그러면 우리는 이제까지 mvc를 해온사람으로서 조금 의문이 들 수 있습니다 

xml 파일에서 기본적으로 설정해야 하는것도 없고 특히나 bean 을 자동으로 찾아주는 component - scan 도 안보이는데 어떻게 mvc와 관련된 bean 애노테이션을 감지하고 보여주느냐

## 3.3 소스 (어노테이션)
```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(excludeFilters = { @Filter(type = FilterType.CUSTOM, classes = TypeExcludeFilter.class),
@Filter(type = FilterType.CUSTOM, classes = AutoConfigurationExcludeFilter.class) })
public @interface SpringBootApplication {


}
```

이 애노테이션 또한 여러 애노테이션을 기반으로 조합된 애노테이션이라고 해서 컴포지드 애노테이션이라고 합니다 

그런데 여기 보면 우리가 익숙하게 본 @ComponentScan 이 보입니다 즉 이 애노테이션의 존재로 

xml 의 어떤 설정도 없이 우리는 그냥 바로 bean 설정에 관련된 애노테이션을 사용할 수 있게 되는 것입니다 

​
필터가 걸려 있긴 한데  이 필터의 역활은 모든 bean 을 전부 스캔하는 것은 아니라 특정 정책에 따라서 bean 을 만들것이냐 만들지 않을것이냐가 갈리게 됩니다 

​
저 필터 정책이 무엇이냐면 한가지로 예를 들어서 

그리고 아까 컨트롤러를 만들때 반드시 이 MySpringBootMvcApplication 클래스가 포함된 패키지에 컨트롤러를 만들었습니다 하지만 이 클래스가 속하지 않은 패키지에 다른 곳에서 컨트롤러를 만든다면 bean 은 절대로 생겨나지 않습니다 즉 이 필터정책중 하나가 자신이 속한 패키지 안에서만 bean 을 만들고 있는 것입니다 

