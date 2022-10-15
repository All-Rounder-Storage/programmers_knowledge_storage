# 순차 리스트(Sequential List) vs 연결 리스트(Linked List)

이번 주제는 제목에서 나타내듯이 **List, 리스트**라는 자료구조이지만 면접에서 리스트(특히나 Linked List)를 물어볼 때 비교대상으로 같이 물어보는 **Array**에 대해서도 간단히 정리해보고자 한다.

우선 실제로 필자가 받았던 질문은 아래와 같았다.

- Array와 Linked List에 차이가 무엇인가요?
- Array와 Linked List의 조회, 삽입/삭제는 어떻게 이뤄지나요?

자 그렇다면 우선 배열, Array에 대해서 먼저 알아보자

## 배열 (Array)

---

여러 데이터를 하나의 이름으로 그룹핑해서 관리하기 위해 사용된다.

```javascript
// 자바스크립트에서의 배열
const example = new Array();
example[0] = 'apple'; // Element(요소)
example[1] = 'banana';
example[2] = 'pear';
```

### Array의 특징

- 고정된 저장 공간을 가진다.
    - 만약에 고정된 저장 공간 (size)보다 더 많이 data를 저장해야할 때는 더 큰 저장 공간 (Array)를 선언하여 data를 옮겨서 할당하고 기존 Array는 삭제 한다.

  ⭐️ 이렇듯 동적으로 배열의 크기를 조절하는 자료구조를 **Dynamic Array**라고 한다.

    - 미리 size를 예측하기 힘들다면 Linked List를 사용하여 data가 추가될 때마다 메모리 공간을 할당받을 수도 있다.
- 관련된 data를 메모리상에 연속적, 순차적으로 저장한다.
- 얼마만큼의 데이터를 저장할지 알고 있고, 조회를 많이 하게 된다면 Array를 사용한다.

### 시간복잡도

- 데이터 접근: O(1)
    - index를 알고 있을 경우 특정 Element에 접근하는 시간 복잡도는 O(1)이다.
- 데이터 삽입/삭제: O(n)
    - 메모리상에 연속적으로 데이터들이 저장되어 있기 때문에, 데이터를 삽입 혹은 삭제할 때 기존 데이터들을 모두 한칸씩 움직여줘야 하기 때문에 O(n)의 시간복잡도를 가진다.

## **리스트 (List)**

---

리스트는 선형으로 원소를 나열하는 구조이다.

구현 방법에 따라 **순차 리스트(Sequential List)** 와 **연결 리스트(Linked List)** 로 나뉘어진다.

### 1.  순차 리스트 (Sequential List)의 특징

- 순차 리스트인 ArrayList는 Array를 사용한다.
- 하지만 초기에 메모리에 크기를 지정 해줘야하는 Array와는 다르게 ArrayList는 크기를 지정하지 않는다.

### 시간복잡도

- 데이터의 접근: O(1)
    - Array를 사용하는 Array List는 index를 가지고 있어 index를 알고 있다면 데이터에 한번에 접근할 수 있다.
- 데이터 삽입/삭제: O(n)
    - 새로운 데이터를 삽입 삭제하려고 한다면 기존에 있던 리스트에서 추가하고 싶은 index부터 마지막 index까지 한칸씩 뒤로 미루거나 한칸씩 앞으로 당기는 연산이 필요하다.

<br />

---

### 2.  연결 리스트 (Linked List)의 특징

- Node라는 구조체로 이루어져 있는데, Node는 data값과 다음 Node의 address를 저장한다.
- 물리적인 **메모리상에서는 비연속적**으로 저장되지만 각각의 Node들이 다음 Node의 address를 가리키기 때문에 **논리적인 연속성**을 가진다.
    - Array는 물리적 메모리 상에서 순차적으로 저장한다는 점에서 차이가 있다.
    - Linked List는 메모리상에서 연속성을 유지하지 않아도 되어 메모리 사용이 더 자유롭지만 다음 Node의 address도 같이 저장해야하기 때문에 데이터 하나당의 메모리는 더 크다.
- 데이터가 추가되는 시점에서 메모리를 할당한다. (메모리를 효율적으로 사용할 수 있다.)
- 저장할 데이터의 수가 불확실하고 삽입 삭제가 잦다면 Linked list를 사용한다.

<br />

### 시간복잡도

- 데이터의 접근: O(n)
    - Linked List는 메모리 상에서 불연속적으로 데이터를 저장하기 때문에 순차적인 접근만 가능하기 때문에 O(n)의 시간복잡도를 가진다.
- 데이터 삽입/삭제: O(1)
    - Array는 데이터를 삽입/삭제 하는 경우에 기존 데이터들을 모두 한칸씩 움직여줘야 했지만 Linked List는 데이터의 이동없이 Node가 가리키는 next address만 변경해주면 되기때문에 O(1)의 시간복잡도를 가진다.


Array, Array List, Linked List를 각각 알아봤으니 글의 처음에서 언급했던 필자가 실제로 면접질문으로 받아 보았던 Array와 Linked List의 차이점에 대해서도 정리해보고 글을 마무리 해보려한다.

## Array와 Linked List

---

- Array는 메모리상에서 연속적으로 데이터를 저장하지만 Linked List는 메모리상에서는 불연속적이나 논리적인 연속성을 가진다.
- 시간복잡도
    - 데이터 조회
        - Array: O(1)
        - Linked List: O(n)
    - 데이터 삽입/삭제
        - Array: O(n)
        - Linked List: O(1)
- 저장할 데이터의 양을 알고 있으며 데이터 조회를 많이 할때는 Array를 활용한다. 반대로 저장할 데이터의 양이 정해져 있지 않고 데이터 삽입/삭제를 많이 할 때는 Linked List를 사용한다.
- 메모리 할당
    - **Array**는 **컴파일 단계**에서 메모리 할당이 일어난다. (Static Memory Allocation) ⇒ **Stack memory 영역**에 할당된다.
    - **Linked List**는 **런타임 단계**에서 새로운 Node가 추가될 때마다 메모리 할당이 일어난다. (Dynamic Memory Allocation) ⇒ **Heap 메모리 영역**에 할당된다.

### Reference

---

- [기출로 대비하는 개발자 전공면접 [CS 완전정복]](https://www.inflearn.com/course/%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%A0%84%EA%B3%B5%EB%A9%B4%EC%A0%91-cs-%EC%99%84%EC%A0%84%EC%A0%95%EB%B3%B5)