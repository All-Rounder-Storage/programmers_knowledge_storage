# 안드로이드의 RecyclerView 를 알아보자

## 공부하게 된 이유
- 안드로이드 앱 개발에 빠질 수 없는 기능이지만, 약간 복잡하여 매번 사용하며 실수를 하는 기능 중 하나이기에, 개념을 복습하고 가면 좋을 것 같다고 판단했다.
- 또한 안드로이드 입문자가 제일 어려워 하는 기능 중 하나이기도 하고, 모르고 넘어갈 수는 없기 때문에 스터디 주제로 소개하기에도 적합하다고 생각했다.

## RecyclerView 란 ?
- 앱 내에서 화면에 리스트를 그릴 수 있는 기능이다. 
  
- 화면이 작은 모바일 환경에서 리스트는 거의 필수적이기에 안쓰는 앱은 거의 없다고 봐도 무방하다.

- 리스트를 그리기 위해 안드로이드가 제공하는 기능으로는 ListView 와 RecyclerView 가 있다.
    - ListView 의 성능을 개선하고, 확장해서 나온 것이 RecyclerView 이다.

- 필요에 의해 RecyclerView 대신 ListView 를 사용할 때가 있지만, 대부분 RecyclerView 를 이용하여 리스트를 표현한다.
    - RecyclerView 가 ListView 보다 훨씬 좋은 성능을 보이기 때문에 주로 사용한다.

## RecyclerView 특징
- 화면에 그려진 View 를 재사용하는 특징이 있다.
- ListView 는 기본 제공하는 Adapter class 를 이용할 수 있으나, RecyclerView 는 RecyclerView.Adapter 상속받아서 개발자가 직접 구현해야 한다.

### 재사용 이요 ?
> - 아래의 사진이 보여주는 것처럼, ListView 는 데이터가 1000개라고 할 경우 모두 그리기 위해 View 를 1000개 만들지만
> - RecyclerView 는 화면에 나타난 View 만 그리고, 화면 밖으로 사라진 View 는 다음 항목을 그릴 때 재사용 한다.

![RecyclerView](https://user-images.githubusercontent.com/49216939/184875700-9d109c1a-b2bc-4fd8-b4be-1c5e593f113e.png)

### RecyclerView 의 구성 요소

1. Adapter
    - 리스트에 나타날 **항목 View** 와 **데이터**를 연결하여 화면에 나타내는 역할을 담당한다.
    - RecyclerView 를 이용해 리스트를 나타낼 때 **가장 핵심적인 기능을 하는 class** 이다.
    - **리스트의 추가, 삭제, 수정** 등을 이 클래스 내에서 메서드를 만들어 처리할 수 있다.
    - 개발자가 **직접 정의**하여 구현해야한다.

2. ViewHolder
    - 화면에 표시될 항목의 **레이아웃를 저장하는 객체** 이다.
    - **Adapter 에 의해 관리**되며, 이미 생성되어있는 경우 만들어져 있는 **뷰홀더를 재활용**하여 화면에 나타낸다.
    - ViewHolder 가 만들어질 때 **타입**을 주어 서로 다른 아이템 레이아웃을 한 RecyclerView 내에서 표현할 수 있다.

3. LayoutManager
- RecyclerView 에 할당해줘야 하는 클래스로, 리스트의 항목이 어떤 식으로 정렬이 될지 LayoutManager 클래스를 통해 결정 된다.
- 모든 매니저 클래스에 스크롤을 가로로 할지, 세로로 할지 지정할 수 있다.
- 기본 제공 매니저 종류
    1. **LinearLayoutManager** : 항목을 **1차원 목록**으로 정렬
        <img src="https://user-images.githubusercontent.com/49216939/184869667-5fd9e29f-f177-4af0-a8e7-406efe89cf27.png" width="240" height="720">
        - 텀블벅 앱에서 캡쳐
   
    2. **GridLayoutManager** : 모든 항목을 **2차원 grid** 로 정렬
        ![GridLayoutManager](https://user-images.githubusercontent.com/49216939/184869783-41869f2d-9778-4266-bc2d-502e192d4524.png)
        - 텀블벅 앱에서 캡쳐
    
    3. **StaggeredGridLayoutManager** : GridLayoutManager 와 동일하나, **항목의 크기가 동일할 필요가 없는 경우**에 사용
        ![StaggeredGridLayoutManager](https://user-images.githubusercontent.com/49216939/184869842-52660164-c7c1-466b-af26-6514faa29691.png)
        - 런데이 앱에서 캡쳐
   
 
- **※ iOS 유저라 이해를 돕기 위한 캡쳐는 iOS 로 대체 합니다....;**

```markdown
- 만약 깜빡하고 매니저를 할당하지 않으면 ?
    - 화면에 리스트가 출력되지 않고 아무것도 없는 빈화면이 나온다..꼭 잊지말고 넣어야 한다 !

- 그때 안드로이드 개발자의 처참한 심정은 아래와 같다...

＿人人人人人人人人人人＿
＞　제가뭘잘못한거죠ㅜ ＜
￣Y^Y^Y^Y^Y^Y^Y^Y^Y￣
　　　┐( ∵ )┌
　 　  ( 　) 　
　　 　　┘|

- ps. 하지만 더 슬플 때는 매니저 어댑터 뷰홀더 API 연결까지 했는데 화면에 안나올 때...ㅜ
```

```markdown
- 그럼 이제 ! RecyclerView 를 만드는 코드를 보며 좀 더 자세히 알아보자.
- 코드는 google 제공 예제 앱에서 발췌, 필요에 의해 조금씩 수정하여 작성 합니다.
```

1. xml 에 RecyclerView layout 과 내부 항목을 나타낼 layout 을 작성 합니다.
```xml
<!--activity_main.xml-->
    <!--ConstraintLayout : html 의 div 같은 항목으로 해당 레이아웃 안에선 제약조건을 걸어 view 들의 위치를 지정 가능하다 -->
    <androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    
    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="LinearLayoutManager"/>
        <!--id : view 에 id 를 지정하여 java 나 kotlin 코드에서 뷰에 접근할 수 있도록 한다.-->
        <!--layoutManager : layoutManager 는 xml 에서도 지정이 가능하고, 추가적으로 다른 옵션 처리를 위해선 Activity 혹은 Fragment class 안에서 할당해주는 코드가 필요하다. -->
    </androidx.constraintlayout.widget.ConstraintLayout>
```
```xml
<!--item_layout.xml-->
    <androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
    
    <!--ImageView : Html 에서 <img> -->
    <ImageView
        android:id="@+id/iv_image"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_margin="8dp"
        android:contentDescription="@string/flower_image_content_description"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/tv_text"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    
    <!--TextView : Html 에서 <h*, span 등등 텍스트를 나타내는 태그>-->
    <TextView
        android:id="@+id/tv_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="8dp"
        android:text="@string/flower1_name"
        android:textAppearance="?attr/textAppearanceHeadline5"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@+id/iv_image"
        app:layout_constraintTop_toTopOf="parent" />
    
    </androidx.constraintlayout.widget.ConstraintLayout>
```
2. Adapter, ViewHolder class 를 작성 합니다.
    - 간략하게만 작성하였으며, 일을 할 때는 이것보다 훨씬 복잡한 Adapter 가 주로 존재한다.
    - ListAdapter 를 상속받아서 DiffUtil 을 구현해서 사용하는 경우도 있다.
        - DiffUtil 이란 ? : 어댑터 내 데이터의 변경을 비교하여, 데이터 변경을 체크하는 알고리즘
            - 주로 성능 향상을 위해 사용되며, 개발자가 refresh 를 해서 데이터를 다시 세팅하더라도, 데이터가 같으면 해당 데이터는 기존 것을 유지한다.(같은 데이터여도 세팅할 경우 깜빡이는 상황을 앱에서 볼 수 있음(UX 저하))
```kotlin
    class MyAdapter(myElement : ArrayList<MyElement>) : RecyclerView.Adapter<MyAdapter.MyViewHolder>() {
    /**
     * 아이템 뷰를 위한 뷰홀더 객체 생성 
     * */
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.header_item, parent, false)
        return MyViewHolder(view)
    }

    /**
     *  position 값에 해당하는 데이터를 뷰홀더 아이템뷰에 표시
     *  재활용 시에도 활용됨 (로그를 찍어보면 스크롤해서 새로운 아이템이 하단에 표시될 때마다 로그가 찍힘)
     * */
    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        holder.bind(myElement[position])
    }

    /**
     * 현재 아이템의 갯수를 반환하는데 이를 이용하여 필요한 곳에 사용할 수 있다.
     * ex. 댓글 리스트 일 경우 총 댓글 수 표시 등
     * */
    override fun getItemCount() = myElement.size
    
    /**
     * 아래처럼 메서드를 만들어서 수정, 삭제 시 동작 등 을 구현해두고, 사용자 액션에 의해 수행이 필요할 때 호출해서 사용한다.
     * */
    fun updateMyElement(updateElement : MyElement) { /*...*/ }
    fun deleteMyElement(deleteElement : MyElement) { /*...*/ }

    /**
     * ViewHolder class 생성, 어댑터 외부에 따로 파일을 만들어서 할수도 있다.
     * 보통 공통적으로 쓰는 뷰홀더의 경우 따로 만들어서 쓴다.
     * */
    class MyViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        private val tvText: TextView = itemView.findViewById(R.id.tv_text)
        private val ivImage: ImageView = itemView.findViewById(R.id.iv_image)

        fun bind(myElement: MyElement) {
            /* ?: -> 엘비스 프레슬리 연산자라고 하며 값이 null 일 경우 대체 값을 지정해줄 수 있다.*/
            tvText.text = myElement.name ?: "fia"
            ivImage.setImageResource(if(myElement.hasProfileUrl) R.drawble.img_smile else R.drawble.img_default)
        }
    }
}
```
3. RecyclerView 를 가진 Activity 혹은 Fragment 에서 어댑터와 레이아웃 매니저 지정하기
    - 여기선 Activity class 를 예시로 진행한다.

```kotlin
class MyActivity : AppCompatActivity() {
    private val myViewModel = myViewModel()

    // 필요한 메서드만 남김
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        private val myElement = MyElem
        /**
         * findViewById : xml 에 선언한 ID 값을 찾아서 해당 view 를 할당하게 하는 기능 (요즘에는 거의 안써서 쓰면 과제에서 광탈한다)
         * 원래 xml 에 선언한 id 값을 할당하는 최고 멋진 viewBinding /DataBinding 기능이 있는데
         * 이번 챕터에 굳이 필요한 내용은 아니어서 사용하지 않았다.
         * 다음에 하나의 챕터로 알아보려고 한다
         * */ 
        val recyclerView: RecyclerView = findViewById(R.id.recycler_view)
        val myAdapter = MyAdapter(myViewModel.remoteMyElement)
        
        recyclerView.adapter = myAdapter // 어댑터 세팅
        recyclerView.layoutManager = LinearLayoutManager(this, RecyclerView.VERTICAL, false) // 매니저 세팅
    }
}
```
4. 빌드 버튼을 누르고 기다린 후.. 확인한다.


### 여담 - 플러터가 리스트를 그리는 법
- 물론 페이징 처리나, API 에서 가져온 데이터를 붙이려면 이것보단 조금 복잡해지긴 할 것이다. 하지만 코드의 양부터 적긴 하다.

```dart
@override
Widget build(BuildContext context) {
  List<String> myData = [
    "되게",
    "쉽지 않나요?",
    "너무 편해서",
    "플러터 하다가",
    "안드로이드 하면",
    "열받아요...ㅋ"
  ];
  return Scaffold(
      body: ListView.builder(
        itemBuilder: (BuildContext, index){
          return Card(
            child: ListTile(
              title: Text(myData[index]),
            ),
          );
        },
        itemCount: images.length,
        shrinkWrap: true,
        padding: EdgeInsets.all(5),
        scrollDirection: Axis.vertical,
      )
  );
}
```

- 거즘 이런 모양으로 그려질 것이다. 웹에 있는 예제 약간 수정해서 빌드한 캡처본
![flutter list example](https://user-images.githubusercontent.com/49216939/185152315-31ee6d54-c545-4173-9a6e-dafc8b0c3154.png)


ㅋ.................


### reference
- [document](https://developer.android.com/guide/topics/ui/layout/recyclerview?gclid=CjwKCAjwo_KXBhAaEiwA2RZ8hGP2MaKVtySg3ToAcC9uo777Nt4SnBEUC0DSIsk0Mv7FB3XHTJ-yyxoCakEQAvD_BwE&gclsrc=aw.ds)

- [google example](https://github.com/android/views-widgets-samples/tree/main/RecyclerViewKotlin)

- [reference](https://velog.io/@hoyaho/RecyclerView)

- [reference](https://readystory.tistory.com/216)

- [reference - 오래된 글이지만 자세한 설명 참고](https://recipes4dev.tistory.com/154)