
## Hash Table

`Hash Table`은 배열과 해시 함수를 사용하여 `Map`을 구현한 자료구조를 말한다. 대부분의 경우 상수 시간으로 데이터에 접근 가능하기 때문에 효율적이다.
### Map

`Map` 자료구조는 `key-value` 쌍을 저장하는 추상 데이터 타입이다. 같은 `key`를 저장하지 않는 게 가장 큰 특징이다. 일반적으로 유일한 값을 `key`로, 그와 매칭되는 값을 `value`로 매칭시키는 경우나 카운팅 등의 경우에서 유용하게 사용될 수 있다.

### Hash Function

해시 함수는 임의의 크기를 가지는 타입의 데이터를 고정된 크기를 가지는 타입의 데이터로 변환하는 함수를 말한다. `Hash Table`에서는 임의의 데이터를 정수로 변환하는 함수가 된다.

### 다시 Hash Table

그래서 이 `Hash Table`은 어떻게 동작하는가를 보면, 우선 `key`를 `Hash Function`을 통해 정수로 변환한 후에 배열에 저장한다. 다만 배열에는 길이가 존재하기 때문의 배열의 길이를 `capacity`라고 하고, `hash % capacity`와 같이 연산하여 나온 결과의 인덱스에 데이터를 저장한다. 이 때, 저장되는 공간을 해시 버킷이라고 한다.

그런데 이 경우 `Hash collision(해시값 충돌)`이 일어날 수 있다. 해싱 알고리즘의 문제로 `key`가 다른데도 `hash`가 같을 수도 있고, `key`도 다르고 `hash`도 다른데 `hash % capacity`가 같아서 문제가 발생할 수도 있다.

### Hash collision 해결 방법

#### Separate chaining

`Separate chaining(개별 체이닝)`은 위에서 말했던 충돌 상황을 해결하기 위한 방법 중 하나다. 이 방법은 `Linked `

#### Open addressing