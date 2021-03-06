---
layout : tutorials
title : 가상 돔과 돔의 차이점
category : tutorials
summary : 이 문서는 http://reactkungfu.com/2015/10/the-difference-between-virtual-dom-and-dom/ 를 번역한 내용입니다.
permalink : /tutorials/translate/virtual-dom
tags : javascript react virtualdom
author : apple77y
---

리액트는 가상 돔이라는 개념을 메인 페이지에 놓았다. 이 개념은 매우 중요하게 보인다.

근데 이 "가상 돔" 이라는 것이 정확히 어떤 의미일까?

## 돔

사전적인 의미로 돔은 Document Object Model을 의미하고, 이것은 구조화된 텍스트의 개념이다.

그러므로 HTML은 단순 텍스트일지라도 돔은 메모리에 값을 가지고 있는 표현 식이다.

```
프로그램의 인스턴스와 비교해보자.
하나의 프로그램에는 다수의 프로세스가 있다. 하나의 HTML 안에 다수의 돔이 있는 것처럼 말이다.
(예를 들어 같은 페이지 안에 여러 개의 탭이 있는 경우)
```

HTML 돔은 노드를 탐색하거나 수정할 수 있는 API를 제공한다. 돔은 ``` getElementById ``` 혹은 ``` removeChild ```와 같은 메소드를 포함한다. 우리는 돔과 관련된 작업을 하기 위해 보통 자바스크립트라는 언어를 사용한다.

왜 하필 자바스크립트인지는 아무도 모르지만.. :)

그러므로 웹 페이지의 컨텐츠를 동적으로 수정하고 싶으면 우리는 돔을 수정한다.

``` javascript
var item = document.getElementById("myLI");
item.parentNode.removeChild(item);
```

``` document ```는 root node의 추상화된 개념이다. 반면에 ``` getElementById ```, ``` parentNode ```, ``` removeChild ```와 같은 메소드는 HTML DOM API에서 비롯되었다.

## 이슈

HTML 돔은 HTML document의 구조와 같은 트리 구조로 이루어져 있다. 트리 구조는 탐색하기 쉬우므로 사용성이 좋다. 하지만, 쉽다고 했지 빠르다고 하지는 않았다.

오늘날의 돔 트리는 매우 거대하다. 왜냐하면, 우리는 더욱더 크고 다양한 웹 어플리케이션(_싱글 페이지 어플리케이션 - SPA_)을 개발하고 있다. 그러다 보니 우리는 돔 트리를 수정하는 일이 점점 잦아졌다. 이 작업은 개발하는 데 있어서 굉장히 골칫거리다.

만약 돔 안에 천 개 단위의 ``` div ```이 있다고 가정하자. 우리는 모던 웹 개발자이기 때문에 이 앱은 SPA로 구성되어 있다. 거기에는 수많은 이벤트 핸들러 메소드가 있다. - clicks, submits, type-ins... 전형적인 ``` 제이쿼리 ```와 같은 이벤트 핸들러는 이렇게 생겼다.

- 이벤트를 다룰 노드를 찾고,
- 필요하면 업데이트한다.

여기에는 두 가지의 문제점이 있다.

첫 번째로 다루기가 힘들다. 이벤트 핸들러를 다뤄야 하는 상황을 가정해보자. 만약 문맥을 제대로 찾지 못한다면, 코드가 어떻게 흘러가는지 깊숙하게 파고들어야 한다. 시간 소모가 크고 버그가 발생할 수도 있다.

두 번째로 불편하다. 탐색하는 걸 일일이 이런 식으로 해야 할까? 아마 우리가 더 똑똑해져서 어떤 노드가 업데이트 되어야 한다는 걸 구분할 수 있다면 어떨까?

다시 한 번 말하지만, 리액트는 이런 부분을 도와준다. 첫 번째 문제점을 해결해줄 방법은 '정의'이다. DOM 트리를 일일이 탐색하는 낮은 수준의 방법 대신에 컴포넌트가 어떻게 구성되어야 할지 간단하게 정의 해주기만 하면 된다. 낮은 수준의 작업은 리액트가 대신해줄 것이다. HTML 돔 API를 호출하는 작업에 대해서는 리액트가 해주기 때문에 걱정할 필요가 없다. 결국, 컴포넌트는 어떤 일을 해야 할지에 대해 정의를 해주면 된다.

하지만 이게 퍼포먼스 이슈를 해결해주지는 않는다. 바로 이 부분에서 가상 돔의 개념이 나올 차례다.

## 가상 돔

첫 번째로 가상 돔은 리액트에서 나온 개념이 아니다. 하지만 리액트는 무료로 이 개념을 사용하고 사용자들에게 제공한다.

가상 돔은 HTML 돔의 추상화 개념이다. 이것은 가볍고, 브라우저 스펙의 구현체와는 분리되어있다. 사실, 돔은 이미 추상화 개념이기 때문에 가상 돔은 추상화를 또 추상화한 개념이다.

사실 가상 돔은 리액트 일부의, HTML 돔의 간소화된 복사본이라고 생각하는 게 나을 것 같다. 가상 돔은 리액트가 가상 개념과 (보통 느리고 브라우저 스펙에 따르는) 진짜 돔 개념 간의 계산을 가능하게 해준다.

일반적인 돔과 가상 돔 간에 큰 차이점은 없다. 이 이유가 바로 리액트 내의 JSX 부분이 순수 HTML과 거의 같게 보이는 이유다.

``` javascript
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
```

대부분의 경우에서 HTML 코드가 있는데 이걸 정적인 리액트 컴포넌트로 만들고 싶을 때, 아래와 같이 하면 된다.

1. render 부분에 HTML을 반환해준다.
2. class 속성을 className로 바꿔준다. 왜냐하면, class는 자바스크립트에 사용되는 예약어이기 때문이다.

몇 가지 돔과의 자잘한 차이점이 더 있긴 하다.

- key, ref 그리고 dangerouslySetInnerHTML은 돔에 없는 속성이다. [참고](https://facebook.github.io/react/docs/special-non-dom-attributes.html)
- 리액트스러운 가상 돔은 [몇 가지 제약](https://facebook.github.io/react/docs/dom-differences.html)을 더 소개해준다.

## ReactElement vs ReactComponent

가상 돔을 대할 때, 이 두 가지의 차이점에 대해 알아보는 것이 중요하다.

### ReactElement

이건 리액트에서 표준 타입이다. [리액트 설명](https://facebook.github.io/react/docs/glossary.html#react-elements)에는 이렇게 되어 있다.

` ReactElement는 가볍고, 상태가 없고, 불변이며 돔 요소의 가상 표현식이다. `

``` ReactElement ```는 가상 돔 안에 포함되어 있다. 이 개념들이 노드를 생성한다. ``` ReactElement ```의 불변성이 다루기 쉽고, 비교와 업데이트를 빠르게 만들어준다. 이러한 것들이 리액트의 퍼포먼스가 뛰어난 이유다.


어떤 것들이 ``` ReactElement ```가 될 수 있을까? HTML 태그라면 어떤 것들이던 가능하다. - ``` div, table, strong ```등등. [이곳](https://facebook.github.io/react/docs/tags-and-attributes.html)에 전체 리스트가 있다.

앞에서 정의한 것처럼, ``` ReactElements ```는 일반 돔으로 그려질 수 있다. 여기서부터는 리액트가 요소들을 관리하는 게 중단되는 부분이다. 이것들은 느린 돔 노드가 된다.

``` javascript
var root = React.createElement('div');
ReactDOM.render(root, document.getElementById('example'));
// If you are surprised by the fact that `render`
// comes from `ReactDOM` package, see the Post Scriptum.
```

JSX는 HTML 태그를 ReactElements로 컴파일 한다. 아래와 같은 형식이다.

``` javascript
var root = <div />;
ReactDOM.render(root, document.getElementById('example'));
```

다시 한 번 말하지만, ReactElements는 리액트 가상 돔의 기본적인 아이템들이다. 하지만, 거기에는 상태 값이 없으므로 개발자들이 봤을 때 크게 도움이 될 것 같이 생기지는 않았다. 차라리 우리는 변수와 상수를 이용한, 클래스처럼 생긴 HTML로 작업하는 편이 더 나을 것 같다. 그렇게 생각하지 않나? 그렇다면...

### ReactComponent

``` ReactComponent ```와 ``` ReactElement ```의 차이점은 ``` ReactComponents ```에는 상태 값이 있다는 것이다.
우리는 보통 어떤 것을 정의할 때 ``` React.createClass ``` 메소드를 사용한다.

``` javascript
var CommentBox = React.createClass({
  render: function() {
    return (
      <div className="commentBox">
        Hello, world! I am a CommentBox.
      </div>
    );
  }
});
```

HTML스러운 블럭은 상태 값을 가질 수 있는 render 메소드를 반환한다. 그리고 가장 좋은 건 (이미 알고 있겠지만, 리액트는 짱이다!) 상태 값이 바뀔 때마다 컴포넌트는 다시 그려진다.

``` javascript
var Timer = React.createClass({
  getInitialState: function() {
    return {secondsElapsed: 0};
  },
  tick: function() {
    this.setState({secondsElapsed: this.state.secondsElapsed + 1});
  },
  componentDidMount: function() {
    this.interval = setInterval(this.tick, 1000);
  },
  componentWillUnmount: function() {
    clearInterval(this.interval);
  },
  render: function() {
    return (
      <div>Seconds Elapsed: {this.state.secondsElapsed}</div>
    );
  }
});
```

``` ReactComponents ```는 동적인 HTML을 디자인 할 수 있는 좋은 도구라고 알려졌다. ``` ReactComponents ```이 가상 돔에 직접 접근할 수 있는 건 아니지만,  ``` ReactElements ```으로 쉽게 컨버팅 된다.

``` javascript
var element = React.createElement(MyComponent);
// or equivalently, with JSX
var element = <MyComponent />;
```

## 차이점

``` ReactComponents ```은 훌륭하다, 또한 다루기 쉬우므로 많이들 사용하고 있다.

``` ReactComponent ```가 상태 값을 바꿀 때, 우리는 일반 돔이 가능하면 최대한 적게 변하길 원한다. 이게 바로 리액트가 하는 역할이다. ``` ReactComponent ```는 ``` ReactElement ```로 변환된다. 그러면 ``` ReactElement ```은 빠르고 쉽게 비교, 업데이트 작업을 한 후 가상 돔에 삽입된다. 일반 돔을 다룰 때보다 훨씬 빠르고 정확하게 된다. (이건 리액트 안에 선언된 diff 알고리즘을 통해 계산된다.)

리액트가 수정 내역을 알게 되면, 이 부분은 low-level(HTML DOM)으로 바뀌고, 일반 돔 안으로 삽입된다. 그 코드는 브라우저에 의해 최적화 된다.

## 요약

가상 돔이 정말 메인 페이지를 강화할 수 있는 기능일까? 나는 그렇다고 생각한다. 예제를 통해서 리액트의 퍼포먼스는 굉장히 좋고, 가상 돔은 정말 큰 도움이 되었다.

추가. 지난주에 리액트 0.14 버전이 나왔는데 돔 처리와 관련된 부분은 ``` react-dom ``` 이름으로 ``` react ``` 패키지에서 따로 분리 되었다. 새롭게 바뀐 점은 [여기](https://facebook.github.io/react/blog/2015/10/07/react-v0.14.html#two-packages-react-and-react-dom)에서 더 읽을 수 있다.
