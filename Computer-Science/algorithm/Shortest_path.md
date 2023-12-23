
## Dijkstra Algorithm

`Dijkstra(다익스트라 또는 데이크스트라) Algorithm`은 그래프 상에서 한 정점에 대해서 모든 정점까지의 최단 경로를 찾는 데 사용되는 알고리즘이다. 이 알고리즘은 단순한 최단 경로보다 특히 다른 정잠까지의 모든 최단 경로를 찾는 부분에서 더욱 유용하다. 다만 이 알고리즘은 간선의 가중치가 음수일 때엔 사용할 수 없다. 

이 알고리즘은 `Dynamic Programming`과 유사한 부분이 있는데, 둘 다 최적 부분 구조를 가진다. 또한 `Memoization`을 통해 중복 계산을 피한다. 이와 같은 방식으로 부분 문제를 통하여 전체 문제의 최적해를 구한다. 이를 이해하면 크게 어렵지 않게 이해할 수 있다.

`Priority Queue`를 사용하여 구현한 `Dijkstra Algorithm`은 `O((V + E) * logV`의 시간 복잡도를 가진다.

### 동작 방식

1. 시작 정점을 선택한다. (최단 경로의 출발점)
2. 시작 정점부터의 최단 경로를 저장하는 배열을 설정한다.
   -> 시작 정점에는 0을, 그 외의 정점에는 가능한 큰 값을 넣는다. (일반적으로 언어에서 제공하는 Infinity 또는 타입의 max값을 넣는다.)
3. 지금까지 확인한 경로 중 최단 경로를 가진 정점을 선택한다.
4. 선택한 정점을 통해 갈 수 있는 모든 인접한 정점들에 대해 최단 경로를 최신화한다. 
   -> 기존 경로와 새로 확인한 경로를 비교하여 작은 값을 최단 경로로 한다.
5. 방문하지 않은 모든 노드에 대해 반복한다. 


```python
import heapq

def dijkstra(graph, start):
    # 시작 정점으로부터의 최단 거리를 저장하는 딕셔너리
    distances = {vertex: float('infinity') for vertex in graph}
    distances[start] = 0

    # 우선순위 큐를 이용하여 정점과 해당 정점까지의 현재까지의 최단 거리를 저장
    priority_queue = [(0, start)]

    while priority_queue:
        current_distance, current_vertex = heapq.heappop(priority_queue)

        # 우선순위 큐에 저장된 거리가 현재 거리보다 크면 무시
        if current_distance > distances[current_vertex]:
            continue

        # 현재 정점에서 이동 가능한 인접 정점들을 순회
        for neighbor, weight in graph[current_vertex].items():
            distance = current_distance + weight

            # 더 짧은 경로를 발견하면 업데이트
            if distance < distances[neighbor]:
                distances[neighbor] = distance
                heapq.heappush(priority_queue, (distance, neighbor))

    return distances

# 그래프 예제
graph = {
    'A': {'B': 1, 'C': 4},
    'B': {'A': 1, 'C': 2, 'D': 5},
    'C': {'A': 4, 'B': 2, 'D': 1},
    'D': {'B': 5, 'C': 1}
}

start_vertex = 'A'
result = dijkstra(graph, start_vertex)

print(f"최단 거리 결과: {result}")

```