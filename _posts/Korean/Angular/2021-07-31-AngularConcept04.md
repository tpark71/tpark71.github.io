---
layout: post
title: "Angular Component Interaction"
subtitle: "[Angular 04] Angular Component Interaction 노트"
date: 2021-07-31
# background: '/img/posts/03.jpg'
categories:
- angular
lang:
- Korean
- English
tags:
- ko
permalink: ko/:categories/:title
---

## Intro
***
- Component들끼리 데이터를 주고 받을 수 있는 방법을 정리해 보자!

<br>

## Component Interaction 종류 (4가지)
***
- @Input Decorator
  - 부모(Parent Component)에서 아이(Child Component)로 데이터를 옮길 때 사용
- @Output Decorator
  - 아이(Child Component)에서 부모(Parent Component)로 데이터를 옮길 때 사용
- Services
  - 부모-아이 관계(Parent-Child Relation)이 없을 때 Angular Service를 사용
- Routes
  - 부모-아이 관계(Parent-Child Relation)이 없을 때 Route를 이용하여 다른 component으로 화면 전환

<br>

## 코드 예시
***
Goal: Input()과 Output() Interaction를 만들어 보자! (Services랑 Routes는 나중에!)
(저번에 만든 component 사용하자!)

<br>

### @Input (Parent -> Child)
***
<h6>Step 1:</h6> 

~~~~typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'componentDemo';
  public data = 'Give this string to your child!'
}
~~~~
- App ts파일을 연다 (src > app > app.component.ts)
<br>

<h6>Step 2:</h6> 

~~~~html
<app-my-first-component [gift]="data"></app-my-first-component>
~~~~
- App의 html 파일을 연다 (src > app > app.component.html)
<br>

<h6>Step 3:</h6> 

~~~~typescript
import { Component, Input, OnInit } from '@angular/core';

@Component({
  selector: 'app-my-first-component',
  templateUrl: './my-first-component.component.html',
  styleUrls: ['./my-first-component.component.css']
})
export class MyFirstComponentComponent implements OnInit {

  @Input() public gift = "";

  constructor() { }

  ngOnInit(): void {
  }

}
~~~~
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 4:</h6> 

~~~~html
<p>My gift from parent {{gift}}</p>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
<br>

<h6>Result</h6>

~~~~
My gift from parent Give this string to your child!
~~~~ 
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 위의 문장이 보인다면 성공!
<br>

### @Output (Child -> Parenet)
***
<br>

<h6>Step 1:</h6>

~~~~typescript
import { Component, EventEmitter, OnInit, Output } from '@angular/core';

@Component({
  selector: 'app-my-first-component',
  templateUrl: './my-first-component.component.html',
  styleUrls: ['./my-first-component.component.css']
})
export class MyFirstComponentComponent implements OnInit {

  @Output() childEvent=new EventEmitter<string>();

  constructor() { }

  ngOnInit(): void {
  }
  onPressed() {
    this.childEvent.emit("A letter from child")
  }

}
~~~~ 
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 2:</h6>
~~~~html
<button (click)="onPressed()"> Click Me! </button>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Step 3:</h6>

~~~~typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'componentDemo';
  public letter = ''
}
~~~~
- App ts파일을 연다 (src > app > app.component.ts)
<br>

<h6>Step 4:</h6>
~~~~html
<input type="button" value="Click Me" (click)="onClick()"/>
~~~~
- App html를 연다 (src > app > app.component.html) 
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127754953-bddc377f-6246-48ff-a43e-9b10bf10a6d2.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 Click Me 버튼을 누렀을 때 "A letter from child"라고 뜬다면 성공!  
<br>  