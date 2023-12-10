
## Array, ArrayList

가장 기초적인 자료구조인 `Array`은 저장에 있어서 의도한 순서와 물리적으로 메모리에 저장되는 순서가 동일하다. 즉, 특정 인덱스의 값을 조회 시 이미 위치를 알고 있기 때문에 `(배열의 주소 + 타입 * 인덱스)` Random Access에 있어서 `O(1)`의 시간 복잡도를 가진다.

다만 배열은 유동적으로 삭제나 삽입이 어렵기 때문에, 삭제 혹은 삽입 시 배열을 다시 정렬 해야한다는 문제점을 가지고 있다. 이 경우엔 최악의 경우 `O(N)`의 시간 복잡도를 가지기 때문에 삽입이나 삭제가 빈번한 경우에는 `Array`가 아닌 다른 자료구조를 사용하는 편이 좋다.

## LinkedList

`LinkedList`는 특정 Context 안에 모든 원소를 담아둔 방식이 아닌, 원소와 원소의 연결을 통하여 구현한 `List` 자료구조의 일종이다.

`LinkedList`는 `Array`와 다르게 유동적으로 사용할 수 있는 자료구조인데, 필요할 때마다 새로운 노드를 생성하여 붙일 수도 있고 노드를 제거할 수도 있다. 연결을 통하여 구현된 구조이기 때문에 원소를 넣고자 하는 위치에 있는 노드의 연결을 끊고 새로운 노드에 연결을 하면 끝이며, 삭제의 경우에도 마찬가지로 삭제할 노드의 연결을 끊고 `free()`하면 되기 때문에 간단하다. 

`LinkedList`의 삭제 또는 삽입 시의 시간 복잡도는 `O(1)`이나, Random Access에 있어서는 연결을 계속 타고 넘어가야 하기 때문에 최악의 경우 `O(N)`의 시간 복잡도를 가진다.

## ArrayList vs LinkedList

`ArrayList`는 `Array`를 기반으로 구현한 List이기 때문에 `Array`의 모든 특성을 가진다. 이를 통해 직접 구현하여 비교하도록 한다.

```c
#include <stdio.h> // arrayList.c
#include <stdlib.h> 

#define MAX_LIST_SIZE 100

typedef int element;
typedef struct {
    element array[MAX_LIST_SIZE];
    int size;
} arrayListType;

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

element get_entry(arrayListType *L, int pos){
    if (pos < 0 || pos >= L->size)
        error("위치 오류");
    return L->array[pos];
}
```

