---
layout: post
title:  "[spring] xml 로 bean 주입하기 "
subtitle:   "xml bean injection"
date: 2020-07-02 10:51 +0900
categories: Spring
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---


## 1. 설명
먼저 고전적으로 bean 을 등록하는 방법을 알아보자 이때 bean 은 Spring Ioc 컨테이너가 관리하는 객체이다 

그런 객체는 기본적으로는 개발자가 만들어주는것이 원칙 그 이후에는 spring이 알아서 bean 을 관리해주고 

개발자는 그런 bean 을 가져다 쓰기만 하면된다 


## 2.java 소스 (main 소스)
```
package com.com.springBoot;

import org.springframework.boot.SpringApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;


public class Application {

	public static void main(String[] args) {
		ApplicationContext context = new ClassPathXmlApplicationContext("classpath:/application.xml");
		BookService service = context.getBean("bookService" , BookService.class);
		System.out.println(service);
		BookRePository bookRePository = service.getBookRePository();
		System.out.println(bookRePository);
		
	}

}

```

## 2.1 java 소스 (service)
```
package com.com.springBoot;

public class BookService {
	
	private BookRePository bookRePository;

	public BookRePository getBookRePository() {
		return bookRePository;
	}

	public void setBookRePository(BookRePository bookRePository) {
		this.bookRePository = bookRePository;
	}
}

```

## 2.2 java 소스 (repository)
```
package com.com.springBoot;

public class BookRePository {

}

```


## 3.bean 설정 

```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id = "bookService" class = "com.com.springBoot.BookService">
		<property name="bookRePository" ref="bookRepository"></property>
	</bean>
	
	<bean id = "bookRepository" class = "com.com.springBoot.BookRePository"/>
	
		
</beans>
```    

bookService 에서 bookrepository 라는 bean 을 주입받기 위해서 setter 를 사용했고 그 setter 을 주입하기 위해서 xml 로 작성을 해둔것이다 

​
bean 의 생성은

ApplicationContext context = new ClassPathXmlApplicationContext("classpath:/application.xml");

로 해당 xml 을 읽어서 해당 bean 을 주입을 하고 

만약 이 과정에서 bean 이 주입이 되지 않았다면 bean 을 주입받지 못한 

```
System.out.println(service);
BookRePository bookRePository = service.getBookRePository();
System.out.println(bookRePository);

```
이들은 모두 null 값이 출력이 될것이다 


