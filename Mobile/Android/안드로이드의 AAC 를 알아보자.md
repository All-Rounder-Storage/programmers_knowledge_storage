# 안드로이드의 AAC 를 알아보자

## AAC 가 무엇인가요 ?
- **Android Architecture Components 의 약자로 테스트 및 유지보수가 쉬운 앱을 디자인 할 수 있도록 돕는 라이브러리 모음**

## 왜 필요한가요 ?
- UI class 에서 화면을 다루는 코드 외의 다른 로직들을 분리하여 의존성을 줄이기 위해서 필요하다.
    - UI code 를 관리하는 class 는 OS 와 앱 사이를 이어주는 class 일 뿐이며, 메모리 부족과 같은 상황이 발생하면
      OS 에서 class 에서 제거하는 등의 처리가 가능하기 때문에, 이로 인한 서비스 유실, 통신 누락 등을 배제 할 수 있다.
    - UI class 가 가지는 lifeCycle 에 제약을 받지 않고 동작할 수 있도록 설계할 수 있다.

## 무엇이 AAC 에 속하나요 ?
- **ViewModel**
    - **앱의 lifeCycle 을 고려하여 UI 에 관련된 데이터를 저장하고 관리하는 컴포넌트**
- **viewModel 이 하는 일**
    - 비즈니스 로직 및 내부 데이터 관리 등 화면에 필요한 데이터를 요청하고 관리합니다.
    - ui 변경에 따른(화면 가로세로 전환 등) 데이터를 저장하고 관리할 수 있습니다.
    - ui 의 lifeCycle 보다 오래 지속되도록 설계 되어 액티비티가 종료될 때, 프레그먼트가 분리 될 때 까지 메모리에 남아 있습니다.
![viewModelScope](https://user-images.githubusercontent.com/49216939/178214935-dbd49122-6b50-49a9-b904-31a24eeddf22.png)

- **LiveData**
      - 식별 할 수 있는 데이터 홀더 클래스로 다른 앱 컴포넌트의 lifeCycle 을 인식하고, 해당 주기가 활성화 된 상태일 때만
      옵저버에 업데이트 정보를 알립니다.
  
- **liveData 가 하는 일**
1. UI 와 데이터 상태가 동일함을 보장합니다.
    - 안드로이드는 화면 가로 세로 전환 시에도 일시적인 UI 관련 데이터가 소멸된다.
      이때 UI class 에서 제공하는 onSavedBundleInstance()를 통해 필요 데이터 유지 처리 할수는 있지만, 대용량 데이터는 불가능 합니다.
      **이러한 상황에서 UI class 내부에서 처리하면 사라질 데이터를 분리하여 유지하고 있을 수 있습니다.**
2. 메모리 누수를 방지할 수 있습니다.
    - ui class 의 lifeCycle 객체에 결합되어 있어서 연결된 객체의 lifeCycle 이 끝나면 자동으로 소멸합니다.
3. UI class 의 상태에 따라 영향을 받지 않습니다.
    - 화면이 비활성화 된 상태라면, 어떠한 이벤트도 받지 않습니다.
4. 최신 데이터를 유지 합니다.
    - 컴포넌트의 활성화 여부에 따라 비활성화 -> 활성화 전환 시 최신 데이터를 새로 수집합니다.
5. 리소스 공유가 가능합니다.
    - 시스템 서비스를 공유할 수 있도록 LiveData 를 확장하여 시스템 서비스를 래핑해 데이터로써 공유할 수 있습니다.
    
- **사용 방법**
  
```kotlin
//viewModel
class MyViewModel @Inject constructor(
    private val repository : MyRepository
) : ViewModel() { // kotlin 에서 상속(extends)은 ":" 으로 가능 합니다.
    // viewModel 내부에서만 사용할  LiveData
    private val _myRemoteData = MutableLiveData<MyData>()
    // ui class 가 observing 할 LiveData, 변경될 수 있는 _myRemoteData 값을 get 한다.
    val myRemoteData : LiveData<MyData> get() = _myRemoteData 
    
    fun getMyRemoteData() {
        viewModelScope.launch { 
            val response = repository.getMyData()
            _myRemoteData.postValue(response.body.data)
        }
    }
} 

// activity
class MyActivity : AppCompatActivity() {
    private val viewModel = MyViewModel()
    // 생명주기 메서드 생략
    private fun setUpObserver() {
        viewModel.myRemoteData.observe(this) {
            // 데이터를 이용한 화면 처리
        }
    }
}
```
- **Room**
    - **SQLite 추상화 레이어를 제공하여 원활한 DB 접근을 돕는 라이브러리 입니다.**
- **Room 이 하는 일**  
    - 원활한 데이터베이스 접근이 가능하게 합니다.
    - 원격에서 가져온 정보를 캐싱하여 제공할 수 있습니다.
    - 필요에 따라 서비스의 정보(EX. access/refresh token, 사용자 지정 항목) 등을 저장할 수 있습니다.
    - 불러온 정보를 liveData 등으로 반환 할 수 있습니다.

![flow](https://user-images.githubusercontent.com/49216939/178237198-c5fdef95-f3de-42e2-b458-4ad45f0dceb8.png)

이 외에도 의존성을 분리하여 테스트와 유지보수를 수월하게 하는 많은 컴포넌트 및 라이브러리를 지원합니다.
    - Android Jetpack 이라는 이름으로 불리우고, jetpack 에 라이브러리 들로 구성되어 있습니다. 
    - ![jetpack](https://user-images.githubusercontent.com/49216939/178237332-c1ec7ce0-bf87-4460-b9cb-deca7fe4f505.png)

이번 포스트 에서는 공식문서에 Android AAC 에 포함되어있는 항목만 정리했습니다.

## 그럼 MVVM 패턴의 ViewModel 과 AAC ViewModel 은 다른건가요 ? 
- **개념적으로 다릅니다.**

```markdown
<b>MVVM 에서의 viewModel 의 개념</b>
- View 에 필요한 데이터를 관리해주고, 비즈니스 로직을 담당해 데이터를 처리하는 요소 입니다.
- Android 의 ViewModel() class 를 상속받지 않아도 패턴을 구현할 수 있습니다.
- 안드로이드 에만 적용되는 개념이 아닙니다.
- View 와 ViewModel 은 1:n 의 개념 입니다.
<b>AAC 에서의 viewModel 의 개념</b>
- Android 내부에서의 수명주기를 고려하여 UI 관련 데이터를 저장하고 관리하는 요소 입니다.
- 안드로이드가 지원하는 컴포넌트로 안드로이드 환경에서만 사용 가능합니다.
```

## 그런데 왜 MVVM 패턴을 구현하는데 AAC viewModel 을 상속 받아서 사용하나요 ? 
- AAC viewModel 로 데이터를 화면에 바인딩 처리를 해준다면
  MVVM 패턴의 viewModel 로써도 활용이 가능 합니다.
- 부가적으로 메모리 관리도 함께 이루어지니 MVVM 패턴 구현 시 안 쓸 이유가 없기도 합니다.
