# woowa_typescript_react
우아한 테크 러닝 리액트&amp;타입스크립트 김민태님 교육 정리

## 4회차
### 올바른 의사소통에 관하여

정보를 주고받는다는 것은 그 기본적으로 수준이 일치해야 한다. 높고 낮음의 의미라기보다는 프로토콜의 의미이다. 내가 a라는 것을 알고 있을 때 이것을 상대방이 이해할 수 있는지 파악하는게 중요한 것이다. 예를 들어 상대가 개발자가 아니면 개발용어로 설명하면 안 된다. 

학습은 지식과 나와의 프로토콜이다. 프로토콜이 맞으면 지식이 쭉 들어온다. 안 맞으면 읽어도 읽어도 모른다. 내가 이 지식을 이해하기 위해서 어느 레이어부터 배워야 내 프로토콜에 맞을까? 이 조정을 스스로 해야 한다. 이게 잘 안되는 사람들을 많이 보았다. 또는 특정 영역이 잘 이해가 된다고 해서 특정 기술만 정말 몰입하는 때도 있다. 누군가에게 너무 어려운 것이라도 내가 알면 쉬운 거다. 내가 어떤 지식을 한쪽으로만 파지 않는 그런 맥락에서 훈련이 좀 필요하다는 생각이 든다.

의식적으로 상대방의 레이아웃을 파악하고 표현해보는 연습을 해보는 것이 중요하다고 생각한다. 팀워크가 안 맞을 때 직접 이런 걸 얘기해주는 사람이 없다. 하나의 주제에 대해 다양한 레이어로 얘기할 수 있는지 고민해보는 게 좋은 거 같다.

--- 
### 리액트 메모리 누수 걱정 관련하여
- 자바스크립트에서 객체안에 있는 데이터를 지우는 방법은 없다.
- gc(가비지컬렉터)에서 알아서 해준다.
- 자바스크립트의 순호순환참조를 걸러낼수 없었는데 최근에 좋은 알고리즘으로 바뀜
- 참조가 없어지면 gc가 일어난다.
- 명시적으로 제공하는 도구는 제공하지 않기에 훅을 날려버려서 메모리 릭을 제거할수 있는 방법은 없다.
--- 

### 비동기
다음 두 식을 동시에 실행할 수 있을까?
```js 
const x = 10;
const y = x * 10;
```
정답은 no.

왜냐하면 x값이 확정되기전에 계산을 실행 할 수 없기 때문이다. 디펜던시가 걸려있는 것이다.

그렇다면 다음 두 식은 동시에 실행할 수 있을까?
```js
const x = () => 10;
const y = x() * 10;
```

정답은 yes. 

왜냐하면 x값이 확정되는 것은 두번째 줄에서 x함수를 실행하면서 부터이다. 이것이 지연이다.
rxjs 나 옵저버 패턴 모두 지연 패턴을 이용한다.

### 제너레이터 
- 제너레이터는 함수는 실행 흐름을 조절할 수 있는 함수이다.
- 제너레이터를 함수를 호출하면 제너레이터를 리턴하며 실행하기 위해서 제너레이터.next()를 해야한다.
- yield가 중단점이며, value,  done 속성을 가지고 있는 리절트 객체를 반환한다.
- 중단점 이후부터 제너레이터.next()를 통해 실행할 수 있다.
- 더 이상 중단점이 없으면 리절트 객체의 done속성은 true값을 가지며 리턴된다.
- 일반 함수와 가장 큰 차이는 함수의 상태를 보존하고 있다는 것이다.
- async await의 경우도 제너레이터로 구현되어 있다.
```js
const delay = (ms) => new Promise((resolve) => setTimeout(resolve, ms)); // resolve를 지연시키고 프로미스 객체를 반환함
const delay2 = ms => ms;

function* main() {
  console.log("시작");
  yield delay2(3000);
  console.log("3초 뒤입니다.");
}

const it = main(); // main제터레이터 함수가 호출되면서 리절트객체(도구)를 it에게 전달해줌
const { value } = it.next(); // {value: 3000, done: false}

if (value instanceof Promise) { // delay1의 경우
  value.then(() => {
    it.next();
  });
} else { // delay2의 경우
  setTimeout(() => {
    it.next(); // {value: undefined, done: true}
  }, value)
}

delay(3000).then(() => {
  console.log("3초 뒤");
});
```