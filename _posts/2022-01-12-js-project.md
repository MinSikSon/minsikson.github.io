---
layout: post
title: "[PROJECT] web mini project"
---
```실제 제작할 프로그램을 목표로 정해두고, 프로젝트 진행하기```
# 목표
* issue 단위로 project 진행하기
# 학습
* 이 blog에서 작성해놓은 지난 학습을 복습
# 설계
* TODO: ..
# 에자일 base 일정표
## issue1: searchTicker 라는 함수가 있다. 해당 함수에서 2가지 기능을 담당하기 때문에 re-rendering 시 문제되는 부분 있다.
* 기능
  * [기능 1] input tag 에 값을 넣을 때, 관련 keyword를 화면에 뿌린다
  * [기능 2] input tag 에 입력된 keyword 를 기억한다. (store)
  * [기능 3] input tag 에 값을 넣은 뒤 ```Enter```를 누른 뒤 원하는 데이터를 가져온다.
* 분리
  * [ ] 기능 1 에서 값을 넣을 때마다, keyword 를 저장해 두는 기능 2 를 분리 시켜, input tag 가 re-rendering 되지 않도록 한다.
  * [ ] 기능 2 에서 기억해두는 값을 기능 3 수행시 사용한다.
  * [ ] input tag 인 경우 input 값이 바뀐다고 해도 re-rendering 하지 않게 바꿔야 한다.

## issue2: 하위 Component 에서 addEvent() 수행 시, 하위 Component 에 event 가 붙는 문제.
* [x] event bubbling 을 사용할 것이기 때문에, event 를 root 에 붙여야 한다.
* [x] 중복 event 등록을 적절히 제거해야 한다.