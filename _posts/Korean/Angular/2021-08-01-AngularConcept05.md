---
layout: post
title: "Angular Directives"
subtitle: "[Angular 05] Angular Component Directives 노트와 Structural Directives Examples"
date: 2021-08-01
# background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
---

## Directives 이란
***
- 텟플릿을 레더링할 때 directives의 정해진 룰로 정보를 보여준다
- 만약 '1000'를 달러로 보여주고 싶을때 directive를 이용하여 '$1000'을 보여줄 수 있다.

<br>

## Types of Directives (4가지)
***
- Components
  - Component은 텟플릿이 있는 Directive이다
- Structural Directives
  - DOM에서 컨디셔널 레더링이나 list를 for loop를 이용해 구현할 수 있게 도아준다
  - *ngIf, [ngSwitch], *ngFor
- Attribute Directives
  - HTML attributes를 Directive으로 할당한다.
- Custom Directives

<br>

## 코드 예시
***
Goal: *ngIf 사용해 보기
준비물: 새 프로젝트와 Component 하나

<br>

### *ngIf, *ngFor, [ngSwitch]
***
<h6>Step 1:</h6> 

~~~~typescript
import { Component, EventEmitter, OnInit, Output } from '@angular/core';

@Component({
  selector: 'app-my-first-component',
  templateUrl: './my-first-component.component.html',
  styleUrls: ['./my-first-component.component.css']
})
export class MyFirstComponentComponent implements OnInit {

  public item1!:string;

  constructor() { }

  ngOnInit(): void {
    this.item1 = "Item One"
  }

}
~~~~
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 2:</h6> 

~~~~html
<div *ngIf="item1">
    <p>Item One Exist</p>
</div>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- 만약 "Item One Exist"이 보인다면 성공!
<br>


### *ngIf (#truecodition과 #falsecondition 이용하기)
***
<h6>Step 1:</h6> 

~~~~html
<div *ngIf="item1 === 'Item One'; then trueCondition; else falseCondition">
</div>

<ng-template #trueCondition>
    <p>Item One Exist</p>
</ng-template>

<ng-template #falseCondition>
    <p>No item found</p>
</ng-template>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
<br>

### *ngFor
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

  public itemList!: any[];

  constructor() { }

  ngOnInit(): void {
    this.itemList = [
      {"name": "Lamp", "color": "Gold"},
      {"name": "Book", "color": "Blue"},
      {"name": "Kite", "color": "Red"},
      {"name": "Pear", "color": "Green"}
    ]
  }
}
~~~~ 
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 2:</h6>
~~~~html
<ul *ngFor="let item of itemList">
    <li>
        {{item.name}} - {{item.color}}
    </li>
</ul>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127756069-5fa19530-a367-4b53-b9f6-a323f1913010.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
<br>  

### ngSwitch
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

  public itemList!: any[];

  constructor() { }

  ngOnInit(): void {
    this.itemList = [
      {"name": "Lamp", "color": "Gold"},
      {"name": "Book", "color": "Blue"},
      {"name": "Kite", "color": "Red"},
      {"name": "Pear", "color": "Green"}
    ]
  }
}
~~~~ 
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 2:</h6>
~~~~html
<ul *ngFor="let item of itemList" [ngSwitch]="item.name">
    <li *ngSwitchCase="'Lamp'">
        This Lamp is {{item.color}}
    </li>
    <li *ngSwitchCase="'Book'">
        This Book is {{item.color}}
    </li>
    <li *ngSwitchCase="'Kite'">
        This Kite is {{item.color}}
    </li>
    <li *ngSwitchDefault>
        Item unknown
    </li>
</ul>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127756162-427033ec-54d1-4e40-8190-7771dd099828.png){: width="60%" height="60%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
<br>  

***Attribute Directives는 다음 포스트에서***