---
layout: post
title: "Angular Forms Validation"
subtitle: "[Angular Basic 10] Validation for Template-Driven and Reactive Forms"
date: 2021-08-13
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
- Validators
- Built-In-Validators
- Form-Control-Status-폼의-의식의-흐름
- Form-Validation-Methods
- 코드-예시-1
- 코드-예시-2
---

## Validators
***
- 유저가 form를 작성할 때 입력 할 수 있는 value를 제한한다
- 예시: 유저가 email-form에 '@'이 없으면 submit를 하지 못하게 한다

## Built-In Validators
- min(x)
  - 입력된 value가 x 보다 높아야 한다
- max(x)
  - 입력된 value가 x 보다 낮아야 한다
- required
  - 무조건 입력해 줘야 한다
- requiredTrue
  - 거의 checkbox를 무조건 check 해 주어야 할때 쓴다
- pattern
  - 지정된 패턴으로만 입력해야 한다
  - Ex: <code>Validators.pattern('[a-zA-Z]*')</code>
- maxLength(x)
  - 입력된 글자의 수가 x 보다 적어야 한다
- minLength(x)
  - 입력된 글자의 수가 x 보다 많아야 한다
- email
  - 이메일을 입력해야 할 때

~~~~
# Example in Html:
<input type="text" ... min="15">
# Example in TS:
name: new FormControl('', Validators.min(15)),
~~~~

## Form Control Status (폼의 의식의 흐름)
<table class="table">
    <tr>
        <th scope="col">#</th>
        <th scope="col">Stage Name</th>
        <th scope="col">Description</th>
    </tr>
    <tr>
        <th scope="row">1</th>
        <td>Value</td>
        <td>
        input box에 value가 있으면 value를 알려주고, 없으면 null를 리턴하다
        </td>
    </tr>
    <tr>
        <th scope="row">2</th>
        <td>Status</td>
        <td>
          Validation의 현 상태를 5가지로 분류해 알려준다:
          <br>
          <ol>
            <li>valid: validation를 전부 다 통과 했을 때</li>
            <li>invalid: validation를 하나라고 통과하지 못 했을 때</li>
            <li>pending: 아직 control이 validation를 확인 중 일 때</li>
            <li>disabled: UI에서 비활성화 되었기 때문에 validation 확인을 할 필요가 없을때</li>
            <li>enabled: UI에서 활성화 되어있을 때</li>
          </ol>
        </td>
    </tr><tr>
        <th scope="row">3</th>
        <td>Error</td>
        <td>
          해당 필드가 에러가 있다고 알려줌
          <br>
          ex: <code>username.errors.minlength</code>
        </td>
    </tr><tr>
        <th scope="row">4</th>
        <td>Pristine</td>
        <td>만약 유저가 UI있는 value를 바꾸지 않았을 때</td>
    </tr><tr>
        <th scope="row">5</th>
        <td>Dirty</td>
        <td>만약 유저가 UI있는 value를 바꿨을 때 (Pristine과 반대)</td>
    </tr><tr>
        <th scope="row">6</th>
        <td>Touched</td>
        <td>유저가 필드에 마우스를 클릭했다가 아무것도 하지 않고 마우스로 다른 곳을 클릭할 경우</td>
    </tr><tr>
        <th scope="row">7</th>
        <td>Untouched</td>
        <td>유저가 아직 필드를 건드리지 않았을 경우 (유저가 마우스로 필드를 클릭하고 가만히 있으면 아직 untouched 상태)</td>
    </tr>
    <tr>
        <th scope="row">8</th>
        <td>valueChanges, statusChanges</td>
        <td>
          value가 바뀌면 또는 status가 바뀌면 알려주는 Observable를 리턴함
          <br>
          ex1: <code>this.loginForm.get("username").valueChanges.subscribe(
            username => {
              console.log("Changed Username:" + username);
            })</code>
        </td>
    </tr>
    <tr>
        <th scope="row">9</th>
        <td>updateOn</td>
        <td>
          validation 확인을 언제 해야하는지 지정해줌 
          <ul>
            <li>change(this is default): 유저가 value를 바꿨을 때</li>
            <li>blur: 확인을 유저가 포커스를 잃었을 때 하기</li>
            <li>submit: 확인을 유저가 submit 했을 때 하기</li>
          </ul>
        </td>
    </tr>
</table>

~~~~typescript
//updateOn 예시
this.loginForm = this.fb.group(
  {
    username: ["", [Validators.required]],
    ...
  },
  {updateOn: "submit"}
)
~~~~

## Form Validation Methods
1. setValidators()
  - 해당 formControl에 validator 할당하기

~~~~typescript
changeValidator(){
  this.loginForm.controls['username'].setValidators(Validator.required);
  this.loginForm.controls['username'].updateValueAndValidity(); //변경뒤 업데이트를 해주기 위해서 꼭 필요!
}
~~~~  

/2. get() / getError()
  - 해당 formControl의 child control를 리턴

~~~~typescript
getLoginDetails()){
  this.loginForm.get('username')
  this.loginForm.get('password')
}
~~~~  

/3. clearValidators()
  - 해당 formControl의 validators를 지움

~~~~typescript
clearValidator(){
  this.loginForm.get('username').clearValidators();
  this.loginForm.get('password').clearValidators();
  this.loginForm.updateValueAndValidity(); //변경뒤 업데이트를 해주기 위해서 꼭 필요!
}
~~~~  

/4. setValue()
  - 해당 formControl의 value를 입력하줌

~~~~typescript
setValues(){
  this.loginForm.setValue({
    "username": "TJ",
    "password": "12345678"
  })
}
~~~~  

/5. patchValue()
  - 해당 formControl의 value를 입력하되 모든 value를 setValue()처럼 지정하지 않아도 됨

~~~~typescript
setPatchValue(){
  this.loginForm.patchValue({
    "username": "TJ" //전부 value를 할당 안하고 username만 할당해도 됨
  })
}
~~~~  


/6. reset()
  - 해당 formControl의 value를 리셋함

~~~~typescript
onSubmit(){
  if (this.loginForm.valid) {
    console.log("Submited!")
    this.loginForm.reset()
  }
}
~~~~  


/7. setErrors()
  - 해당 formControl의 error를 직접 일으킴

~~~~typescript
setErrors(){
  this.loginForm.get('username').setErrors({'server-connection-error':"true"})
}
~~~~  

/8. hasError()
  - 해당 FormControl 에 에러가 있는지 확인

~~~~html
<div *ngIf="username.hasError('required')">Username is required</div>
~~~~  

<br>

## 코드 예시 1
***
Goal: Template Driven Form의 Validator를 만들어 보자
<h6>Step 1: 새로운 프로젝트와 컴포너트를 만든다</h6>

<div class="code-header">terminal</div>
~~~~bash
$ ng new template-validator
$ cd template-validator/src/app
$ ng g c template-form
~~~~
- 새로운 프로젝트와 컴포너트를 만들어 준다
<br>

<h6>Step 2: Bootstrap </h6> 

<div class="code-header"> ./index.html </div>
~~~~html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-KyZXEAg3QhqLMpG8r+8fhAXLRk2vvoC2f3B09zVXn8CA5QIVfZOJ3BCsw2P0p/We" crossorigin="anonymous">
    <base href="/">
    <title>Hello, world!</title>
  </head>
  <body>
    <app-root></app-root>

    <!-- Optional JavaScript; choose one of the two! -->

    <!-- Option 1: Bootstrap Bundle with Popper -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-U1DAWAznBHeqEIlVSCgzq+c9gqGAJn5c/t99JyeKa9xxaYpSvHU5awsuZVVFIhvj" crossorigin="anonymous"></script>

    <!-- Option 2: Separate Popper and Bootstrap JS -->
    <!--
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js" integrity="sha384-eMNCOe7tC1doHpGoWe/6oMVemdAVTMs2xqW4mwXrXsW0L84Iytr2wi5v2QjrP/xp" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.0/dist/js/bootstrap.min.js" integrity="sha384-cn7l7gDp0eyniUwwAZgrzD06kc/tftFf19TOAs2zVinnD/C7E91j9yyk5//jjpt/" crossorigin="anonymous"></script>
    -->
  </body>
</html>
~~~~
- 부스트랩 오피셜 사이트에서 starter template를 복사해 index.html에 넣어준다
- (#10): base 태그를 꼭 넣어줘야하다.
- (#14): app-root 태그를 이용하여 app 컴포넌트가 제일 처음 화면에 보여지게 한다.
- <https://getbootstrap.com/docs/5.1/getting-started/introduction/>
<br>

<h6>Step 3: 폼 만들기 </h6> 

<div class="code-header">src > app > template-form > template-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Template Driven Login Form</h1>
  <form (ngSubmit)="onSubmitForm(f)" #f="ngForm">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input type="text" placeholder="Name" class="form-control" name="userName" [(ngModel)]="userLoginForm.name"/>
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input type="email" placeholder="Email" class="form-control" name="userEmail" [(ngModel)]="userLoginForm.email"/>
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input type="text" placeholder="Phone" class="form-control" name="userPhone" [(ngModel)]="userLoginForm.phone"/>
    </div>
    <div class="mb-3">
      <label for="userPassword" class="form-label">Password</label>
      <input type="password" placeholder="Password" class="form-control" name="userPassword" [(ngModel)]="userLoginForm.password"/>
    </div>
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label">Confirm Password</label>
      <input type="password" placeholder="Confirm Password" class="form-control" name="userConfirmPassword" [(ngModel)]="userLoginForm.confirm_password"/>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~  
![image](https://user-images.githubusercontent.com/44415731/129434341-b0dc8dcb-b139-4f6c-b0c7-d0dfbde18176.png){: width="100%" height="100%"}{: .align-center}
- 저번 시간에 배운 것처럼 일단 template-driven form를 만들기
- 폼 텟플렛만 부스트랩을 이용하여 위에 처럼 만들기
<br>

<h6>Step 4: Required </h6> 

<div class="code-header">src > app > template-form > template-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Template Driven Login Form</h1>
  <form ngNativeValidate (ngSubmit)="onSubmitForm(f)" #f="ngForm">
    <div class="mb-3">
      <label for="userName" class="form-label">Email address</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        [(ngModel)]="userLoginForm.name"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Password</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        [(ngModel)]="userLoginForm.email"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Password</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        [(ngModel)]="userLoginForm.phone"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPassword" class="form-label">Password</label>
      <input
        type="password"
        placeholder="Password"
        class="form-control"
        name="userPassword"
        [(ngModel)]="userLoginForm.password"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label">Password</label>
      <input
        type="password"
        placeholder="Confirm Password"
        class="form-control"
        name="userConfirmPassword"
        [(ngModel)]="userLoginForm.confirm_password"
        required
      />
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~  
![image](https://user-images.githubusercontent.com/44415731/129434368-16da58ba-e561-495e-b575-e7427b5e9919.png){: width="100%" height="100%"}{: .align-center}
- 유저가 필드에 아무것도 쓰지 않고 submit하려고 하면 유저에게 이 필드는 필수사항이라고 알려주는 validator 입니다
- (#3): 'ngNativeValidator'를 폼에 넣어준다
- (#12,23,34,45,56): 'Required'라고 알려준다.
  - 아마 html를 경험해 보신 분들은 알고 계셨을거다.
<br>

<h6>Step 5: Phone Number Length</h6> 

<div class="code-header">src > app > template-form > template-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Template Driven Login Form</h1>
  <form ngNativeValidate (ngSubmit)="onSubmitForm(f)" #f="ngForm">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        [(ngModel)]="userLoginForm.name"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        [(ngModel)]="userLoginForm.email"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        [(ngModel)]="userLoginForm.phone"
        minlength="10"
        maxlength="12"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPassword" class="form-label">Password</label>
      <input
        type="password"
        placeholder="Password"
        class="form-control"
        name="userPassword"
        [(ngModel)]="userLoginForm.password"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label">Confirm Password</label>
      <input
        type="password"
        placeholder="Confirm Password"
        class="form-control"
        name="userConfirmPassword"
        [(ngModel)]="userLoginForm.confirm_password"
        required
      />
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~  
![image](https://user-images.githubusercontent.com/44415731/129434463-3ec44a4b-15ab-4b93-94d3-ae678ad5ff6e.png){: width="100%" height="100%"}{: .align-center}
- 이번에는 전화번호가 10에서 12사이일때만 submit 되게 만들어 보자:
  - (#33,34): minlength와 maxlength를 지정해준다
<br>

<h6>Step 6: 바로 유저한테 인풋이 플렸다고 알려주기 </h6> 

<div class="code-header">src > app > template-form > template-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Template Driven Login Form</h1>
  <form ngNativeValidate (ngSubmit)="onSubmitForm(f)" #f="ngForm">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        [(ngModel)]="userLoginForm.name"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        [(ngModel)]="userLoginForm.email"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        [(ngModel)]="userLoginForm.phone"
        #phone='ngModel'
        minlength="10"
        maxlength="12"
        required
      />
      <ng-container *ngIf="phone.errors?.minlength|| phone.errors?.maxlength">
        <div style="color:red"> Phone number must be minimum 10 digits and maximum 12 digits</div>
      </ng-container>
    </div>
    <div class="mb-3">
      <label for="userPassword" class="form-label">Password</label>
      <input
        type="password"
        placeholder="Password"
        class="form-control"
        name="userPassword"
        [(ngModel)]="userLoginForm.password"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label">Confirm Password</label>
      <input
        type="password"
        placeholder="Confirm Password"
        class="form-control"
        name="userConfirmPassword"
        [(ngModel)]="userLoginForm.confirm_password"
        required
      />
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~  
![image](https://user-images.githubusercontent.com/44415731/129434866-736235d3-9d6d-41b2-9a26-3b8db6a2f5c3.png){: width="100%" height="100%"}{: .align-center}
- 전화번호 필드에서 보다시피 유저가 타입을 하고 submit를 눌러야만 알림이 뜬다
- 하지만, 우리가 원하는 건 바로 알림이 뜨는것이다
- Angular에서는 해당 필드를 variable로 지정하여 확인하고 바로 알려줄 수 있다 
  - (#34): #phone 이라는 variable를 만든다
  - (#39-41): phone 변수가 에러가 있는지 확인 후 화면에 경고를 뜨운다.
<br>

<h6>Step 7: Password Confirm </h6> 

<div class="code-header">src > app > template-form > template-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Template Driven Login Form</h1>
  <form ngNativeValidate (ngSubmit)="onSubmitForm(f)" #f="ngForm">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        [(ngModel)]="userLoginForm.name"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        [(ngModel)]="userLoginForm.email"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        [(ngModel)]="userLoginForm.phone"
        #phone='ngModel'
        minlength="10"
        maxlength="12"
        required
      />
      <ng-container *ngIf="phone.errors?.minlength|| phone.errors?.maxlength">
        <div style="color:red"> Phone number must be minimum 10 digits and maximum 12 digits</div>
      </ng-container>
    </div>
    <div class="mb-3">
      <label for="userPassword" class="form-label">Password</label>
      <input
        type="password"
        placeholder="Password"
        class="form-control"
        name="userPassword"
        [(ngModel)]="userLoginForm.password"
        #password="ngModel"
        required
      />
    </div>
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label">Confirm Password</label>
      <input
        type="password"
        placeholder="Confirm Password"
        class="form-control"
        name="userConfirmPassword"
        [(ngModel)]="userLoginForm.confirm_password"
        #confirm_password="ngModel"
        required
      />
      <ng-container *ngIf="password.value != confirm_password.value">
        <div style="color:red"> Password Not Match</div>
      </ng-container>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~  
![image](https://user-images.githubusercontent.com/44415731/129434989-967d8e55-6aaf-4834-8317-1251c80aaf96.png){: width="100%" height="100%"}{: .align-center}
- 'password'와 'confirm password' input를 확인해 서로 같은지 확인해 보자
  - (#51,63): 각 각 변수를 지정해 준다.
  - (#66-68): 서로 같은지 확인 후 틀리면 경고를 준다
<br>

## 코드 예시 2
***
Goal: Reactive Form 버전의 validator를 만들어 보자!
<h6>Step 1: Reactive Form Setup</h6>
- 코드 예시 1에 step 1과 step 2를 해준다.
- 그 후, 바로 전 포스트처럼 Reactive 폼을 만들어 준다.
<br>

<h6>Step 2: 부스크랩 사용하여 심플한 폼 만들기</h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.html</div>
~~~~html
<div class="container mt-5">
  <h1>Reactive Login Form</h1>
  <form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
      />
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
      />
    </div>
    <div formGroupName="passwordsGroup">
      <div class="mb-3">
        <label for="userPassword" class="form-label">Password</label>
        <input
          type="password"
          placeholder="Password"
          class="form-control"
          name="userPassword"
        />
      </div>
      <div class="mb-3">
        <label for="userConfirmPassword" class="form-label"
          >Confirm Password</label
        >
        <input
          type="password"
          placeholder="Confirm Password"
          class="form-control"
          name="userConfirmPassword"
        />
      </div>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~
- 부스트랩을 이용하여 보기 쉬운 reactive 폼을 만든다
<br>

<h6>Step 3: Validator Required 지정해 주기 </h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.ts</div>
```typescript
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  templateUrl: './reactive-form.component.html',
  styleUrls: ['./reactive-form.component.css']
})
export class ReactiveFormComponent implements OnInit {

  userLoginForm = new FormGroup({
    name: new FormControl('', Validators.required),
    email: new FormControl('', Validators.required),
    phone: new FormControl('', Validators.required),
    passwordsGroup: new FormGroup({
      password: new FormControl('', Validators.required),
      confirm_password: new FormControl('', Validators.required)
    })
  })

  constructor() { }

  ngOnInit(): void {
  }

  onSubmitForm() {
    console.log(this.userLoginForm.value)
  }
}
```
- 유저가 해당 필드에 입력하지 않고 submit를 하지 못하게 validator를 지정해 주자
  - (#12-17): Validators.required를 통해 지정해 줄 수 있다
<br>

<h6>Step 4: 다른 Validators </h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.ts </div>
~~~~typescript
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  templateUrl: './reactive-form.component.html',
  styleUrls: ['./reactive-form.component.css']
})
export class ReactiveFormComponent implements OnInit {

  userLoginForm = new FormGroup({
    name: new FormControl('', Validators.required),
    email: new FormControl('', [Validators.required, Validators.email]),
    phone: new FormControl('', [Validators.required, Validators.minLength(10), Validators.maxLength(12)]),
    passwordsGroup: new FormGroup({
      password: new FormControl('', Validators.required),
      confirm_password: new FormControl('', Validators.required)
    })
  })

  constructor() { }

  ngOnInit(): void {
  }

  onSubmitForm() {
    console.log(this.userLoginForm.value)
  }
}
~~~~
- (#13): 이메일 포멧으로 쓰지 않을 경우를 위한 validator
- (#14): 전화번호가 10에서 12자 사이가 아닐 경우를 위한 validator
<br>

<h6>Step 5: 유저가 validator를 실패할 경우 경고 뜨우기</h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Reactive Login Form</h1>
  <form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        formControlName="name"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('name')?.touched && userLoginForm.get('name')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your name</div>
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
      />
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
      />
    </div>
    <div formGroupName="passwordsGroup">
      <div class="mb-3">
        <label for="userPassword" class="form-label">Password</label>
        <input
          type="password"
          placeholder="Password"
          class="form-control"
          name="userPassword"
        />
      </div>
      <div class="mb-3">
        <label for="userConfirmPassword" class="form-label"
          >Confirm Password</label
        >
        <input
          type="password"
          placeholder="Confirm Password"
          class="form-control"
          name="userConfirmPassword"
        />
      </div>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~
![image](https://user-images.githubusercontent.com/44415731/129435716-27427868-f598-46b3-92b8-7dadba88d5a0.png){: width="100%" height="100%"}{: .align-center}
- (#11): formControlName를 이용하여 해당 input이 어떤 formControl인지 알려준다
- (#12-14): ngClass를 이용하여 만약 유저가 이름을 'touch'만 하고 입력을 안 했을 경우 is-invalid를 Class에 넣는다
- (#17): 만약 ngClass로 인해서 클라스가 'form-control is-valid'가 되었을 경우 부스트랩을 이용하여 유저에게 경고를 한다
<br>

<h6>Step 6: 다른 필드도 경고 뜨워주기</h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Reactive Login Form</h1>
  <form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        formControlName="name"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('name')?.touched && userLoginForm.get('name')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your name</div>
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        formControlName="email"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('email')?.touched && userLoginForm.get('email')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your email</div>
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        formControlName="phone"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('phone')?.touched && userLoginForm.get('phone')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your phone</div>
    </div>
    <div formGroupName="passwordsGroup">
      <div class="mb-3">
        <label for="userPassword" class="form-label">Password</label>
        <input
          type="password"
          placeholder="Password"
          class="form-control"
          name="userPassword"
          formControlName="password"
          [ngClass]="{
            'is-invalid':
            userLoginForm.get('passwordsGroup.password')?.touched && userLoginForm.get('passwordsGroup.password')?.invalid
          }"
        />
        <div class="invalid-feedback">Please enter your password</div>
      </div>
      <div class="mb-3">
        <label for="userConfirmPassword" class="form-label"
          >Confirm Password</label
        >
        <input
          type="password"
          placeholder="Confirm Password"
          class="form-control"
          name="userConfirmPassword"
          formControlName="confirm_password"
          [ngClass]="{
            'is-invalid':
            userLoginForm.get('passwordsGroup.confirm_password')?.touched && userLoginForm.get('passwordsGroup.confirm_password')?.invalid
          }"
        />
        <div class="invalid-feedback">Please enter your password again</div>
      </div>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~
![image](https://user-images.githubusercontent.com/44415731/129435948-6f30df4f-6c6d-4375-accc-0385f97cf70b.png){: width="100%" height="100%"}{: .align-center}
- Step 5처럼 다른 필드에도 터치만 하고 입력을 하지 않았을 때 경고를 띄워주자
<br>

<h6>Step 7: 이메일 Validator</h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.html </div>
~~~~html
<div class="container mt-5">
  <h1>Reactive Login Form</h1>
  <form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        formControlName="name"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('name')?.touched && userLoginForm.get('name')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your name</div>
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        formControlName="email"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('email')?.touched && userLoginForm.get('email')?.invalid
        }"
      />
      <div class="invalid-feedback">
        <ng-container *ngIf="userLoginForm.get('email')?.errors?.required">Please enter your email</ng-container>
        <ng-container *ngIf="userLoginForm.get('email')?.errors?.email">Enter a valid email address</ng-container>
      </div>
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        formControlName="phone"
        [ngClass]="{
          'is-invalid':
          userLoginForm.get('phone')?.touched && userLoginForm.get('phone')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your phone</div>
    </div>
    <div formGroupName="passwordsGroup">
      <div class="mb-3">
        <label for="userPassword" class="form-label">Password</label>
        <input
          type="password"
          placeholder="Password"
          class="form-control"
          name="userPassword"
          formControlName="password"
          [ngClass]="{
            'is-invalid':
            userLoginForm.get('passwordsGroup.password')?.touched && userLoginForm.get('passwordsGroup.password')?.invalid
          }"
        />
        <div class="invalid-feedback">Please enter your password</div>
      </div>
      <div class="mb-3">
        <label for="userConfirmPassword" class="form-label"
          >Confirm Password</label
        >
        <input
          type="password"
          placeholder="Confirm Password"
          class="form-control"
          name="userConfirmPassword"
          formControlName="confirm_password"
          [ngClass]="{
            'is-invalid':
            userLoginForm.get('passwordsGroup.confirm_password')?.touched && userLoginForm.get('passwordsGroup.confirm_password')?.invalid
          }"
        />
        <div class="invalid-feedback">Please enter your password again</div>
      </div>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~
![image](https://user-images.githubusercontent.com/44415731/129436037-c3fe0cfe-fb58-4da2-bcba-e55aa861f30e.png){: width="100%" height="100%"}{: .align-center}
- 이메일 필드에 경우 유저가 입력을 하지 않아서 경고해야 할때도 있지만, 유저가 이메일 포멧을 따르지 않아서 경고를 줘야 할때도 있다
- ngIf를 이용하여 유저가 required validator 아님 email validator를 패스 못 했을 경우 해당 경고만 띄워보자
- (#33-34): 경우에 따라 에러 메세지가 바뀌게 된다
<br>

<h6>Step 8: Length Validator</h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.html </div>
~~~~html
<!-- Omitted Above -->
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        formControlName="phone"
        [ngClass]="{
          'is-invalid':
            userLoginForm.get('phone')?.touched &&
            userLoginForm.get('phone')?.invalid
        }"
      />
      <div class="invalid-feedback">
        <ng-container *ngIf="userLoginForm.get('phone')?.errors?.required"
          >Please enter your phone</ng-container
        >
        <ng-container
          *ngIf="
            userLoginForm.get('phone')?.errors?.minlength ||
            userLoginForm.get('phone')?.errors?.maxlength
          "
          >Phone number must be between 10 digits to 12 digits</ng-container
        >
      </div>
    </div>
<!-- Omitted Below -->
~~~~
![image](https://user-images.githubusercontent.com/44415731/129437298-5db17859-1e21-4a39-b006-80072d3b9cb4.png){: width="100%" height="100%"}{: .align-center}
- Step 7를 응용하여 전화번호 길이의 대한 validator를 만들어 보자
<br>

<h6>Step 9: Password Confirm</h6> 

<div class="code-header">terminal</div>
~~~~bash
mkdir src/app/directives
cd src/app/directives
ng generate directive confirm-equal-validator 
~~~~
- 먼저 directive를 만든다

<div class="code-header">src > app > directives > confirm-equal-validator.directives.ts</div>
~~~~typescript
import { Validator,NG_VALIDATORS, AbstractControl } from '@angular/forms';
import { Directive, Input } from '@angular/core';

@Directive({
  selector: '[appConfirmEqualValidator]',
  providers: [{
    provide: NG_VALIDATORS,
    useExisting: ConfirmEqualValidatorDirective,
    multi: true
  }]
})
export class ConfirmEqualValidatorDirective implements Validator{

  @Input() appConfirmEqualValidator!: string;

  validate(control: AbstractControl):{[key:string]: any} |null {
      const controlToCompare = control.parent!.get(this.appConfirmEqualValidator);
      if(controlToCompare && controlToCompare.value !==control.value){
          return { 'notEqual': true}
      }
      return null;
  }
}
~~~~
- confirm-equal-validator를 위와 같이 만든다.

<div class="code-header">src > app > confirm-password.validator.ts</div>
~~~~typescript
import { AbstractControl, ValidationErrors } from '@angular/forms';
export class ConfirmPasswordValidator {
  static MatchPassword(control: AbstractControl): ValidationErrors | null {
    let password = control.get('passwordsGroup.password')?.value;
    let confirmPassword = control.get('passwordsGroup.confirm_password')?.value;
    console.log(password)
    if (password != confirmPassword) {
      control.get('passwordsGroup.confirm_password')?.setErrors({ ConfirmPassword:true});
      return ({ConfirmPassword:true})
    }
    else {
      return null;
    }
  }
}
~~~~
- confirm-password.validator.ts를 src > app 디렉토리에 만든다
- 위에 코드를 넣는다

<div class="code-header">src > app > reactive-form > reactive-form.component.html </div>
~~~~html
<!-- Omitted Above -->
    <div class="mb-3">
      <label for="userConfirmPassword" class="form-label"
        >Confirm Password</label
      >
      <input
        type="password"
        placeholder="Confirm Password"
        class="form-control"
        name="userConfirmPassword"
        formControlName="confirm_password"
        [ngClass]="{
          'is-invalid':
            userLoginForm.get('passwordsGroup.confirm_password')?.touched &&
            userLoginForm.get('passwordsGroup.confirm_password')?.invalid
        }"
      />
      <div class="invalid-feedback">
        <ng-container *ngIf="userLoginForm.get('passwordsGroup.confirm_password')?.errors?.required">Please enter your password again</ng-container>
        <ng-container *ngIf="userLoginForm.get('passwordsGroup.confirm_password')?.errors?.ConfirmPassword">Password does not match</ng-container>
      </div>
    </div>
<!-- Omitted Below -->
~~~~
![image](https://user-images.githubusercontent.com/44415731/129438708-c6744b89-9606-4156-94de-aa092e082c84.png){: width="100%" height="100%"}{: .align-center}
- (#19-20): 상황에 맞게 경고가 뜨게 한다
<br>

<h6>Final Code of HTML</h6>
~~~~html
<div class="container mt-5">
  <h1>Reactive Login Form</h1>
  <form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
    <div class="mb-3">
      <label for="userName" class="form-label">Name</label>
      <input
        type="text"
        placeholder="Name"
        class="form-control"
        name="userName"
        formControlName="name"
        [ngClass]="{
          'is-invalid':
            userLoginForm.get('name')?.touched &&
            userLoginForm.get('name')?.invalid
        }"
      />
      <div class="invalid-feedback">Please enter your name</div>
    </div>
    <div class="mb-3">
      <label for="userEmail" class="form-label">Email</label>
      <input
        type="email"
        placeholder="Email"
        class="form-control"
        name="userEmail"
        formControlName="email"
        [ngClass]="{
          'is-invalid':
            userLoginForm.get('email')?.touched &&
            userLoginForm.get('email')?.invalid
        }"
      />
      <div class="invalid-feedback">
        <ng-container *ngIf="userLoginForm.get('email')?.errors?.required"
          >Please enter your email</ng-container
        >
        <ng-container *ngIf="userLoginForm.get('email')?.errors?.email"
          >Enter a valid email address</ng-container
        >
      </div>
    </div>
    <div class="mb-3">
      <label for="userPhone" class="form-label">Phone</label>
      <input
        type="text"
        placeholder="Phone"
        class="form-control"
        name="userPhone"
        formControlName="phone"
        [ngClass]="{
          'is-invalid':
            userLoginForm.get('phone')?.touched &&
            userLoginForm.get('phone')?.invalid
        }"
      />
      <div class="invalid-feedback">
        <ng-container *ngIf="userLoginForm.get('phone')?.errors?.required"
          >Please enter your phone</ng-container
        >
        <ng-container
          *ngIf="
            userLoginForm.get('phone')?.errors?.minlength ||
            userLoginForm.get('phone')?.errors?.maxlength
          "
          >Phone number must be between 10 digits to 12 digits</ng-container
        >
      </div>
    </div>
    <div formGroupName="passwordsGroup">
      <div class="mb-3">
        <label for="userPassword" class="form-label">Password</label>
        <input
          type="password"
          placeholder="Password"
          class="form-control"
          name="userPassword"
          formControlName="password"
          [ngClass]="{
            'is-invalid':
              userLoginForm.get('passwordsGroup.password')?.touched &&
              userLoginForm.get('passwordsGroup.password')?.invalid
          }"
        />
        <div class="invalid-feedback">Please enter your password</div>
      </div>
      <div class="mb-3">
        <label for="userConfirmPassword" class="form-label"
          >Confirm Password</label
        >
        <input
          type="password"
          placeholder="Confirm Password"
          class="form-control"
          name="userConfirmPassword"
          formControlName="confirm_password"
          [ngClass]="{
            'is-invalid':
              userLoginForm.get('passwordsGroup.confirm_password')?.touched &&
              userLoginForm.get('passwordsGroup.confirm_password')?.invalid
          }"
        />
        <div class="invalid-feedback">
          <ng-container *ngIf="userLoginForm.get('passwordsGroup.confirm_password')?.errors?.required">Please enter your password again</ng-container>
          <ng-container *ngIf="userLoginForm.get('passwordsGroup.confirm_password')?.errors?.ConfirmPassword">Password does not match</ng-container>
        </div>
      </div>
    </div>
    <button type="submit" class="btn btn-primary me-3">Submit</button>
    <button type="reset" class="btn btn-danger">Clear</button>
  </form>
</div>
~~~~
- 이걸로 validation는 끝!

<br>