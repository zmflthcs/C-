#Collection 정리
## Generic
System.Collections.Generic으로 List<T>, Dictionary<TKey, TValue>, HashSet<T>

## Non Generic
System.Collections 로 ArrayList, HashTable등 object 기반으로 캐스팅 필요. legacy 코드에 주로 쓰임.
캐스팅이 필요하여 성능 저하

### 인터페이스 계층
1. IEnumerable<T>
Foreach로 순회 가능한 최소 단위 인터페이스

2. ICollection<T> : IEnumerable
Count, Add/Remove, Contains 등을 제공하는 컬렉션 기본단위

3. IList<T>: IEnumerable<T>, ICollection<T>
인덱스로 접근 가능한 가변 리스트

4. ISet<T> : IEnumerable<T>, ICollection<T>
집합 연산(합집합, 교집합, 중복 제거)에 특화

5. IDictionary<TKey,TValue>: ICollection<KeyValuePair<TKey, TValue>>, IEnumerable<KeyValuePair<TKey, TValue>>
키-값(key-value) 컬렉션


### 자주 쓰는 일반 컬렉션
1. List<T> : IList<T>
동적 배열: 인덱스 접근, 순차 탐색에 강함.
중복 허용, 정렬/검색 메서드 제공(Sort, BinarySearch 등

2. LinkedList<T>: ICollection<T>, IEnumerable<T>
이중 연결 리스트; 중간 삽입/삭제가 많고, 순차 방문 위주일 때 유리

3. HashSet<T> / SortedSet<T> : ISet<T>
HashSet<T>: 순서 없고 중복 허용하지 않는 집합; Contains가 매우 빠름.
​SortedSet<T>: 정렬된 집합; 항상 정렬 상태 유지.

4. Queue<T> / Stack<T>
Queue<T>: FIFO(선입선출) 구조; Enqueue/Dequeue.
​Stack<T>: LIFO(후입선출) 구조; Push/Pop.

5. Dictionary<TKey,TValue> (IDictionary<TKey,TValue>)
가장 기본적인 해시 기반 맵; 키로 빠르게 값 조회.
키는 중복 불가, 값은 중복 가능.

6. SortedDictionary<TKey,TValue> / SortedList<TKey,TValue>
SortedDictionary: 트리 기반, 키 정렬 유지, 삽입/삭제가 빈번할 때.
SortedList: 내부 배열 + 키 정렬; 요소 수가 작고 조회가 많은 경우 효율

### Thread Safe 컬렉션

스레드 안전 컬렉션
System.Collections.Concurrent
ConcurrentDictionary<TKey,TVal>, ConcurrentQueue<T>, ConcurrentStack<T>, ConcurrentBag<T> 등 제공.
여러 스레드가 동시에 Add/Remove 해도 추가 락 없이 안전하게 동작하도록 설계.
