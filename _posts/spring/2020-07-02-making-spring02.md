---
layout: post
title:  "[spring] component - scan 과 애노테이션 기반 bean 생성"
subtitle:   "component - scan"
date: 2020-07-02 11:18 +0900
categories: Spring
tags: githubpage
comments: true
# related_posts:
#     - category/_posts/study/2020-12-26-making-blog-02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---


## 1. 설명
앞시간에서 고전적인 방법으로 bean 을 주입하는 방법을 알아봤는데 이번에는 spring 이 제공하는 

애노테이션을 사용해서 bean 을 주입하는 방법을 사용해보면 


## 2.xml 소스 bean 주입
```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.1.xsd">
		
		<context:component-scan base-package="com.com.springBoot"/>
		
</beans>

```

## 3.component-scan 
```
<context:component-scan base-package="com.com.springBoot"/>

```
이 태그에 주목을 해보자 이 태그는 관련된 패키지에서 특정한 알고리즘으로 bean을 스캔하여 어떤 bean 은 IOC 컨테이너에 올리고 어떤 bean 은 올리지 않는다 
하지만 개발자가 사용하는 거의 대부분의 bean은 올라가니 이 특정한 알고리즘에 대해서 이해할 필요는 없다 
물론 특정한 bean 자체를 검색 못하게 막을 수도 있다 

​


## 4 java 소스 (service)
```
package com.com.springBoot;

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

## 4.2 java 소스 (repository)
```
package com.com.springBoot;

@Repository
public class BookRePository {

}

```

여기서 중요한것은 @Repository , @Service 애노테이션이다 이 애노테이션은 위에서 bean -scan 에 감지되는 애노테이션으로 이 타입의 bean 들은 검색이 되어서 
Ioc 컨테이너에 들어가게 된다 


## 5 AutoWire 사용방법
```
@Autowired
private BookRePository bookRePository;

```
그리고 Ioc 에 생성된 bean 을 특정 타입에 주입하는 방법은 이렇게 @Autowire 라는 애노테이션을 사용해서 bean 을 주입한다 

