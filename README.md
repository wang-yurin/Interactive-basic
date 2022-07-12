# 강의 노트

## 📍 이벤트

- `EventTarget.addEventListener()`
- (이벤트 타겟).addEventListener('type', 이벤트핸들러로 실행되는 함수)
- 함수가 이벤트핸들러로 실행이 되면 this가 가리키는 것은 addEventListener가 호출한 객체(이벤트가 적용이 되는 애 == 이벤트 타겟)이다.
- (이벤트핸들러 함수의 매개변수).currentTarget도 this와 같다.

<br>

## 📍 Node

- 예를들어 부모 엘리먼트를 stage라는 변수로 지정하고 자식 엘리먼트를 클릭했을 때 지우는 것을 한다면?
  - `stage.removeChild(this)`
  - stage(부모), this(자식, 클릭한 애)
- 부모인 stage 변수를 사용하지 않고도 할 수 있는 방법은?
  - `this.parentNode`로 부모 엘리먼트에 접근이 가능하다.
  - 위 예제에서는 this.parentNode가 stage를 가리키기 때문에 `this.parentNode.removeChild(this)`를 쓰는 것과 `stage.removeChild(this)`와 같다.

<br>

## 📍 이벤트 위임

- addEventListener함수가 많아지면 메모리 점유를 많이하여 페이지 성능이 안좋아지는 단점이 있다.
- 이벤트가 일어나는 각각의 요소에게 이벤트를 걸어주는 것(바인딩)이 아니라 그 요소를 감싸고 있는 부모에게 이벤트 처리를 위임한다.
- (예전 게시판처럼 한 페이지에 정해진 여러 데이터들이 나오고 그 다음 페이지를 누르는 인터페이스가 아닌) 최근 SNS에서 스크롤을 할 때 자동으로 게시물 다음 데이터들이 로드되어 붙는 방식인데 그 각각의 게시물들마다 좋아요, 댓글, 공유 등 버튼이 있다고 한다면 새로운 게시물들이 로드 될 때마다 각각의 게시물들에 이벤트가 적용이 되어야 할 것이다. 이때 이벤트 위임을 사용하여 게시물을 감싸고 있는 부모 컨테이너에 이벤트를 적용시켜 주는 방법이 좋다.
- 이벤트 위임은 부모 엘리먼트에게 적용하여 해당 이벤트 타겟만 조사하면 편리하다.

```js
function 🧡이벤트핸들러🧡(이벤트객체) {
   💜this;💜
   💜이벤트객체.currentTarget;💜
   💚이벤트객체.target;💚
}

💜이벤트호출객체💜.addEventListener('click', 🧡이벤트핸들러🧡)
```

- 함수가 이벤트리스너로서 호출이 되면 첫 번째 매개변수 자리에 자동으로 이벤트 객체가 들어온다.
- this와 이벤트객체.currentTarget은 이벤트호출객체를 가리킨다.
- 이벤트객체.target은 💚내가 실제 누른 요소💚를 가리킨다.

<br>

## 📍 transform

- `translate3d` 사용하는 이유?<br>
  : transform에 3d가 붙은 속성들은 하드웨어 가속을 이용해(GPU를 사용한다는 말) 퍼포먼스를 향상시킬 수 있다.
- `translate`에 % 단위를 사용하면 편리한게 해당 속성이 있는 객체의 크기를 % 단위로 잡는다.
- 3D 효과를 주려면?
  - `perspective` 속성 사용하기
  - 무대 자체(부모)에 perspective를 주면 시점 차이가 발생하여 위치에 따라 각기 다르게 동작이 보여진다.
  - 회전하는 애 자체(자식)에게 perspective를 주면 시점 차이가 발생하지 않아 똑같이 동작하게 된다.(시점 통일)
  - `transform-origin` : 축 이동 (기본값 center)
  - `transition` 단축 속성(MDN 참고하기)

<br>

## 📍 이벤트 핸들러

- 간단하게 만드는게 좋다.
- 기능을 잘게 쪼개면 코드가 유연해진다.

<br>

## 📍 this

- 객체가 메서드를 실행했을 때 이 메서드를 실행한 주체 객체를 가리킨다.

```js
const person = {};
person.introduce() -----> this가 가리키는 객체는 person
```

- 자바스크립트 함수 안에서의 this란 함수가 실행되는 컨택스트에 따라서 값이 바뀐다.
- 전역 영역에 정의된 함수 안에서의 this는 window를 가리킨다.

<br>

## 📍 생성자 함수

- 생성자 함수의 이름 첫 글자를 대문자로 (객체를 생성해내는 함수는 첫 글자를 대문자로 쓰는게 관례, 이 함수는 생성자입니다~ 알아보기 편하다.)
- 생성자 함수는 호출 할 때 new 키워드를 붙여야한다.(= 함수가 생성자로서 호출이 되게 하려면 new 키워드를 붙여야한다.)

❗️❗️ 전역에 함수를 정의했을 때 this가 가리키는 건 window였는데(함수 안에서 콘솔로 this를 찍어보면 window 전역객체가 찍힌다.) 생성자 함수도 전역에 정의하는데 생성자 함수 안의 this가 가리키는건 왜 각각의 개별 객체를 가리키는거야??<br>
----> 바로 new 키워드 때문! ❗️❗️

<br>

```js
예제1

function Person(nickname, age) {
    this.nickname = nickname;
    this.age = age;
    this.introduce = function() {
        console.log(`안녕하세요. 저는 ${this.nickname}이고, 나이는 ${this.age}살이에요.`);
    }
}

const person1 = new Person('일분이', 10);
const person2 = new Person('이분이', 8);
person1.introduce();
person2.introduce();


- 생성자는 constructor
- person1, person2는 instance(생성자 함수로 만들어낸 각각의 개별 객체)

- 하지만 이 코드는 person1과 person2에 같은 introduce라는 메서드를 공유하고 있다.
    이게 반복이 된다면 새로운 인스턴스를 추가할 때마다 메모리를 점유해 효율적이지 않다. (예제2가 개선된 코드)
```

```js
예제2

생성자 함수 안의 공통으로 사용하는 메서드를 공유하게 만들려면?
: Person.prototype 객체에 추가하면 된다.

function Person(nickname, age) {
    this.nickname = nickname;
    this.age = age;
    this.introduce();
};

Person.prototype.introduce = function() {
    console.log(`안녕하세요. 저는 ${this.nickname}이고, 나이는 ${this.age}살이에요.`);
}
또는
Person.prototype = {
    constructor: Person,
    introduce: function() {
        console.log(`안녕하세요. 저는 ${this.nickname}이고, 나이는 ${this.age}살이에요.`);
    }

}

- prototype 객체는 함수 하나에 쌍으로 하나만 있다.(개별 인스턴스들이 프로토타입에 있는 메서드와 속성들을 공유한다.)
- 생성자 함수와 prototype은 한 쌍으로 생각하면 편하다.(prototype은 무조건 생성되는 객체)
- 위 예제에서는 introduce를 prototype객체에 추가한 것이니 메모리 상에는 단지 prototype에 메서드 하나가 추가 된 것으로 보면 된다.(객체 또는 인스턴스가 늘어난다고해서 introduce()가 늘어난 것은 아니다. 예제1은 늘어나는 예제이고 이건 아님)
- nickname과 age처럼 각 인스턴스 객체들마다 각기 다른 값이 들어오는 애들은 생성자 안에 this 형태로 작성하면 되고 introduce()처럼 똑같이 공유가면 되는 애들은 prototype 객체에 정의해주면 좋다.
```

<br>

## 📍 스크롤

- `window.pageYOffset` : y축으로 스크롤을 얼마나 했는지 px로 나타내준다.
- `HTMLElement.offsetTop` : 해당 엘리먼트의 처음 위치를 가져온다.
- `Element.getBoundingClientRect()` : DOMRect 객체를 반환하는데 해당 엘리먼트의 크기와 뷰포트의 상대적인 위치 정보를 알려준다.

<br>

## 📍 Transition 이벤트

- `addEventListener()` type 중 transitionstart, transitionend가 있다.
- `TransitionEvent.elapsedTime` : 해당 엘리먼트의 transition이 재생되는데 얼만큼의 시간이 경과했는지? (transition-duration의 시간)
- `TransitionEvent.propertyName` : 해당 엘리먼트의 transition 이벤트 발생 했을 때 적용 된 CSS 속성의 이름 반환.

<br>

## 📍 setTimeout

- 원하는 시간만큼 지연시킬 때, 비동기 테스트할 때, 의도적으로 다음 턴에 실행할 때 사용한다.
- `setTimeout()`은 리턴하는 값이 숫자이다. (반복이 되면 보통 1씩 늘어난다.)
- `clearTimeout()`으로 setTimeout을 취소할 수 있다.

<br>

## 📍 setInterval

- `setInterval(함수, 시간)` : 계속 반복시키는 애, 매개변수에 적은 시간마다 반복을 해준다.
- 캔버스에서 반복적으로 그리거나, 어떠한 값을 갱신할 때 주로 사용한다.
- 단점 : 반복을 해주지만 내가 아무리 빠르게 반복하고 싶어도 처리할 능력이 되지 않으면 버벅이고 처리를 못한다. 프레임로스, 모바일일 경우 배터리 소모가 빠르다.
- `clearInterval`로 반복 멈추게 할 수 있다.
-
- 단점을 개선한 애가 있는데 `requestAnimationFrame()`
- 반복시킬 함수 안에 적어주면 된다.
- 반복 취소시키려면 `cancelAnimationFrame()`

<br>

## 📍 전진 3D에서

- reset.css에 아래 코드 넣는 이유? <br>
  : height는 컨텐츠 크기만큼의 높이를 가지니 컨텐츠가 없다면 height는 0이다.<br>
  페이지를 만들 때 body의 높이가 최소 브라우저 높이 만큼이다~ 라는 걸 인식하고 있어야 작업하기가 수월하다.<br>

```css
html {
  height: 100%;
}
body {
  min-height: 100%;
}
```

<br>

- 부모 컨테이너에 3D 효과를 적용한 것이 자식들에게도 적용이 되게 하려면 자식 엘리먼트에 `transform-style: preserve-3d` 속성 적용하기.
- 스크롤바가 되는 범위(스크롤바의 상단 부분이 기준이 되어 측정이 됨)는 전체 body의 높이에서 스크롤 크기(창의 크기)만큼 빼주어야한다.
  <br>
  <br>
- 아래 코드는 어떤 수치든간에 -1에서 1사이의 값이 나오도록 하는 계산이다.<br>
  정 가운데를 클릭하면 0, 클릭한 지점의 위치에 따라 -1에서 1사이의 비율로 사용하기 좋은 값을 갖게되어 원하는 계산을 수월하게 할 수 있다.<br>
  예를들어 브라우저 width가 1000, 마우스로 클릭한 지점(e.clientX)이 500이라고 한다면<br>
  mousePosition.x = -1 + (500 / 1000) \* 2 = 0

```js
const mousePosition = { x: 0, y: 0 };
window.addEventListener('mousemove', function(e) {
    mousePosition.x = -1 + (e.clientX / window.innerWidth) * 2;
    mousePosition.y = 1 - (e.clientY / window.innerHeight) * 2;
})


(1) e.clientX(현재 마우스 x좌표 값) / window.innerWidth(뷰포트 전체 너비 값) 의 식으로 비율 구하기(이때 값은 0 ~ 1 사이의 값)
(2) -1에서 1 사이의 값이 나와야하기 때문에 *2를 해주어 0 ~ 2 사이의 값을 만들어 준다.
(3) e.clientX / window.innerWidth * 2에 -1을 더하여 -1 ~ 1사이의 값을 만들어 주는 흐름
```
