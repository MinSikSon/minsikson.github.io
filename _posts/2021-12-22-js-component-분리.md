# [출처: Vanilla Javascript로 웹 컴포넌트 만들기](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Component/)
# [이벤트 버블링](https://joshua1988.github.io/web-development/javascript/event-propagation-delegation/#이벤트-버블링---event-bubbling)
* 이벤트 버블링(하위 element에서 root element 까지 전파)을 사용할 경우에, 최상위 tag에다가 event를 붙이게 된다. 기존 예제에서 사용하던 code를 조금 수정해야 하는데,,
* 이벤트 버블링을 통해서, 각 element에 뭍어있는 event Listener가 모두 수행된다. 아래 예제에는 #app에만 event Listener가 달려 있음.
* 글에는 이렇게 설명 되어 있다.
  * event를 각각의 하위 요소가 아니라 component의 target 자체에 등록하는 것이다.
  * 따라서 component가 생성되는 시점에만 이벤트 등록을 해놓으면 추가로 등록할 필요가 없어진다. 

## code 살펴보기
### AS-IS: SetEvent
```javascript
// add event 대상이 각 btn 이다.
    setEvent() {
        this.$target.querySelector('.addBtn').addEventListener('click', () => {
            const { items } = this.$state;
            this.setState({ items: [...items, `item${items.length + 1}`] });
        });

        this.$target.querySelectorAll('.deleteBtn').forEach(deleteBtn => {
            deleteBtn.addEventListener('click', (e) => {
                const { target } = e;
                const items = [...this.$state.items];
                items.splice(target.dataset.index, 1);
                this.setState({ items });
            })
        });
    }
```
### TO-BE: SetEvent
```javascript
// add event 대상이 this.$target 이다. 즉, #app 이다.
    setEvent() {
        this.$target.addEventListener('click', (e) => {
            const { target } = e;
            if (target.classList.contains('addBtn')) {
                const { items } = this.$state;
                this.setState({ items: [...items, `item${items.length + 1}`] });
            }
            if (target.classList.contains('deleteBtn')) {
                const items = [...this.$state.items];
                items.splice(target.dataset.index, 1);
                this.setState({ items });
            }
        });
    }
```

### App.js
```javascript
import Items from "./components/Items.js";
class App {
    constructor(target) {
        new Items(target);
    }
}
let app = new App('#app');
```
### AS-IS: Component.js
```javascript
export default class Component {
    $target;
    $state;
    constructor(target) {
        this.$target = document.querySelector(target);
        this.setup();
        this.render();
    }
    setup() { }
    template() { return ``; }
    setEvent() { }
    render() {
        this.$target.innerHTML = this.template();
        this.setEvent(); // 여기에서
    }
    setState = (newState) => {
        this.$state = { ...this.$state, ...newState };
        this.render();
    }
}
```
### TO-BE: Component.js
```javascript
export default class Component {
    $target;
    $state;
    constructor(target) {
        this.$target = document.querySelector(target);
        this.setup();
        this.setEvent(); // 요기로 이동!!
        this.render();
    }
    setup() { }
    template() { return ``; }
    setEvent() { }
    render() {
        this.$target.innerHTML = this.template(); // this.$target은 변하지 않고 this.$target.innerHTML이 변할 뿐이다..!
    }
    setState = (newState) => {
        this.$state = { ...this.$state, ...newState };
        this.render();
    }
}
```

* event bubbling 구조로 setEvent()함수를 변경한 뒤에, 왜 이렇게 Component 구조를 바뀌어야 하는지 처음 code를 보면 좀 의문이 생길 수 있는데..! ***this.$target*** 은 ***#app*** 인데, 얘는 re-rendering되지 않기 떄문이다..!! 때문에, 기존처럼 render() 함수 내에 setEvent()가 있을 경우에, re-rendering 될 때마다 setEvent()가 중첩되게 된다. 그래서 setEvent()의 위치를 생성자(constructor)로 옮겨야 한다.
