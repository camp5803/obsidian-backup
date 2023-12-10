
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

`ArrayList`는 `Array` 기반으로 구현한 List이기 때문에 `Array`의 모든 특성을 가진다. 

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

`LinkedList`는 포인터 기반으로 구현된 자료구조이다. 원소와 포인터만 존재하는 구조로 여러 가지의 `ListNode`가 모여 하나의 `LinkedList`가 된다.

#### LinkedList로 삽입 또는 삭제를 하는 경우

```c
ListNode *insert_first(ListNode *head, int value) {
    ListNode *p = (ListNode *)malloc(sizeof(ListNode));
    p->data = value;
    p->link = head;
    head = p;
    return head;
}

ListNode *insert(ListNode* head, ListNode *pre, element value) {
    ListNode *p = (ListNode *)malloc(sizeof(ListNode));
    p->data = value;
    p->link = pre->link;
    pre->link = p;
    return head;
}

ListNode *delete_first(ListNode *head) {
    ListNode *removed;
    if (head == NULL) return NULL;
    removed = head;
    head = removed->link;
    free(removed);
    return head;
}
```

`LinkedList`는 `Array`와 다르게 유동적으로 사용할 수 있는 자료구조인데, 필요할 때마다 새로운 노드를 생성하여 붙일 수도 있고 노드를 제거할 수도 있다. 

연결을 통하여 구현된 구조이기 때문에 원소를 넣고자 하는 위치에 있는 노드의 연결을 끊고 새로운 노드에 연결을 하면 끝이며, 삭제의 경우에도 마찬가지로 삭제할 노드의 연결을 끊고 `free()`하면 되기 때문에 간단하다. 이 경우 `O(1)`의 시간 복잡도를 가진다.

#### LinkedList로 인덱싱을 하는 경우

`LinkedList`는 `ArrayList`와 다르게 순서는 존재하나 인덱스가 존재하지 않는다. 즉, 인덱싱이 불가능하다는 것이다. 
그래서 조회의 경우에는 특정 원소를 찾는 방법으로 조회가 가능하며, 최악의 경우에는 모든 노드를 확인하여야 하기 때문에 `O(N)`의 시간 복잡도를 가질 수 밖에 없다.

#### LinkedList의 문제점

이론적으로는 목적에 따라 `LinkedList`도 좋은 자료구조라고 볼 수 있으나, 일반적으로 삽입 또는 삭제 이전엔 원하는 위치를 파악해야 한다는 맹점을 가지고 있다. 그렇다면 정말 특수한 경우가 아닌 이상 삽입과 삭제에서도 `O(N)`의 시간 복잡도를 가질 수 밖에 없다는 것이다.

결론적으로 **정말 특수한 경우가 아니고서야** `LinkedList`는 삽입, 삭제, 조회 모든 경우에서 `O(N)`의 시간 복잡도를 가지는 자료구조라고 볼 수 있다.

#### 번외

Python의 `List` 자료구조는 `ArrayList`와 `LinkedList`의 특징을 섞어서 만든 자료구조인데, 엄연히 `ArrayList`의 구조를 가지고 구현되었다. 
즉, 최초 생성 시 어느 정도의 길이를 가진 `List`가 생성되는데, 특정 길이를 넘어서면 재 할당을 하여 유동적으로 길이를 늘린다고 하는데 이 경우 시간 복잡도는 `O(logN)`이라고 한다. 