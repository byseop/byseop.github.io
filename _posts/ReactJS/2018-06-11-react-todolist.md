---
layout: post
title: React 기초 입문 프로젝트 - Todo List 만들기
feature-img: "assets/img/react/react_bg.png"
tags: [ReactJS, study]
---

최근에 리액트 공부를 하기 시작했다. 시작한건 한달정도 전이었는데 리액트를 공부하기위해 자바스크립트 ES6 의 문법부터 익혔다.  
이제 본격적으로 입문 프로젝트를 시작하려고 한다. 첫번째 프로젝트는 Todo List 프로젝트이다.
<br>
<br>
<br>
<br>

## 0. 시작하기

### create-react-app 설치 및 사용

리액트 프로젝트를 만들때는 페이스북에서 만든 리액트 프로젝트 생성 도구인 create-react-app 을 사용한다. 단순히 구현된 기능이 많아~ 하는 수많은 리액트 boilerplate 와는 다르게 정말 프로젝트에 필요한 기능만 딱 들어있다. 초심자의 입장에서 create-react-app 을 사용하여 프로젝트를 생성하고 커스터마이징 하는게 좋아보인다. Github 에 돌아다니는 리액트 boilerplate 를 사용하게 된다면 도대체 무슨기능이 어떻게 작동하는지도 모른체 사용하게 될 가능성이 높다. 필요없는 기능을 빼는데 오히려 더 많은 투자 하게 될수도...

```text
create-react-app todo-list
```

<br>
리액트 프로젝트를 만들고 디렉토리에 들어간다.
```text
yarn start
```
<br>
VSCode는 내장된 터미널에서 간단하게 실행할 수 있다. 이제 서버가 실행되었다.  
<br>
![todolist01]({{ site.baseurl }}\assets\img\react\react_todolist01.png){: width="1161"}{: .center}
<br>

App.js 를 다음과 같이 변경해본다.

```jsx
import React, { Component } from 'react'

class App extends Component {
    render() {
        return <div>app</div>
    }
}

export default App
```

이렇게 하면 페이지에 App 이란 텍스트만 나온다.
<br>
<br>

## 1. 컴포넌트 구성하기

이제 컴포넌트를 만들어보자. 컴포넌트는 src/components 디렉토리에 몰아서 만들것이다. 실제 프로젝트를 만들게 될 경우엔 컴포넌트를 종류별로 분류하여 각각 다른 디렉토리야 만들겠지만 난 몇개 되지 않으니 하나에 몰아서 만들것이다.
  
### 첫번째 컴포넌트, TodoTemplate  
  
컴포넌트를 만들땐 그냥 검정 텍스트만 보여지는게 아니라면 스타일도 주어야 한다. 각 컴포넌트마다 css 파일을 만들어 줄것이다.

components 디렉토리에 다음과 같은 파일들을 생성한다.

-   src/components/TodoListTemplate.js
-   src/components/TodoListTemplate.css

우선 이 컴포넌트들의 역할을 말하자면 이름이 명시하는대로 템플릿의 역할을 한다. 이 템플릿은 하나의 '틀' 이라고 보면 된다.  
TodoListTemplate 를 수정해보자!
<br>

```jsx
// src/components/TodoListTemplate.js

import React from 'react'
import './TodoListTemplate.css'

const TodoListTemplate = ({ form, children }) => {
    return (
        <main className="todo-list-template">
            <div className="title">오늘 할 일</div>
            <section className="form-wrapper">{form}</section>
            <section className="todos-wrapper">{children}</section>
        </main>
    )
}

export default TodoListTemplate
```

<br>
이 컴포넌트는 함수형 컴포넌트이다. 파라미터로 받게 되는것은 props 이고, 이를 비구조화 할당 하여 원래 <code>(props) => { ... }</code>를 해야 하는것을 <code>({ form, children }) => { ... }</code> 형태로 작성했다. 이 컴포넌트는 두가지의 props를 받게 된다. children의 경우엔 나중에 이 컴포넌트를 사용하게 될 때
```text
<TodoListTemplate>여기에 있는 내용!</TodoListTemplate>
```
이 들어가게 된다. (태그의 사이)  

여기서 form 은 나중에 인풋과 버튼이 들어가있는 컴포넌트를 렌더링할때 사용한다. 이것도 마치 children 을 사용하듯 JSX 형태로 전달해준다.

```
<TodoListTemplate form={<div>이렇게 말이죠.</div>}>
    <div>여기는 children 자리</div>
</TodoListTemplate>
```

<br>
그리고 CSS도 작성해보면
```css
/*
    src/components/TodoListTemplate.css
*/

.todo-list-template {
    background: white;
    width: 512px;
    box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16), 0 3px 6px rgba(0, 0, 0, 0.23);
    margin: 0 auto;
    margin-top: 4rem;
}

.title {
    padding: 2rem;
    font-size: 2.5rem;
    text-align: center;
    font-weight: 100;
    background: #22b8cf;
    color: white;
}

.form-wrapper {
    padding: 1rem;
    border-bottom: 1px solid #22b8cf;
}

.todos-wrapper {
    min-height: 5rem;
}
```
그리고 index.css 에서 배경색을 회색으로 지정해준다.
```css
/*
    src/index.css
*/

body {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
  background: #f9f9f9;
}
```
<br>
![todolist02]({{ site.baseurl }}\assets\img\react\react_todolist02.png){: width="841"}{: .center}
<br>
위와 같이 보여진다.
<br>
<br>
<br>
### 두번째 컴포넌트, Form 만들기  
  
이 컴포넌트는 인풋과 버튼이 담기는 컴포넌트이다. 기능을 구현하기전 모양새부터 갖춘다. 앞으로 리액트 컴포넌트를 구현하게 될 때는 다음과 같은 흐름으로 개발하자.
1. 컴포넌트의 외형을 정의한다. (컴포넌트 DOM태그작성, CSS스타일 작성)
2. 상태관리 및 props로 필요한 값 전달  
  
components 디렉토리에 다음 파일들을 생성하자:
- src/components/Form.js
- src/components/Form.css  
  
컴포넌트 자바스크립트 파일부터 작성해보자.
```jsx
// src/components/Form.js

import React from 'react';
import './Form.css';

const Form = ({value, onChange, onCreate, onKeyPress}) => {
    return (
        <div className="form">
            <input 
                value={value} 
                onChange={onChange}
                onKeyPress={onKeyPress}
            />
            <div className="create-button" onClick={onCreate}>
                추가
            </div>
        </div>
    );
};

export default Form;
```
이 컴포넌트는 총 4가지의 props를 받아온다.  
- value: 인풋의 값
- onCreate: 버튼이 클릭될 때 실행 될 함수
- onChange: 인풋 내용이 변경될 때 실행되는 함수
- onKeyPress: 인풋에서 키를 입력할 때 실행되는 함수. 이 함수는 나중에 Enter가 눌렸을 때 onCreate를 한것과 동일한 작업을 위해 사용한다.  
  
그러면 스타일링도 적용해보자.
```css
/*
    src/Form.css
*/
.form {
    display: flex;
}

.form input {
    flex: 1; /* 버튼을 뺀 공간을 모두 채운다. */
    font-size: 1.25rem;
    outline: none;
    border: none;
    border-bottom: 1px solid #c5f6fa;
}

.create-button {
    padding: 0.5rem 1rem;
    margin-left: 1rem;
    background: #22b8cf;
    border-radius: 3px;
    color: white;
    font-weight: 600;
    cursor: pointer;
}

.create-button:hover {
    background: #3bc9db;
}
```
다 되었다면 이제 App에 렌더링해보자.  
```jsx
// src/App.js

import React, { Component } from 'react'
import TodoListTemplate from './components/TodoListTemplate';
import Form from './components/Form';

class App extends Component {
    render() {
        return (
            <TodoListTemplate form={<Form/>}>
                템플릿완성
            </TodoListTemplate>
        )
    }
}

export default App
```
<br>
<br>
<br>
### 세번째 컴포넌트, TodoItemList 만들기
이 컴포넌트는 곧 이어 만들 TodoItem 컴포넌트를 여러개 렌더링해주는 역할이다. Template 컴포넌트를 만들었기 때문에 이 컴포넌트에서는 따로 스타일링이 필요 없다. '리스트'를 렌더링 하게 될 때는 보여주는 리스트가 동적인 경우에는 함수형이 아닌 클래스형으로 작성하자. 그 이유는 클래스형 컴포넌트로 작성해야 나중에 성능을 최적화 할 수 있다. (사실 이렇게 작은 프로젝트에서는 컴포넌트 성능 최적화를 하지 않아도 매우 빠르게 작동하지만, 리스트내에서 렌더링 할 컴포넌트가 수백개가 될수도 있다. 이경우에는 컴포넌트 최적화는 필수이다!)  
```jsx
// src/components/TodoItemList.js

import React, { Component } from 'react';

class TodoItemList extends Component {
    render() {
        const { todos, onToggle, onRemove } = this.props;
        
        return (
            <div>
                
            </div>
        );
    }
}

export default TodoItemList;
```
지금은 이렇게 비어있는 컴포넌트를 만들었다. 이 컴포넌트는 3가지의 props를 받는다.  
- todos: todo 객체들이 들어있는 배열
- onToggle: 체크박스를 키고 끄는 함수
- onRemove: 아이템을 삭제시키는 함수  
  
<br>
<br>
<br>
### 네번째 컴포넌트, TodoItem 컴포넌트 만들기  
  
이번엔 TodoItem 컴포넌트를 만들어보자.  
이 컴포넌트는 체크값이 활성화 되어있으면 우측에 체크마크(<code>$#x713;</code>)를 보여주고 마우스가 위에 있을때는 좌측에 엑스마크 (<code>&times ;</code>)를 보여준다. 이 컴포넌트의 영역이 클릭되면 체크박스가 활성화되며 중간줄이 그어지고 좌측의 엑스가 클릭되면 삭제된다.  
다음 파일들을 생성하자:  
- src/components/TodoItem.js
- src/components/TodoItem.css  
  

이제 TodoItem 컴포넌트를 작성해보자. 이 컴포넌트 또한 추후 진행할 최적화를 목적으로 클래스형으로 작성해본다.  
