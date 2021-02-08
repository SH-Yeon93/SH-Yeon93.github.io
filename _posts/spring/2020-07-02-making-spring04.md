---
layout: post
title:  "[spring] java 파일 기반 componentScan"
subtitle:   "java componentScan"
date: 2020-07-02 11:27 +0900
categories: Spring
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---


## 1. 설명
앞선시간에는 xml 에 component - scan 으로 bean 을 scan 했다면 이번에는 마찬가지로 java 로 component - scan 을 만들어보겠습니다 


## 2. java 소스 (bean 설정파일)
```
package com.com.springBoot;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration // spring 설정파일
@ComponentScan(basePackageClasses = Application.class) // 이 클래스가 위치하는 패키지부터 연관된 모든 패키지를 bean 스캔함
public class ApplicationConfig {
	

}

```
이곳에서 제일 중요한것이 @Configuration , @ComponentScan 애노테이션이다 이 애노테이션의 역활은 
@Configuration 은 이 class 는 spring 설정 파일이라는 것을 명시해주는 애노테이션입니다 
@ComponentScan 은 마찬가지로 bean 스캔 범위를 설정하는데 이때는 Applcation.class 에 포함된 클래스 부터 하위 모든 클래스를 뜻한다는 것입니다 


## 2.1 java 소스 (service)
```
package com.com.springBoot;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class BookService {
	
	@Autowired
	private BookRePository bookRePository;

	public BookRePository getBookRePository() {
		return bookRePository;
	}

	public void setBookRePository(BookRePository bookRePository) {
		this.bookRePository = bookRePository;
	}
}


```

​

## 2.2 java 소스 (repository)
```
package com.com.springBoot;

import org.springframework.stereotype.Repository;

@Repository
public class BookRePository {

}



```

## 2.3 java 소스 (main)
```
package com.com.springBoot;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;


public class Application {

	public static void main(String[] args) {
		//ApplicationContext context = new ClassPathXmlApplicationContext("classpath:/application.xml");
		ApplicationContext context = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		BookService bookService = context.getBean("bookService" , BookService.class);
		System.out.println(bookService);
		BookRePository bookRePository = bookService.getBookRePository();
		System.out.println(bookRePository);

	}

}

```
이렇게 되면 이제 xml , java 모든 방법으로 bean 을 직접 정의하거나 bean 을 주입하는 방법을 해보았습니다 
