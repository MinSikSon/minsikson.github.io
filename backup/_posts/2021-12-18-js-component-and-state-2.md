# [출처: Vanilla Javascript로 웹 컴포넌트 만들기](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Component/)
# class 화 연습
```html
<div id="app"></div>
<script>
    class App {
        $target;
        state = {
            items: ['item1', 'item2'],
        };
        constructor(target) {
            this.$target = document.querySelector(target);
            this.render();
            this.setEvent();
        }
        render() {
            const { items } = this.state;
            this.$target.innerHTML = `
                <ul>${items.map((item) => `<li>${item}</li>`).join('')}</ul>
                <button id="add">item 추가</button>
            `;
        }
        setEvent() {
            document.querySelector('#add').addEventListener('click', () => {
                const { items } = this.state;
                this.setState({ items: [...items, `item${items.length + 1}`] })
            });
        }

        setState = (newState) => {
            this.state = { ...this.state, ...newState };
            this.render();
            this.setEvent();
        }
    }

    let app = new App('#app');
</script>
```