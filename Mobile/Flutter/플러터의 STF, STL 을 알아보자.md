# 플러터의 STF, STL 을 알아보자

# 공부하게 된 이유
- 슬슬 잊어가던 플러터의 기억을 되찾기 위해..
- 개인적으로 플러터만큼 매력적인 프레임워크가 또 있을까 싶기도 하다. 그래서 잘 하고 싶다 ~

## 이게 뭔가요 ?
- 플러터는 ui 를 그리는 모든 것이 widget 이다.
    - view 의 애니메이션, 아이콘, 스타일, 레이아웃 등 화면에 나타나는 모든 것은 위젯
  - 위젯이 위젯을 포함하고, widget Tree 를 바탕으로 화면이 그려진다.
    - react 로 치면 component 와 비슷한 개념
    - widget 은 STF / STL 로 크게 구분할 수 있음

`상태를 가질 수 있는 widget - StatefulWidget(STF)`

- State 객체를 가지지 않음
    - 상태를 가지지 못하기 때문에 **상태 관리** 필요가 없음
- 위젯의 리빌드 시점, 제거 시점 등이 **외부에서 결정 되어짐**
- 외부의 정보에 따라서 변경, **절대 변하지 않음을 의미하지 않음**

`상태를 가질 수 없는 widget - StatelessWidget(STL)`
- state 를 가질 수 없는 위젯은 **STL(StatelessWidget)**

### fullName 이 있는데 왜 STF / STL 로 표시 했나요 ?
- IDE 에서 **stf** 혹은 **stl** 을 입력하고 enter 를 누르면 자동 완성 되어서 나타난다.

```dart
///stl 입력 시
class Example extends StatelessWidget {
  const Example({ Key? key }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(...);
  }
}
```

```dart

/// stf 입력 시
class Example extends StatefulWidget {
  const Example({ Key? key }) : super(key: key);

  @override
  _ExampleState createState() => _ExampleState();
}

class _ExampleState extends State<Example> {
  @override
  Widget build(BuildContext context) {
    return Container(...);
  }
}
```