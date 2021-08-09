---
layout: post
title: "Angular Components"
subtitle: "[Angular 02] Angular Components 개념 알아보고 만들기"
date: 2021-07-29
# background: '/img/posts/03.jpg'
categories:
- angular
lang:
- Korean
- English
tags:
- ko
permalink: ko/:categories/:title
toc:
- Components-이란
- 좋은-점
- 코드-예시
---

## Components 이란
***
Components 이란 템플렛 뷰 (html과 css)와 비지니스 로직 (TypeScript)이 합쳐진 것 입니다. 각각의 Component는 웹페이지에 보여질 작은 화면들을 만들어 내고, 만들어진 Component들이 모여서 하나의 웹페이지를 브라우져를 통하여 보여주게 됩니다. 

<!-- 예시로 아마존 웹사이트를 보도록 하겠습니다. 아마 아마존이 Angular를 사용하지는 않겠지만 좋은 Component 예시라서 아마존 웹사이트로 보여드립니다.   -->
<br>
<br>

## 좋은 점
***
1. 로직과 스타일을 다시 사용하기 편하다  
2. 스크린을 파트 별로 나누기 좋다  
3. 병합하기 편리하다  
4. Component를 새로 만들거나 업데이트하기 쉽다  
<br>
<br>

## 코드 예시
***
Goal: Component를 만들어 보고 browser에서 확인하기!
<br>

Step 1: 먼저 새로운 프로젝트를 만들도록 하겠습니다. Terminal를 열어서 프로젝트를 만들고 싶은 곳에 아래 커맨트를 입력합니다.  
~~~~
$ ng new componentDemo
~~~~
<br>

<h6>Step 2:</h6> 

![image](https://user-images.githubusercontent.com/44415731/127566380-1daac241-b8fd-4998-bebc-8207a12c2080.png){: width="100%" height="100%"}{: .align-center}  
![image](https://user-images.githubusercontent.com/44415731/127567190-afec7ad3-3c57-4cb0-b8e6-de2ca36704a5.png){: width="100%" height="100%"}{: .align-center}  

- y를 입력과 엔터 후, CSS를 선택하고 엔터를 눌러주세요.

<br>

<h6>Step 3:</h6> <p style="margin-top:0px">방금 만든 프로젝트의 app 디렉토리로 들어가서 my-first-component 만들도록 하겠습니다.</p>  
```
$ cd ./componentDemo/src/app
$ ng generate component my-first-component
```
***Note: Component 단축 커멘드***
~~~~
$ ng g c my-first-component
~~~~
<br>

<h6>Step 4:</h6> 4개의 파일들이 만들어지고 1개의 파일을 업데이트 합니다.

![image](https://user-images.githubusercontent.com/44415731/127567976-706cb96f-89b6-4577-b6f1-7eec5859c333.png){: width="100%" height="100%"}{: .align-center}  
- y로 routing 선택 후, CSS 선택

![image](https://user-images.githubusercontent.com/44415731/127569468-d28cc7e1-c714-4dbc-a342-b79379f31957.png){: width="100%" height="100%"}{: .align-center}  
- 만들어진 4개의 파일은 새로운 component의 파트에 추가 된 것이고,
  - my-first-component.ts: component의 로직 파일
  - my-first-component.css: component의 스타일 파일
  - my-first-component.html: component의 템플릿 파일 
- 업데이트 된 1개의 파일은 app.moudle.ts에 새로운 component가 import 된 것을 알려준 것 입니다.
<br>

<h6>Step 5:</h6> Component의 Selector 확인하기

![image](https://user-images.githubusercontent.com/44415731/127720479-d6979ab2-e811-49db-a312-88016b660d95.png){: width="100%" height="100%"}{: .align-center} 
- src/app 디렉토리에 들어가서 my-first-component.ts의 selector를 확인합니다.
- Selector를 html tag처럼 이용하여 component를 html 파일에 넣을 수 있습니다.
  - \<app-{name_of_selector}>\</app-{name_of_selector}>
  - 예시: \<app-my-first-component>\</app-my-first-component>
<br>

<h6>Step 6:</h6> Component를 브라우저에 띄우기

![image](https://user-images.githubusercontent.com/44415731/127720983-6e117662-1115-483d-875b-185731f0a402.png){: width="100%" height="100%"}{: .align-center}  
- src > app > app.component.html 안에 있는 모든 코드를 지우고 \<app-my-first-component>\</app-my-first-component>를 씁니다.
- 다 저장한 후 terminal에서 아래 커멘트를 실행합니다.
~~~~
$ ng serve --open
~~~~
<br>

<h6>Step 7:</h6> Component 브라우져에서 확인하기

![image](https://user-images.githubusercontent.com/44415731/127721185-fd754c12-f5a2-4f30-992a-3c420f4deffc.png){: width="100%" height="100%"}{: .align-center}  
- 브라우져가 자동 실행되지 않았다면 localhost:4200를 주소창에 넣고 실행해주세요
- 만약 my-first-component works가 보여진다면 성공!
