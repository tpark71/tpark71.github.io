---
layout: post
title: "[Angular 06] Angular Directives (Part 2)"
subtitle: "Angular Component Directives 노트와 Structural Directives Examples"
date: 2021-08-02
# background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
---

## Intro
***
- Attribute Directives 예시
  - \[ngClass\]: add or remove css class
  - \[ngStyle\]: styles html element

<br>

## 코드 예시
***
Goal: [ngClass], [ngStyle] 사용해 보자!
준비물: 새 프로젝트와 Component 하나

<br>

### [ngClass]
***
<h6>Step 1:</h6>

<div class="code-header">src > app > my-first-component > my-first-component.component.ts</div>
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

~~~~css
.color-red {
    color:red
}

.color-yellow {
    color:yellow
}

.color-blue {
    color:blue
}
~~~~
- Component의 css 파일을 연다 (src > app > my-first-component > my-first-component.component.css)
<br>

<h6>Step 3:</h6> 

~~~~html
<ul *ngFor="let item of itemList">
    <li [ngClass]="{
        'color-red':item.name === 'Lamp',
        'color-yellow':item.name === 'Book',
        'color-blue':item.name === 'Kite'
    }">Item Color</li>
</ul>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/127756471-d0d253b1-718d-4587-896d-0c80978d24ab.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
<br>  

### ngStyle
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

  public color!:string;
  public size!:number;

  constructor() { }

  ngOnInit(): void {
    this.color = "blue";
    this.size = 30;
  }
}
~~~~
- Component의 ts 파일을 연다 (src > app > my-first-component > my-first-component.component.ts)
<br>

<h6>Step 2:</h6>
~~~~html
<div [ngStyle]="{'color':color, 'font-size': size + 'px'}">This is ngStyle</div>
<button (click)="size = size + 1">+</button>
<button (click)="size = size - 1">-</button>
~~~~
- Component의 html 파일을 연다 (src > app > my-first-component > my-first-component.component.html)
- 위의 코드를 html 파일에 넣는다.  
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/128107109-4cf9ae49-e27f-4e1f-9f73-6b0959479140.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- '+' 누르면 커지고 '-' 누르면 작아진다면 성공!
<br>  

***Custom Directives는 다음 포스트에서***