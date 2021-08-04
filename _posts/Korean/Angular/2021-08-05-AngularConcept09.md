---
layout: post
title: "Angular Forms"
subtitle: "[Angular Basic 09] Template-Driven and Reactive Forms"
date: 2021-08-05
# background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
---

## Form
***
- Html에서 유저가 입력한 input를 server로 받아올때 form를 사용합니다.

## Types of Angular Forms (2 types)

<table class="table">
    <tr>
        <th scope="col"></th>
        <th scope="col">Template-Driven Form</th>
        <th scope="col">Reactive Form</th>
    </tr>
    <tr>
        <th scope="row">Setup</th>
        <td>By component</td>
        <td>By directives</td>
    </tr>
    <tr>
        <th scope="row">Data Model</th>
        <td>Structured</td>
        <td>Unstructed</td>
    </tr>
    <tr>
         <th scope="row">Predictability</th>
        <td>Synchronous</td>
        <td>Asychronous</td>
    </tr>
    <tr>
        <th scope="row">Form Validation</th>
        <td>Functions</td>
        <td>Directives</td>
    </tr>
    <tr>
        <th scope="row">Mutability</th>
        <td>Immutable</td>
        <td>Mutable</td>
    </tr>
</table>

## Types of Form Building Blocks (4 types)
- FormControl
  - 각 'form-control'의 value와 validation를 트랙함.
- FormGroup
  - form controls 그룹의 value와 validation를 트랙함.
- FormArray
  - Array의 form control의 value와 validation를 트랙함.
- ControlValueAccessor
  - native DOM element과 Angular FormControl element들을 연결시켜줌.
  
<br>

## Template Driven Form Directives (3 directives)
- ngForm
  - 모든 form의 담당자
- ngModel
  - 각 form의 필드 데이터 담당자
- ngModelGroup

<br>

## Types of Form Model for Reactive Form (3 types)
- FormControl
  - 각 form element를 담당.
- FormGroup
  - FormControl들을 담당
- FormBuilder
  - Form를 만들어 줌 (Validation를 적용하는 곳)

<br>

## Form Data Functions
- reset()
  - Remove all values from the form 
- setValue()
  - Set all form to some value
- patchValue()
  - Update selected properties of a form model

## 코드 예시
***
Goal: Custom Pipe를 만들어 보자!
준비물: 새 프로젝트와 Component 하나

<br>

<h6>Step 1: Open Terminal</h6>

<div class="code-header">terminal</div>
~~~~bash
# inside of your project folder
mkdir src/app/pipes
cd src/app/pipes
# ng generate pipe <custom_pipe_name>
ng generate pipe custompipe
~~~~ 
- pipes라는 폴더를 만든다
- pipes라는 폴더 안에 custompipe 이라는 pipe를 만든다
<br>

<h6>Step 2: App Component에 리스트 만들기</h6> 

<div class="code-header">src > app > app.component.ts</div>
~~~~typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'componentDemo';

  shoppingList: any[] = [
    {
      "name": "Lamp",
      "price": 1000
    },
    {
      "name": "Book",
      "price": 2000
    },
    {
      "name": "Radio",
      "price": 3000
    },
    {
      "name": "Phone",
      "price": 4000
    }
  ]
}
~~~~
- shoppingList를 위와 같이 만든다.
<br>

<h6>Step 3: Custom Pipe 만들기</h6> 

<div class="code-header">src > app > pipes > custompipe.pipe.ts</div>
~~~~typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'custompipe'
})
export class CustompipePipe implements PipeTransform {

  transform(value:any, search:string): any {
    if(value.length === 0 || search === "" ) {
      return value
    }
    const result = []
    for (const element of value) {
      if (element.price > +search) {
        result.push(element)
      }
    }
    return result
  }

}
~~~~
- 해당 function를 만들어 준다
  - function = 유저가 입력한 숫자보다 큰 가격을 가진 아이템을 리턴하다
- 파이프 이름을 외운다
  - name: 'custompipe'
<br>

<h6>Step 4: App Html</h6> 

<div class="code-header">src > app > app.component.html</div>
~~~~html
Price Greater Than: <input type="text" [(ngModel)]="search">

<ul *ngFor="let product of shoppingList | custompipe:search">
    <li>{{product.name}} - {{product.price | currency:"USD"}}</li>
</ul>
~~~~
- Input Box에 숫자를 입력하면 해당 price보다 높은 아이템들을 보여준다
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/128118039-905ba8ed-9509-465b-a1c3-c25221a41425.png){: width="100%" height="100%"}{: .align-center}  
![image](https://user-images.githubusercontent.com/44415731/128118041-56993119-1a1c-4694-b21c-243ede01cf6b.png){: width="100%" height="100%"}{: .align-center}  
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.

<br>

### Extra (Pure Pipe and Impure Pipe)
***
- Pure Pipe
  - Pipe에 변화가 감지되었을 때 발동
- Impure Pipe
  - 다른 Component가 바꿨을 때도 발동
- Pipe name 아래에 <code>pure:false</code> 삽입