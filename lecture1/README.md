# 1회

## 어떻게 적정 기술을 선택할 수 있을까?
1. 도구
바닥부터 만드는 경우는 거의 없다.
어떤 도구를 쓰더라도 그 도구의 근본적인 목적을 잘 이용하고 잘 쓰는거 같다.
만든사람의 의도와 흐름을 잘 이해하는 것이 중요

2. 강의 목표 및 기대 
### 4주동안의 키워드
- 상태(state)
- 환경(env)
- 제품(prod)
- 목표(mission)
- 코드(quality)
- 상대적(e=mc2)

### 블루 프린트
- typescript와 리액트와는 궁합이 잘 맞는가?
- 타입스크립트의 엄청난 기능 중에서 타입밖에 안써서 이걸 어떻게 더 활용할까 고민하다
- 블루프린트가 이걸 잘 이용해 볼 수 있을거 같아서 선택

--- 
## 타입스크립트

1. 타입을 명시하지 않아도 타입추론된다.
```ts
let foo = 10; // 컴파일링 하면서 타입추론하기 때문에 별 문제가 일어나지 않는다.
foo = false; // 숫자타입이기 때문에 에러가 난다.
```

명시적
```ts
let foo2: number = 10;
foo2 = false;
```

암묵적인거 보다 명시적인게 좋다라는 생각을 가진 개발자가 많다.
요즘은 짧고 난해한 코드보다 좀 길더라도 명시적인 코드가 좋다고 생각.

```ts
function bar(...args) {
    return 0;
}
bar(10, 20);
```

타입 알리아스 : 오른쪽의 타입의 별명
```ts
type Age = number;
let a: Age = 10;
let weight: number = 72;
```

컴파일 때 검출
- a = 'abc';

컴파일 때만 작동되는 요소 
- 타입알리아스, 제네릭 등

런타임때까지 가서 작동되는 요소 
- ()

javascript에는 없는 컴파일 타임이 있기에 버그 잡기는 쉬우나 다루기 어려워 질 수도

```ts
type Age = number

type Foo = {
    age: number;
    name: string;
}

const foo: Foo = {
    age: 10,
    name: 'kim'
}

interface Bar {
    age: Age,
    name: string
}

const bar: Bar = {
    age: 10,
    name: 'kim',
}
```
