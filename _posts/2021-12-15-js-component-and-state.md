# [출처: Vanilla Javascript로 웹 컴포넌트 만들기](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Component/)
## 상태관리
`state란 무엇일까? 컴포넌트의 상태(데이터)`
* DOM을 직접 조작하는 것보다, 상태(state)를 기준으로 DOM을 렌더링하는 형태로 발전.
* DOM이 변하는 경우가 state에 종속됨 $\leftrightarrow$ state가 변하지 않을 경우 DOM 변하면 안됨.

## SSR, CSR
* SSR: Service-Side Rendering
  * JSP, PHP, ASP
  * client(browser)에서 data(state)를 정교하게 관리할 필요 없음.

* CSR: Client-Side Rendering
  * JS가 발전하면서 client(browser)에서 모든 렌더링을 처리하려는 시도 $\rightarrow$ Angular(CSR의 시작), React(컴포넌트 개발의 시작), Vue(angular와 react의 장점 모두 수용) 등 탄생

## `state - setState - render` 관련 sample code
```javascript
<div id="app"></div>
<script>
    const $app = document.querySelector('#app');
    let state = {
        items: ['item1', 'item2'],
    }

    let render = () => {
        const { items } = state;
        $app.innerHTML = `
        <ul>
            ${items.map((item) => `<li>${item}</li>`).join('')}
        </ul>
        <button id="add">item 추가</button>
        `;

        document.querySelector('#add').addEventListener('click', () => {
            console.log(`clicked`);
            setState({ items: [...items, `item${items.length + 1}`] })
        });
    }

    let setState = (newState) => {
        state = { ...state, ...newState };
        render();
    }

    render();
</script>
```
* 위 sample code의 핵심은 무엇일까? ... 위 [화살표 함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions) setState 수행 시, render()를 call 한다는 부분이다. 현대 front-end 개발은 DOM 렌더링이 state에 종속된다.
