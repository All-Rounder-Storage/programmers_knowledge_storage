# 안드로이드의 RecyclerView 를 알아보자

## 공부하게 된 이유
- 안드로이드 앱 개발에 빠질 수 없는 기능이지만, 약간 복잡하여 매번 사용하며 실수를 하는 기능 중 하나이기에, 개념을 복습하고 가면 좋을 것 같다고 판단했다.
- 또한 안드로이드 입문자가 제일 어려워 하는 기능 중 하나이기도 하고, 모르고 넘어갈 수는 없기 때문에 스터디 주제로 소개하기에도 적합하다고 생각했다.

## RecyclerView 란 ?
- 앱 내에서 화면에 리스트를 그릴 수 있는 기능이다. 

- 안쓰는 앱은 거의 없다고 봐도 무방하며 필요에 의해 RecyclerView 대신 ListView 를 사용할 때가 있지만, 대부분 RecyclerView 를 이용하여 리스트를 표현한다.

## RecyclerView 특징
- 화면에 그려진 view 를 재사용하는 특징이 있는 list 이다.

### RecyclerView 의 구성 요소

1. Adapter
    - 
2. ViewHolder

3. LayoutManager
- RecyclerView 에 할당해줘야 하는 클래스로써 리스트의 항목이 어떤 식으로 정렬이 될지 LayoutManager 클래스를 통해 결정 됩니다.
- 미할당 시 화면에 리스트가 출력되지 않습니다.    
- 모든 매니저 클래스에 스크롤을 가로로 할지, 세로로 할지 지정할 수 있습니다.
- 기본 제공 매니저 종류
   - LinearLayoutManager : 항목을 1차원 목록으로 정렬합니다. 
   - GridLayoutManager : 모든 항목을 2차원 grid 로 정렬 합니다.
   - StaggeredGridLayoutManager : GridLayoutManager 와 동일하나, 항목의 크기가 동일할 필요가 없는 경우에 사용
      
