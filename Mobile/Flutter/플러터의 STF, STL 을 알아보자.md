# 플러터의 STF, STL 을 알아보자

# 공부하게 된 이유
- 슬슬 잊어가던 플러터의 기억을 되찾기 위해...기본 개념부터 정리를 시작하려고 함
- 개인적으로 플러터만큼 매력적인 프레임워크가 또 있을까 싶기도 하다. 그래서 잘 하고 싶다 ~

## 이게 뭔가요 ?
- 플러터는 ui 를 그리는 모든 것이 widget 이다.
    - view 의 애니메이션, 아이콘, 스타일, 레이아웃 등 화면에 나타나는 모든 것은 위젯
  - 위젯이 위젯을 포함하고, widget Tree 를 바탕으로 화면이 그려진다.
    - react 로 치면 component 와 비슷한 개념
    - widget 은 STF / STL 로 크게 구분할 수 있음


`상태를 가질 수 없는 widget - StatelessWidget(STL)`

- State 객체를 가지지 않음
    - 상태를 가지지 못하기 때문에 **상태 관리** 필요가 없음
- 위젯의 리빌드 시점, 제거 시점 등이 **외부에서 결정 되어짐**
- 외부의 정보에 따라서 변경, **절대 변하지 않음을 의미하지 않음**
- 사용자 action 혹은 이벤트 에 의해 동작하지 않음
- 화면이 한번 그려지고 **변경이 없는 view** 에 사용

`상태를 가질 수 있는 widget - StatefulWidget(STF)`

- State 객체를 가짐
    - 상태를 가지기 때문에 setState() 메서드가 해당 위젯의 상태 변경을 알림
    - widget 의 동작 동안 data 변경이 있어서 ui 를 업데이트 해야할 때 사용
    - 사용자의 액션( 버튼 클릭, 정보 입력 등) 으로 setState() 가 호출 되면 플러터 엔진에서 해당 widget 을 다시 그림 (reBuild)

### fullName 이 있는데 왜 STF / STL 로 표시 했나요 ?
- IDE 에서 **stf** 혹은 **stl** 을 입력하고 enter 를 누르면 자동 완성 되어서 나타난다. (나름 유용)

`아래의 자동 완성 코드 예시를 보며, STF / STL 의 구성도 함께 알아보자`

```dart
///stl 입력 시
class StudyPlan extends StatelessWidget { /// StatelessWidget : StatelessWidget 을 상속 받아 STL 위젯이 됨 
  
  /// const StudyPlan({ Key? key }) : 기본 생성자로 class 명과 동일한 이름을 가짐
  /// super(key: key) : 부모 StatelessWidget 의 기본 생성자를 호출
  /// 데이터 전달이 필요한 파라미터가 없다면 기본 생성자는 생략이 가능
  
  const StudyPlan({ Key? key }) : super(key: key);
  
  @override
  Widget build(BuildContext context) { /// build : Widget 을 리턴하는 함수, 모든 위젯 클래스에 포함된 필수 메서드
    
    /// Container : flutter 에서 제공하는 기본 Widget 중 하나 html 의 <div> 태그와 비슷한 기능을 함
    return Container();
  }
}
```

```dart

/// stf 입력 시
class StudySchedule extends StatefulWidget { /// StatefulWidget : StatefulWidget 을 상속 받아서 STF 위젯이 됨 
  
  const StudySchedule({ Key? key }) : super(key: key);

  /// createState() : State 객체를 반환하는 필수 메서드로 연결된 State 의 인스턴스를 반환
  /// StatefulWidget 빌드를 동작시키면 createState() 메서드가 즉시 실행 되어짐
  @override
  _StudyScheduleState createState() => _StudyScheduleState();
}

class _StudyScheduleState extends State<StudySchedule> { /// State<StudySchedule> : State<StudySchedule> 을 상속 받는 객체가 build 메서드로 STF widget 반환 
  @override
  Widget build(BuildContext context) {

    /// Column :  flutter 에서 지원하는 widget 으로 내부의 widget 을 vertical 하게 배치 하는 widget
    return Column();
  }
}
```

### StatefulWidget 에 state 를 제어하는 메서드

`initState()`

- 화면에 widget 을 그리기 전에 필요한 초기화를 수행하는 메서드
    - **사용자와의 상호 작용 전 필요한 ui / data 등의 초기화 작업 수행**
        - ex. 기본 값 세팅, 리스너 할당, 체크박스 체크 상태 세팅, 비동기 함수 호출 등
    - android 의 onCreate() 메서드와 비슷한 개념

`setState()`  

- widget 에 state 값 변경을 알려 build 메서드를 재호출 하도록 하는 메서드
    - **사용자와 상호작용 하며 변경된 값에 대해 ui에 나타낼 때 사용**
        - ex. 사용자의 입력 값 반영, api 응답 값 화면에 나타내는 경우 등
  
### 그래서 이게 다같이 어떻게 쓰이는거죠 ?

> flutter 프로젝트를 처음 생성하면 만들어져 있는 코드로 알아보자(필요한 대로 약간의 가공을 했다)

```dart
class MyHomePage extends StatefulWidget {
   MyHomePage({ Key? key }) : super(key: key);
   
   @override
   _MyHomePageState createState() => _MyHomePageState();
 }

 class _MyHomePageState extends State<MyHomePage> {
  
   /// dart 는 타입 지정과 미지정 이 공존한다
   /// int, float, String 등 : 지정된 타입과 알맞은 값으로 초기화
   /// var : 내가 최초 할당한 값에 의해 타입이 정해짐( 처음 String 값, 두번째 int 값 -> 오류 발생 )
   /// dynamic : 할당 값에 따라 변경 됨 (처음 String 값 , 두번째 int 값 -> 처음엔 String, 두번째는 int)
   
  int _counter = 0;
  var title;
  dynamic evenOrOdd;


  @override
  initState() {
    /// 부모의 initState 호출
    super.initState();
    title = "플러터 공부 중";
    evenOrOdd = "not yet";
  }
  
  void _incrementCounter() {
    setState(() {
      ++_counter;
      if(_counter % 2 == 0){
        evenOrOdd = _counter;
      } else {
        evenOrOdd = "odd";
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    /// Scaffold : 일종의 도화지 로 화면 구성에 필요한 기능을 적절한 속성으로 나눠서 가지고 있음
    return Scaffold(
      /// AppBar : 화면 상단 bar 로 주로 뒤로가기 화살표, 해당 화면 이름, 검색, 설정 아이콘 등 존재
      appBar: AppBar( 
        /// initState() 에서 초기화 해준 title 값을 Text widget 에 할당하여 appBar 에 화면 이름을 나타냄
        title: Text(title),
      ),
      /// body : 앱 화면 내에 메인 콘텐츠를 나타내는 영역
      /// Center : 내부에 속한 Widget 을 화면의 중앙에 정렬하여 배치
      body: Center(
        child: Column(
          /// mainAxisAlignment : Column 내부 Widget 의 정렬을 결정하는 속성, 여기선 가운데 정렬
          mainAxisAlignment: MainAxisAlignment.center,
          /// children: <Widget>[] : 여러 widget 을 포함할 수 있는 속성
          /// 단일 위젯만 가질 수 있는 경우 child 속성을 지님
          children: <Widget>[
            /// 아래의 두 Text widget 은 모두 setState() 를 통해 변경되는 값을 할당 받고 있어서
            /// setState() 가 호출되면서 변경 된 값이 화면에 나타난다 (reBuild)
            
            Text(
              /// '$_counter' : setState() 로 변경되는 int 값을 할당 받고 나타냄 
              '$_counter',
              /// style : 해당 Text Widget 의 텍스트 스타일 지정
              style: Theme.of(context).textTheme.headline4,
            ),
            
            Text(
              /// dynamic type 으로 선언 되어 setState() 에서 변경되는 숫자 혹은 문자 값을 할당 받고 나타냄
              '$myHomeNickName',
            ),
          ],
        ),
      ),
      /// floatingActionButton : 떠 있는 버튼 기본 정렬 우측 하단 
      floatingActionButton: FloatingActionButton(
        ///onPressed : 버튼 클릭 시 할 동작, 여기서는 _incrementCounter 메서드가 동작(위에 만들어져 있음)
        onPressed: _incrementCounter,
        /// tooltip : 커서를 올려놓으면 뜨는 알림 메세지
        tooltip: '버튼 누르기 ~ ', 
        /// Icon : flutter 에서 제공하는 Icon widget
        /// 할당된 Icons.add 도 제공하는 icon 으로 프로젝트 내부에 이미지 저장이 필요 없음
        child: Icon(Icons.add), 
      ),
    );
  }
}
```
### 그럼 setState() 로 변경을 다할 수 있겠네요 ?
- 가능하지만, 잘 사용하지 않습니다. 그 이유는 ↓
    - 프로그램이 커질수록 관리가 힘들고, 어느 위치에서 어떤 값을 통해 reBuild 가 되었는지 추적하기 쉽지 않음
  
`그럼 어떻게 하나요 ? `

- 상태관리를 지원하는 library 사용해 관리 함
    - ex. getX , provider, bloc 등 개발 시 필요에 맞는 library 로 사용


## reference
- [document - 직접 플러터 코드를 실행시켜볼 수 있음](https://api.flutter.dev/flutter/material/Scaffold-class.html)

- [reference](https://velog.io/@dosilv/Flutter-StatelessWidget-StatefulWidget)

- [reference](https://itwise.tistory.com/29)
