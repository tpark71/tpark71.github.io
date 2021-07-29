---
layout: post
title: "[Angular Concept 02] Angular Components 만들기"
subtitle: "Angular Components 개념 알아보고 만들어 보기"
date: 2021-07-27
background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
- English
---

## Components 이란
***
Components 이란 템플렛 뷰 (html과 css)와 비지니스 로직 (TypeScript)이 합쳐진 것 입니다. 각각의 Component는 웹페이지에 보여질 작은 화면들을 만들어 내고, 만들어진 Component들이 모여서 하나의 웹페이지를 브라우져를 통하여 보여주게 됩니다. 

예시로 아마존 웹사이트를 보도록 하겠습니다. 아마 아마존이 Angular를 사용하지는 않겠지만 좋은 Component 예시라서 아마존 웹사이트로 보여드립니다.

## 좋은 점 
***
1. 로직과 스타일을 다시 사용하기 편하다
2. 스크린을 파트 별로 나누기 좋다
3. 병합하기 편리하다
4. Component를 새로 만들거나 업데이트하기 쉽다

## 코드 예시

먼저 새로운 프로젝트를 만들도록 하겠습니다. Terminal를 열어서 프로젝트를 만들고 싶은 곳에 아래 커맨트를 입력합니다.
~~~~
$ ng new componentDemo
~~~~

![image](https://user-images.githubusercontent.com/44415731/127566380-1daac241-b8fd-4998-bebc-8207a12c2080.png){: width="50%" height="50%"}{: .align-center}
y를 입력하려 routing를 만들도록 하겠습니다. 오늘 데모에서는 필요없지만 나중에 routing에 대해서 노트를 쓸 때 정리하도록 하겠습니다.

![image](https://user-images.githubusercontent.com/44415731/127567190-afec7ad3-3c57-4cb0-b8e6-de2ca36704a5.png){: width="50%" height="50%"}{: .align-center}
CSS를 선택하고 엔터를 하겠습니다.

그 다음 방금 만든 프로젝트의 app 디렉토리로 들어가서 my-first-component 만들도록 하겠습니다.
~~~~
$ cd ./componentDemo/src/app
$ ng generate component my-first-component
~~~~

***Note: Component 만들때 단축 된 커멘드***
~~~~
$ ng g c my-first-component
~~~~

![image](https://user-images.githubusercontent.com/44415731/127567976-706cb96f-89b6-4577-b6f1-7eec5859c333.png){: width="50%" height="50%"}{: .align-center}  
그럼 이렇게 4개의 파일들을 만들고 1개의 파일을 업데이트 했다고 알려줍니다.

![image](https://user-images.githubusercontent.com/44415731/127569468-d28cc7e1-c714-4dbc-a342-b79379f31957.png){: width="50%" height="50%"}{: .align-center}  
- 만들어진 4개의 파일은 새로운 component의 파트에 추가 된 것이고,
- 업데이트 된 1개의 파일은 app.moudle.ts에 새로운 component가 import 된 것을 알려준 것 입니다.
