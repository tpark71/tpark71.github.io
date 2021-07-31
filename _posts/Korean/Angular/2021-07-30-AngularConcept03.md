---
layout: post
title: "[Angular Concept 03] Angular Data Binding"
subtitle: "Angular Data Binding으로 개념 데이터 연동하기"
date: 2021-07-30
# background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
---

## Data Binding 이란
***
- 데이터 바이딩이란 Typescript에 있는 비지니스 로직과 HTML 템플릿에 있는 Component들의 데이터들을 연결시켜주는 것 입니다.
- 만약 영화 데이터베이스 웹사이트라면, Typescript가 영화들을 Data Binding를 통해 HTML 템플릿 위에 영화들 그립니다.

<br>

## Data Binding 종류 (2가지)
***
- One-Way Binding
  - 데이터를 한쪽으로만 전해줍니다.
  - Typescript -> HTML or HTML -> Typescript
- Two-way Binding
  - 데이터를 쌍방향으로 교환하여 HTML과 Typescript의 데이터들이 언제나 연동됩니다.
  - Typescript <-> HTML

<br>

## Data Binding 하는 법 (4가지)
***
<table>
    <th>
        <td>종류</td>
        <td>Syntax</td>
        <td>데이터 이동 방향</td>
        <td></td>
    </th>
    <tr>
        <td>String Interpolation</td>
        <td>{{ data }}</td>
        <td>Component -> DOM</td>
        <td>간단한 String를 보낼 때 사용</td>
    </tr>
    <tr>
        <td>Property Binding</td>
        <td>[property] = "data"</td>
        <td>Component -> DOM</td>
        <td>HTML의 인라인 속성 바꿀 때 사용</td>
    </tr>
    <tr>
        <td>Event Binding</td>
        <td>(event) = "function()"</td>
        <td>DOM -> Component</td>
        <td>HTML의 event 발생시 발동하는 function를 할당 할때 사용</td>
    </tr>
    <tr>
        <td>2-Way Data Binding</td>
        <td>[(ngModel)}</td>
        <td>DOM <-> Component</td>
        <td>DOM과 Component의 데이터가 연동 되어야 할때 사용</td>
    </tr>
</table>

<br>

## 코드 예시
***
Goal: 4 종류의 데이터 바이딩 만들어 보자!  
(저번에 만든 component 사용하자!)

<br>

### String Interpolation
***
<h6>Step 1:</h6> 

![image](https://user-images.githubusercontent.com/44415731/127726013-6b2d770f-67f5-47b4-8a8f-61788c44a2ae.png){: width="100%" height="100%"}{: .align-center}  
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
- 라인 10번처럼 data라는 변수를 만든다
- 라인 15번처럼 data에 데이터를 onOnInit() 안에서 할당해 준다
<br>

<h6>Step 2:</h6> 

![image](https://user-images.githubusercontent.com/44415731/127726048-7dfd8640-31de-4665-8418-4ead27c4039f.png){: width="100%" height="100%"}{: .align-center}  
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 사진과 같이 {{}} 안에 data라는 변수를 넣어준다.
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127726118-a46af02b-58b2-496d-a65f-817d8e802854.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 Hello World가 보인다면 성공!
<br>

### Property Binding
***
데이터 바인딩을 이용해서 input tag에 버튼 타입을 할당하자!
<br>

<h6>Step 1:</h6>

![image](https://user-images.githubusercontent.com/44415731/127726539-93e6c190-a16b-4396-9db1-5252c7b993ed.png){: width="100%" height="100%"}{: .align-center}  
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
- 라인 10번처럼 data라는 변수를 만든다
- 라인 15번처럼 data에 데이터를 onOnInit() 안에서 할당해 준다
<br>

<h6>Step 2:</h6>
~~~~html
<input [type]="attr" value="Click Me"/>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127726586-a3db0fa2-8fea-4af9-b964-52d28a562c92.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 Click Me 버튼이 보인다면 성공!
<br>

### Event Binding
***
데이터 바인딩을 이용해서 버튼에 함수를 할당시키자!
<br>

<h6>Step 1:</h6>

![image](https://user-images.githubusercontent.com/44415731/127726695-0fe1859f-48ca-4cb1-9e36-a5f3734a52d7.png){: width="100%" height="100%"}{: .align-center}  
- Component의 ts 파일을 연다 (src > app > my-first-component >my-first-component.component.ts)
- 라인 15-17번처럼 onClick()이라는 함수를 만들어 준다
<br>

<h6>Step 2:</h6>
~~~~html
<input type="button" value="Click Me" (click)="onClick()"/>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127726736-32be39a6-d12a-4bd5-a38c-820cfb2efbf1.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 Click Me 버튼을 누렀을 때 "button clicked!라고 뜬다면 성공!
<br>

### 2-Way Data Binding
***
데이터 바인딩을 이용해서 ts과 html를 연동시키자!
<br>

<h6>Step 1:</h6>

![image](https://user-images.githubusercontent.com/44415731/127727040-3284249f-a146-487c-bfcb-c19ed69a99ae.png){: width="100%" height="100%"}{: .align-center}  
- App Module 파일을 연다 (src > app > app.module.ts)
- 라인 2번에 "FormsModule" 인포트 해준다.
- 라인 17번에 "FormsModule" 추가해준다 (주의: 라인 16번뒤에 반점 까먹으면 안됨!)
<br>

<h6>Step 2:</h6>

![image](https://user-images.githubusercontent.com/44415731/127727164-55343698-e699-4f86-bcdf-28fdb0d7afa5.png){: width="100%" height="100%"}{: .align-center}  
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
- <code>data</code> 변수를 만들고 <code>ngOnInit()</code>에 <code>this.data="";</code> 를 추가해준다.
<br>

<h6>Step 3:</h6>
```html
<p>{% raw %}{{ data }}{% endraw %}</p>
<input type="text" [(ngModel)]="data" />
```
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127726991-43d82473-173c-43b2-ba0a-2ea39865b91a.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 input 박스에 쓴 글이 바로 위에 나타난다면 성공!
<br>
