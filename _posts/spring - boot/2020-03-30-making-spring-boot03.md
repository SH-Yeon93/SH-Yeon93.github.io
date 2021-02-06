---
layout: post
title:  "[SpringBoot] 스프링 Boot 파일 업로드"
subtitle:   "spring boot file upload"
date: 2020-03-30. 18:56 +0900
categories: SpringBoot
tags: githubpage
comments: true
#related_posts:
#      - category/_posts/spring-boot/2020-03-10-making-spring-boot02.md
#     - category/_posts/study/2020-12-26-making-blog-03.md
---

## 1. 설명
spring boot 에서는 파일을 업로드 할때 특별한 maven 을 설치 하지 않아도 된다 그리고 boot 자체적으로 

설정이 기본적으로 되어 있기 때문에 가장 기본적인 설정을 필요가 없다 물른 커스텀에 따라서 일부 변경을 할 수 있겧지만 오늘은 boot 에서 파일을 업로드 하는 방법을 알아보자 



## 2. 소스(Handler)

```
@GetMapping("/file")
public String goFileUpload() { //파일 업로드 페이지로 이동
	
	return "/file/index";
}

@PostMapping("/uploadFile")
public void uploadFile(@RequestParam("file") List<MultipartFile> files, RedirectAttributes attributes) throws IOException {

//파일 2개 이상을 업로드 할때는 List형태의 MultipartFile 로 받아줘야 합니다 그렇지 않으면 마지막에 들어간 파일만 출력이 됩니다 
//파일 단 1개만 업로드 할때는 그냥 List 없이 선언해주시면됩니다 	
	
	for(int i = 0; i < files.size(); i++) {
		MultipartFile file2 = files.get(i);
		String orignalFileName = file2.getOriginalFilename();
		System.out.println(orignalFileName);			
		byte [] getBytes = file2.getBytes();
		
	}
	
}

```
2개의 핸들러를 만들었습니다 하나는 file 업로드 하는 페이지로 이동하는 핸들러 
다른 하나는 파일을 업로드 하는 핸들러입니다 



## 2.1 소스 (화면 )

```
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
<meta charset="UTF-8">
<title>Insert title here</title>
</head>
<body>
	<form method="POST" enctype="multipart/form-data" action="#" th:action="@{/uploadFile}">
		File: <input type="file" multiple="multiple" name="file">
		<output id="list"></output>
		<input type="submit" value="Upload"/>
	</form>
	

</body>
</html>

```
이때 2개이상의 파일을 업로드 할때는 multiple 를 반드시 넣어주셔야 합니다 그렇지 않으면 파일을 여러개를 선택한다고 해도 1개의 파일만 넘어아게 됩니다 
