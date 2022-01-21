---
layout: post
title: "[JS] closure 또 보기"
---
# Closure ...
* 함수 생성 시점에, 그 함수 생성에 관여하는 함수와 변수의 묶음.
# lexical scope
* 흠.. 어려버

# 예제 1
```javascript
function makeAdder(x) {****
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
```javascript
export const observable = (obj) => {
    console.log(`[observable] obj:`, obj);
    Object.keys(obj).forEach(key => {
        console.log(`[Object.keys(obj).forEach] key:`, key, `, obj[key]:`, obj[key], `, typeof obj[key]:`, typeof obj[key]);
        let _value = obj[key];
        const observers = new Set(); // observers == subscribers
        // console.log(`[observable]`, `observers:`, observers, `obj`, obj, `key`, key);
        Object.defineProperty(obj, key, {
            get() {
                if (currentObserver) {
                    observers.add(currentObserver);
                    // console.log(`[observable get()]`, `observers:`, observers, `currentObserver:`, currentObserver, `_value:`, _value);
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
                // console.log(`[${typeof _value}, ${typeof value}]`, `_value:`, _value, `, value:`, value);
                // NOTE: 배열과 객체 같은 경우에는 JSON.stringify로 비교 가능하다. 그 이외에는..?
                if (JSON.stringify(_value) === JSON.stringify(value)) {
                    return;
                }
                // }

                console.log(`[observable set()] this:`, this, `, _value:`, _value, `(`, typeof _value, `), value:`, value, `(`, typeof value, `)`);
                console.log(`[observable set()] this.tagName:`, this.tagName);
                // console.log(`[observable set()]`, `observers:`, observers, `value`, value);
                _value = value;
                observers.forEach(fn => fn());
            }
        });
    });
    return obj;
}
```

* ```Object.keys```: 객체를 배열로 반환.
```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};

console.log(Object.keys(object1));
// expected output: Array ["a", "b", "c"]
```

* ```Array.prototype.forEach()```: forEach() 메서드는 주어진 함수를 배열 요소 각각에 대해 실행.
```javascript
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"

```