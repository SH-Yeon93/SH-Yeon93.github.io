---
layout: post
title:  "[spring] java 파일로 bean 설정"
subtitle:   "spring bean"
date: 2020-07-02 11:20 +0900
categories: Spring
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---


## 1. 설명
앞선 시간에는 xml 로 bean 을 생성하거나 component - scan 으로  bean 을 검색했다면 이번에는 java 파일로 bean 을 생성을 해볼려고 합니다 


## 2. java 소스 (main)
```
package com.com.springBoot;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;


public class Application {

	public static void main(String[] args) {
		ApplicationContext context = new AnnotationConfigApplicationContext(ApplicationConfig.class);
		BookService bookService = context.getBean("bookService" , BookService.class);
		System.out.println(bookService);
		BookRePository bookRePository = bookService.getBookRePository();
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

​

## 2.2 java 소스 (repository)
```
package com.com.springBoot;

public class BookRePository {

}


```

## 2.3 java 소스 (configuration)
```
package com.com.springBoot;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration // spring 설정파일
public class ApplicationConfig {
	
	@Bean
	public BookRePository bookRepository() {
		BookRePository bookRePository = new BookRePository();
		return bookRePository;
	}
	
	@Bean
	public BookService bookService() {
		BookService bookService = new BookService();
		bookService.setBookRePository(bookRepository());
		return bookService;
	}

}


```
이 파일이 spring bean 설정 파일입니다 이 파일 안에서 bean 을 정의하고 IOC 컨테이너는 bean 정의서를 바탕으로 생성하고 주입합니다 
즉 제일 첫장에서 xml 로 bean 을 주입하는 방법을 java 로 옮긴것입니다 
