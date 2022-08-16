# 안드로이드의 RecyclerView 를 알아보자

## 공부하게 된 이유
- 안드로이드 앱 개발에 빠질 수 없는 기능이지만, 약간 복잡하여 매번 사용하며 실수를 하는 기능 중 하나이기에, 개념을 복습하고 가면 좋을 것 같다고 판단했다.
- 또한 안드로이드 입문자가 제일 어려워 하는 기능 중 하나이기도 하고, 모르고 넘어갈 수는 없기 때문에 스터디 주제로 소개하기에도 적합하다고 생각했다.

## RecyclerView 란 ?
- 앱 내에서 화면에 리스트를 그릴 수 있는 기능이다. 
  
- 화면이 작은 모바일 환경에서 리스트는 거의 필수적이기에 안쓰는 앱은 거의 없다고 봐도 무방하다.

- 리스트를 그리기 위해 안드로이드가 제공하는 기능으로는 ListView 와 RecyclerView 가 있는데, ListView 의 성능을 개선하고, 확장해서 나온 것이 RecyclerView 이다.

- 필요에 의해 RecyclerView 대신 ListView 를 사용할 때가 있지만, 대부분 RecyclerView 를 이용하여 리스트를 표현한다.
    - RecyclerView 가 ListView 를 확장하여 만들어졌으며, 훨씬 좋은 성능을 보이기 때문에 주로 사용한다.

## RecyclerView 특징
- 화면에 그려진 View 를 재사용하는 특징이 있다.
- ListView 는 기본 제공하는 Adapter class 를 이용할 수 있으나, RecyclerView 는 RecyclerView.Adapter 상속받아서 개발자가 직접 구현해야 한다.

### 재사용 이요 ?
> 아래의 사진이 보여주는 것처럼, ListView 는 데이터가 1000개라고 할 경우 모두 그리기 위해 View 를 1000개 만들지만
> RecyclerView 는 화면에 나타난 View 만 그리고, 화면 밖으로 사라진 View 는 다음 항목을 그릴 때 재사용 한다.

![RecyclerView](https://user-images.githubusercontent.com/49216939/184875700-9d109c1a-b2bc-4fd8-b4be-1c5e593f113e.png)

### RecyclerView 의 구성 요소

1. Adapter
    - 리스트에 나타날 **항목 View** 와 **데이터**를 연결하여 화면에 나타내는 역할을 담당한다.
    - RecyclerView 를 이용해 리스트를 나타낼 때 가장 핵심적인 기능을 하는 class 이다.
    - 리스트의 추가, 삭제, 수정 등을 이 클래스 내에서 메서드를 만들어 처리할 수 있다.
    - 개발자가 직접 정의하여 구현해야한다.

2. ViewHolder
    - 화면에 표시될 항목의 레이아웃를 저장하는 객체 이다.
    - Adapter 에 의해 관리되며, 이미 생성되어있는 경우 만들어져 있는 뷰홀더를 재활용하여 화면에 나타낸다.
    - ViewHolder 가 만들어질 때 타입을 주어 서로 다른 아이템 레이아웃을 한 RecyclerView 내에서 표현할 수 있다.

3. LayoutManager
- RecyclerView 에 할당해줘야 하는 클래스로써 리스트의 항목이 어떤 식으로 정렬이 될지 LayoutManager 클래스를 통해 결정 된다.
- 미할당 시 화면에 리스트가 출력되지 않는다.(어떤 식으로 그려야할지 모르기 때문)    
- 모든 매니저 클래스에 스크롤을 가로로 할지, 세로로 할지 지정할 수 있다.
- 기본 제공 매니저 종류
   1. LinearLayoutManager : 항목을 1차원 목록으로 정렬
        - ![linearLayoutManager](https://user-images.githubusercontent.com/49216939/184869667-5fd9e29f-f177-4af0-a8e7-406efe89cf27.png)
        - 텀블벅 앱에서 캡쳐
   2. GridLayoutManager : 모든 항목을 2차원 grid 로 정렬
        - ![GridLayoutManager](https://user-images.githubusercontent.com/49216939/184869783-41869f2d-9778-4266-bc2d-502e192d4524.png)
        - 텀블벅 앱에서 캡쳐
   3. StaggeredGridLayoutManager : GridLayoutManager 와 동일하나, 항목의 크기가 동일할 필요가 없는 경우에 사용
        - ![StaggeredGridLayoutManager](https://user-images.githubusercontent.com/49216939/184869842-52660164-c7c1-466b-af26-6514faa29691.png)
        - 런데이 앱에서 캡쳐
    
- ※ iOS 유저라 이해를 돕기 위한 캡쳐는 iOS 로 대체 합니다....;

> RecyclerView 를 만드는 코드를 보며 좀 더 자세히 알아보자!
> 코드는 google 제공 예제 앱에서 발췌, 필요에 의해 조금씩 수정하여 작성 합니다.

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

<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
xmlns:app="http://schemas.android.com/apk/res-auto"
android:layout_width="match_parent"
android:layout_height="wrap_content">

<ImageView
    android:id="@+id/flower_image"
    android:layout_width="48dp"
    android:layout_height="48dp"
    android:layout_margin="8dp"
    android:contentDescription="@string/flower_image_content_description"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toStartOf="@+id/flower_text"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent" />

<TextView
    android:id="@+id/flower_text"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_margin="8dp"
    android:text="@string/flower1_name"
    android:textAppearance="?attr/textAppearanceHeadline5"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintStart_toEndOf="@+id/flower_image"
    app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
 
