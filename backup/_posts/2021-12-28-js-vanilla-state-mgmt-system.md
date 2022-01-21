# [출처: Vanilla Javascript로 상태관리 시스템 만들기](https://junilhwang.github.io/TIL/Javascript/Design/Vanilla-JS-Store/)
# 중앙 집중식 상태관리
```현대적인 프론트엔드 개발 == 상태관리 == 상태관리 기반 DOM 렌더링```
## 어플리케이션 사이즈 커짐에 따라 상태관리도 복잡해진다. 이를 개선하기 위해서,, ```중앙 집중식 저장소 역할 + 예측 가능한 방식으로 상태 변경```이 필요

# 중앙 집중식 저장소 == store
## store와 component의 관계 정의
* store는 여러 개의 component에서 사용됨
* store가 변경됨 -> store가 사용되고 있는 component 변경되어야함

## observer pattern (== publish-subscribe model)
* 객체(component)의 상태 변화를 관찰하는 observer를 객체에 등록 -> 상태변화 -> 객체(component)가 직접 observer에게 통지
* ```분산 이벤트 핸들링 시스템``` 구현에 용이

## observable == publisher
## observe == subscribe?