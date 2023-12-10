
## Array

가장 기초적인 자료구조인 `Array`은 저장에 있어서 의도한 순서와 물리적으로 메모리에 저장되는 순서가 동일하다. 

---

## LinkedList

`LinkedList`는 특정 Context 안에 모든 원소를 담아둔 방식이 아닌, 원소와 원소의 연결을 통하여 구현한 `List` 자료구조의 일종이다.

## ArrayList vs LinkedList

#### ArrayList의 구현

```c
#include <stdio.h> // arrayList.c
#include <stdlib.h> 

#define MAX_LIST_SIZE 100

typedef int element;
typedef struct {
    element array[MAX_LIST_SIZE];
    int size;
} arrayListType;
```

`ArrayList`는 `Array`를 기반으로 구현한 List이기 때문에 `Array`의 모든 특성을 가진다. 

#### ArrayList로 인덱싱을 하는 경우

```c
element get_entry(arrayListType *L, int pos){
    if (pos < 0 || pos >= L->size)
        error("위치 오류");
    return L->array[pos];
}
```

`ArrayList`의 장점으로는 특정 인덱스의 값을 조회 시 이미 위치를 알고 있기 때문에 `(배열의 주소 + 타입 * 인덱스)` Random Access에 있어서 `O(1)`의 시간 복잡도를 가진다.

#### ArrayList로 삽입 또는 삭제를 하는 경우

```c
void insert_list(arrayListType *L, element item) {
    if (L->size >= MAX_LIST_SIZE) {
        error("리스트 오버플로우");
    }
    L->array[L->size++] = item;
}

void insert(arrayListType *L, int pos, element item) {
    if (!is_full(L) && (pos >= 0) && (pos < L->size)) {
        for (int i = (L->size - 1); i >= pos; i--)
            L->array[i + 1] = L->array[i];
        L->array[pos] = item;
        L->size++;
    }
}

element delete(arrayListType *L, int pos) {
    element item;

    if (pos < 0 || pos >= L->size)
        error("위치 오류");
    item = L->array[pos];
    for (int i = pos; i < (L->size - 1); i++) 
        L->array[i] = L->array[i + 1];
    L->size--;
    return item;
}
```

일반적인 삽입의 경우에는 간단하게 마지막 인덱스 뒤에 삽입을 진행하게 된다. 이 경우에는 `O(1)`의 시간 복잡도를 가진다.

다만, `push`가 아니라 특정 위치에 원소를 삽입하는 경우에는 그 자리에 바로 넣을 수 없어 공간을 만들어 주고 삽입해야 한다는 문제점을 가지고 있다. 삭제의 경우에도 마찬가지로 삭제 후 빈 공간을 제거하는 작업을 거치게 된다.

이 경우엔 최악의 경우 `O(N)`의 시간 복잡도를 가지기 때문에 조회보다는 삽입이나 삭제가 빈번한 경우에는 `Array`가 아닌 다른 자료구조를 사용하는 편이 좋다.

#### LinkedList의 구현

```c
#include <stdio.h> // nodeList.c
#include <stdlib.h>

typedef int element;
typedef struct ListNode {
    element data;
    struct ListNode* link;
} ListNode;
```

LinkedList는 