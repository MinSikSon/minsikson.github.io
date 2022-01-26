---
layout: post
title: "[JS] closure 또 보기"
---
# Closure ...
* 함수 생성 시점에, 그 함수 생성에 관여하는 함수와 변수의 묶음.
# lexical scope
* 흠.. 어려버

# 예제 1
## 예제 code
```javascript
function makeAdder(x) {
    var y = 1;
    return function(z) {
    y = 100;
    return x + y + z;
    };
}

var add5 = makeAdder(5);
var add10 = makeAdder(10);
//클로저에 x와 y의 환경이 저장됨

console.log(add5(2));  // 107 (x:5 + y:100 + z:2)
console.log(add10(2)); // 112 (x:10 + y:100 + z:2)
//함수 실행 시 클로저에 저장된 x, y값에 접근하여 값을 계산
```

# 예제 2
## 예제 code
```javascript
const debounceFrame = (callback) => {
    let currentCallback = -1;
    return () => {
        cancelAnimationFrame(currentCallback); // 현재 등록된 callback이 있을 경우 취소 함
        currentCallback = requestAnimationFrame(callback); // callback이 1프레임 뒤에 실행되로록 함
    }
}

// Observer Pattern: 객체의 상태변화를 관찰하는 관찰자(Observer)들의 목록을 객체에 등록하여
// 상태 변화가 있을 때마다 메서드 등을 통해 "객체가 직접" 각 옵저버들에게 통지하도록 하는 디자인 패턴.
// - 발행/구독 모델

let currentObserver = null;
// NOTE: observe == subscriber
export const observe = (fn) => {
    currentObserver = debounceFrame(fn);
    // currentObserver = fn;
    // console.log(`[observe fn]`, fn);
    fn();
    currentObserver = null;
}

export const observable = (obj) => {
    Object.keys(obj).forEach(key => {
        let _value = obj[key];
        const observers = new Set(); // observers == subscribers
        Object.defineProperty(obj, key, {
            get() {
                if (currentObserver) {
                    observers.add(currentObserver);
                }
                return _value;
            },

            set(value) {
                // 상태가 변했지만, 동일할 경우
                if (_value === value) {
                    return;
                }

                // NOTE: _value, value 값 내에 dictionary가 있다면, return 되어버린다.
                // if (typeof _value !== "object") {
                // NOTE: 배열과 객체 같은 경우에는 JSON.stringify로 비교 가능하다. 그 이외에는..?
                if (JSON.stringify(_value) === JSON.stringify(value)) {
                    return;
                }
                // }

                _value = value;
                observers.forEach(fn => fn());
            }
        });
    });
    return obj;
}
```

## 함수 설명
* ```window.cancelAnimationFrame()```: [window.cancelAnimationFrame()](https://developer.mozilla.org/ko/docs/Web/API/Window/cancelAnimationFrame) 메소드는 이전에 window.requestAnimationFrame() 을 호출하여 스케줄된 애니메이션 프레임 요청을 취소합니다.
* ```window.requestAnimationFrame()```: [window.requestAnimationFrame()](https://developer.mozilla.org/ko/docs/Web/API/Window/requestAnimationFrame) 은 브라우저에게 수행하기를 원하는 애니메이션을 알리고 다음 리페인트가 진행되기 전에 해당 애니메이션을 업데이트하는 함수를 호출하게 합니다. 이 메소드는 리페인트 이전에 실행할 콜백을 인자로 받습니다.
* ```리페인트```: https://developer.mozilla.org/ko/docs/Web/Performance/Critical_rendering_path 를 참고해보자
* ```Object.keys()```: 객체의 속성을 배열로 반환.
* ```Array.prototype.forEach()```: 주어진 함수를 배열 요소 각각에 대해 실행.
```javascript
const array1 = ['a', 'b', 'c'];
array1.forEach(element => console.log(element));
// expected output: "a"
// expected output: "b"
// expected output: "c"
```

* ```Set```: Set 객체는 자료형에 관계 없이 원시 값과 객체 참조 모두 ***유일한 값***을 저장할 수 있습니다.
```javascript
var mySet = new Set();

mySet.add(1); // Set { 1 }
mySet.add(5); // Set { 1, 5 }
mySet.add(5); // Set { 1, 5 }
mySet.add('some text'); // Set { 1, 5, 'some text' }
var o = {a: 1, b: 2};
mySet.add(o);

mySet.add({a: 1, b: 2}); // o와 다른 객체를 참조하므로 괜찮음

mySet.has(1); // true
mySet.has(3); // false, 3은 set에 추가되지 않았음
mySet.has(5);              // true
mySet.has(Math.sqrt(25));  // true
mySet.has('Some Text'.toLowerCase()); // true
mySet.has(o); // true

mySet.size; // 5

mySet.delete(5); // set에서 5를 제거함
mySet.has(5);    // false, 5가 제거되었음

mySet.size; // 4, 방금 값을 하나 제거했음
console.log(mySet);// Set {1, "some text", Object {a: 1, b: 2}, Object {a: 1, b: 2}}
```

* ```원시 값```: [원시 값(primitive, 또는 원시 자료형)](https://developer.mozilla.org/ko/docs/Glossary/Primitive)이란 객체가 아니면서 메서드도 가지지 않는 데이터입니다. 원시 값에는 7종류, string, number (en-US), bigint (en-US), boolean, undefined, symbol, 그리고 null이 존재합니다.
* ```Object.defineProperty()```: [Object.defineProperty()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty) 정적 메서드는 객체에 새로운 속성을 직접 정의하거나 이미 존재하는 속성을 수정한 후, 해당 객체를 반환합니다.
```javascript
const object1 = {};

Object.defineProperty(object1, 'property1', {
  value: 42,
  writable: false
});

object1.property1 = 77;
// throws an error in strict mode

console.log(object1.property1);
// expected output: 42
```

* ```정적 메서드```: [static](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Classes/static) 키워드는 클래스의 정적 메서드를 정의합니다.
```javascript
class ClassWithStaticMethod {
  static staticProperty = 'someValue';
  static staticMethod() {
    return 'static method has been called.';
  }
  static {
    console.log('Class static initialization block called');
  }
}
console.log(ClassWithStaticMethod.staticProperty);
// output: "someValue"
console.log(ClassWithStaticMethod.staticMethod());
// output: "static method has been called."
```