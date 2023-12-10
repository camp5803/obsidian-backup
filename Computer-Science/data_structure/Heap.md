
## Heap

`Heap`은 `Binary Tree` 기반의 자료구조로, 각 노드는 오름차순 또는 내림차순의 조건을 만족하는 구조를 가진다. `Heap`은 위 조건에 따라 `최대 힙(Max Heap)` 또는 `최소 힙(Mix Heap)`으로 나뉜다. 또한 `Binary Tree`를 기반으로 하기 때문에, 중복된 원소를 허용하지 않는다. 

`최대 힙(Max Heap)`은 각 노드의 값이 그 자식 노드의 값보다 항상 커야 한다. 또한 `최소 힙(Min Heap)`은 각 노드의 값이 그 자식 노드의 값보다 항상 작아야 한다. 이러한 특성으로 `Heap` 자료구조는 정렬에 있어서 느슨한 편이다. 

그 이유는 `Heap`에서 삽입 또는 제거를 하는 과정에서 알 수 있는데, `Heapify`가 어떻게 동작하는지 알아야 한다.

```python
def push(self, value):
	self.heap.append(value)
	self.heapifyUp

def pop(self):
	root = self.heap[0] // Heap의 마지막 값을 임시 저장
	self.heap[0] = self.heap.pop() // 
	self.heapifyDown()
	return root
```

