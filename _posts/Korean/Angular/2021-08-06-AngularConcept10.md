---
layout: post
title: "Angular Forms Validation"
subtitle: "[Angular Basic 10] Validation for Template-Driven and Reactive Forms"
date: 2021-08-04
# background: '/img/posts/03.jpg'
categories:
- ko
- angular
lang:
- Korean
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

<br>

## 코드 예시
***
Goal: Template Driven Form를 만들어 보자!
<h6>Step 1: FormsModule 임포트하기</h6>

<div class="code-header">src > app > app.module.ts</div>
~~~~typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule }   from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~~ 
- (#3) and (#15): FormsModule를 임포트 해준다
<br>

<h6>Step 2: Login Form Component 만들기</h6> 

<div class="code-header">bash</div>
~~~~bash
$ cd src/app
$ ng g c template-form
~~~~
- app 디렉토리로 변경 후 template-form component를 만들어 준다
<br>

<h6>Step 3: app.component.html </h6> 

<div class="code-header">src > app > app.component.html </div>
~~~~html
<app-template-form></app-template-form>
~~~~
- (#1): 홈 화면에 template-form component가 보여지게 한다.
<br>

<h6>Step 4: template driven form setup </h6> 

<div class="code-header">src > app > template-form > template-form.component.ts </div>
~~~~typescript
import { Component, OnInit } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-template-form',
  templateUrl: './template-form.component.html',
  styleUrls: ['./template-form.component.css']
})
export class TemplateFormComponent implements OnInit {

  userLoginForm = {
    name: "",
    email: "",
    phone: "",
    password: "",
    confirm_password: ""
  };

  constructor() { }

  ngOnInit(): void {
  }

  onSubmitForm(formData: NgForm) {
    console.log(formData)
  }
}
~~~~
- (#2): NgForm 인포트
- (#11-17): 유저가 입력한 value들을 저장할 곳
- (#24-26): 유저가 submit 버튼을 눌으면 브라우져의 콘솔에서 입력한 value를 확인하기 위한 함수
<br>

<h6>Step 5: template driven form 템플릿 만들기 </h6> 

<div class="code-header">src > app > template-form > template-form.component.ts </div>
~~~~html
<h1>Template Driven Login Form</h1>
<form (ngSubmit)="onSubmitForm(f)" #f="ngForm">
  <input type="text" placeholder="Name" class="form-control" name="userName" [(ngModel)]="userLoginForm.name"/>
  <br>
  <input type="email" placeholder="Email" class="form-control" name="userEmail" [(ngModel)]="userLoginForm.email"/>
  <br>
  <input type="number" placeholder="Phone" class="form-control" name="userPhone" [(ngModel)]="userLoginForm.phone"/>
  <br>
  <input type="password" placeholder="Password" class="form-control" name="userPassword" [(ngModel)]="userLoginForm.password"/>
  <br>
  <input type="password" placeholder="Confirm Password" class="form-control" name="userConfirmPassword" [(ngModel)]="userLoginForm.confirm_password"/>
  <br>
  <button type="submit">Submit</button>
  <button type="reset">Clear</button>
</form>
~~~~
- (#2): f라는 variable를 만들고 그 value를 ngForm이랑 연결시킨뒤 submit를 했을때 아까 만든 함수가 발동되게 합니다.
- (#3-11): 각 하나의 formControl를 담당하고 있습니다. <br> ***(주의: name="SOMEVALUE"와 \[(ngModel)\] 꼭 필요!)***
- (#13-14): 각각 submit과 reset를 담당하는 버튼입니다.
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/128277155-7754505b-1aaf-4d14-9a7b-e91385510e1a.png){: width="100%" height="100%"}{: .align-center}   
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- value 입력 후 submit 버튼을 누르면 브라우져 콘솔에서 입력한 value가 나오는 것을 확인한다

<br>

## 코드 예시 2
***
Goal: Reactive Form를 만들어 보자!
<h6>Step 1: FormsModule 임포트하기</h6>

<div class="code-header">src > app > app.module.ts</div>
~~~~typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule, ReactiveFormsModule }   from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule,
    FormsModule,
    ReactiveFormsModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
~~~~ 
- (#3) and (#16): ReactiveFormsModule를 임포트 해준다
<br>

<h6>Step 2: Login Form Component 만들기</h6> 

<div class="code-header">bash</div>
~~~~bash
$ cd src/app
$ ng g c reactive-form
~~~~
- app 디렉토리로 변경 후 reactive-form component를 만들어 준다
<br>

<h6>Step 3: app.component.html </h6> 

<div class="code-header">src > app > app.component.html </div>
~~~~html
<app-reactive-form></app-reactive-form>
~~~~
- (#1): 홈 화면에 reactive-form component가 보여지게 한다.
<br>

<h6>Step 4: reactive form setup </h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.ts </div>
~~~~typescript
import { Component, OnInit } from '@angular/core';
import { FormControl, FormGroup, FormsModule } from '@angular/forms';

@Component({
  selector: 'app-reactive-form',
  templateUrl: './reactive-form.component.html',
  styleUrls: ['./reactive-form.component.css']
})
export class ReactiveFormComponent implements OnInit {

  userLoginForm = new FormGroup({
    name: new FormControl(''),
    email: new FormControl(''),
    phone: new FormControl(''),
    passwordsGroup: new FormGroup({
      password: new FormControl(''),
      confirm_password: new FormControl('')
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
- (#11-19): Reactive Form를 만들어 준다 (FormBuilder 만드는건 다음 포스트에서 form-validation과 함께)
- (#26-28): 유저가 submit 버튼을 눌으면 브라우져의 콘솔에서 입력한 value를 확인하기 위한 함수
<br>

<h6>Step 5: reactive form 템플릿 만들기 </h6> 

<div class="code-header">src > app > reactive-form > reactive-form.component.ts </div>
~~~~html
<h1>Reactive Login Form</h1>
<form [formGroup]="userLoginForm" (ngSubmit)="onSubmitForm()">
  <input type="text" placeholder="Name" formControlName="name"/>
  <br>
  <input type="email" placeholder="Email" formControlName="email"/>
  <br>
  <input type="number" placeholder="Phone" formControlName="phone"/>
  <br>
  <div formGroupName="passwordsGroup">
    <input type="password" placeholder="Password" formControlName="password"/>
    <br>
    <input type="password" placeholder="Confirm Password" formControlName="confirm_password"/>
    <br>
  </div>
  <button type="submit">Submit</button>
  <button type="reset">Clear</button>
</form>
~~~~
- (#2): \[formGroup\]를 아까 만든 formGroup과 연결해준다
- (#3-7): formControlName를 아까 만든 FormControl들과 이름이 같게 연결한다
- (#9): 패스위드들을 위한 FormGroup은 따로 만들었기 때문에 div tag로 패스워드들을 감싸준다
<br>

<h6>Result</h6>

![image](https://user-images.githubusercontent.com/44415731/128279484-76274b44-7e4e-4757-9863-5b7326804209.png){: width="100%" height="100%"}{: .align-center}   
- Terminal에서 <code>ng serve</code>를 이용하여 브라우저에서 결과를 확인한다.
- value 입력 후 submit 버튼을 누르면 브라우져 콘솔에서 입력한 value가 나오는 것을 확인한다

<br>