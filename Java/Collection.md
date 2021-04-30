# Java Collection Framework (JCF)
Java에서 데이터를 저장하는 기본적인 자료구조들을 한 곳에 모아 관리하고 편하게 사용하기 위해서 제공하는 것을 의미한다.
즉, 데이터를 담는 그릇들에 대한 정의를 모아놓은 프레임워크이다. 크게 List, Set, Map 3가지로 분류할 수 있다.
```
           ┌ List  ┬  LinkedList
           |       ├  Stack
Collection ┤       ├  Vector
           |       └  ArrayList
           └ Set   ┬  HashSet - LinkedHashSet
                   └  SortedSet - TreeSet
           
           ┌ Hashtable       
           |       
       Map ┼ HashMap - LinkedHashMap
           |
           └ SortedMap - TreeMap
```

### List Interface
List interface는 순서를 유지하는 데이터의 집합이며 데이터의 중복을 허용한다.
구현체로는 크게 4가지를 가지고 있다.
1. LinkedList : 양방향의 포인터 구조로 ```데이터 삽입/삭제가 빈번할 경우 유용하며, 검색은 ArrayList에 비해 비효율적이다.```
2. Stack : 한 쪽 끝에서만 자료를 넣고 뺄 수 있는 ```LIFO(Last In First Out)```형식의 자료 구조.
3. Vector : 동적 크기 배열, 배열과 마찬가지로 index로 배열에 접근 가능하며, ```동기화(Thead Safe)```되어 있어, 한번에 하나의 쓰레드만 벡터의 메소드를 호출할수 있다.
            ```멀티 쓰레드 환경이 아니라면 ArrayList를 사용하는 것이 성능에 더 좋다.```
4. ArrayList : 단방향 포인터 구조로 각 데이터의 index를 가지고 있으며, ```조회 기능에 성능이 뛰어나```지만 ```데이터 삽입/삭제가 빈번할 경우 비효율적이다.```(비어있는 index를 기준으로 뒷편의 데이터들을 복사하여 앞으로 밀어주는 작업이 필요하다.)

### Set Interface
Set interface는 순서를 유지하지 않는 데이터의 집합이며 데이터의 중복을 허용하지 않는다.
구현체로는 크게 2가지를 가지고 있다.
1. HashSet : 해시 알고리즘을 사용한 가장 빠른 임의 접근 속도를 가지며, ```순서를 예측 할 수 없다.``` 만약 저장 순서를 유지하고 싶다면 ```LinkedHashSet```을 사용할 수 있다.
2. TreeSet : ```이진 검색 트리(BST / Binary Search Tree)``` 형태로 데이터를 저장하는 클래스이며 ```검색에 뛰어난 성능을 보이며```, BST의 성능을 향상시킨 레드-블랙 트리로 구현되어있다.
             HashSet과 마찬가지로 ```저장 순서를 유지하지 않는다.```
   
### Map Interface
Map interface는 Key-Value 쌍으로 이루어진 데이터의 집합이며 키는 중복이 허용되지 않지만 값은 중복이 허용되며 순서를 유지하고 있지 않다.
구현체로는 크게 3가지를 가지고 있다.
1. HashMap : ```중복과 순서가 허용되지 않고``` Value에 ```null을 넣을수 있다.```
2. TreeMap : 키 값이 자연 순서 대로 ```오름차순 정렬``` 되어 Key-Value쌍으로 저장된다. 정렬되어 있기 때문에 ```검색이 빠르다.``` 
3. HashTable : HashMap 보다 느리지만 ```동기화(Thread Safe)를 지원```하며 HashMap과 달리 ```Value에 null을 넣을수 없다.```

### 시간 복잡도

- ArrayList vs LinkedList

    | |ArrayList|LinkedList
    |:---:|:---:|:---:|
    |탐색|O(1)|O(n)|
    |삽입|O(n)|O(1): 맨앞, 맨뒤 <br>O(n): 탐색을 통한 중간 삽입|
    |삭제|O(n)|O(1): 맨앞, 맨뒤 <br>O(n): 탐색을 통한 중간 삭제|
    ※ 데이터 조회가 자주 이루어질 경우 ```ArrayList```, 데이터 삽입 삭제가 자주 이루어질 경우 ```LinkedList```를 사용하는 것이 유리하다.  


- HashSet vs TreeSet vs LinkedHashSet
  
    | |HashSet|TreeSet|LinkedHashSet|
    |:---:|:---:|:---:|:---:|
    |탐색|O(1)|O(logn)|O(1)|
    |삽입|O(1)|O(logn)|O(1)|
    |삭제|O(1)|O(logn)|O(1)|

    | |HashSet|TreeSet|LinkedHashSet|
    |:---:|:---|:---|:---|
    |성능|가장 좋음|삽입 순서를 유지하기 위하여 포인터 값을 저장하므로 HashSet보다 약간 느리다.|삽입과 삭제 오퍼레이션 후 재정렬해야하므로 셋 중에서 가장 느리다.|
    |사용 용도|순서를 유지할 필요가 없는 경우|삽입 순서를 유지하고 싶은 경우|Comparator에 의해 원소들을 정렬하고 싶은 경우|
  
- HashMap vs LinkedHashMap vs TreeMap vs HashTable
    
    | |HashMap|LinkedHashMap|TreeMap|HashTable|
    |---|:---:|:---:|:---:|:---:|
    |탐색|O(1)|O(1)|O(logn)|O(1)|
    |삽입|O(1)|O(1)|O(logn)|O(1)|
    |삭제|O(1)|O(1)|O(logn)|O(1)|

    | |HashMap|LinkedHashMap|TreeMap|HashTable|
    |---|:---|:---|:---|:---|
    |성능|가장좋음|삽입순서를 유지하기 위하여 포인터값을 저장흐므로 HashMap보다 약간 느리다.|삽입과 삭제 오퍼레이션 후 재정렬해야하므로 가장 느리다.|동기화되므로 HashMap보다 약간 느리다.|
    |사용용도|가장 좋은 퍼포먼스를 보이르모 일반적으로 권장된다.|삽입순서를 유지하고 싶은경우|Comparator에 의해 원소들을 정렬하고 싶은 경우|Thread Safe가 요구될 경우|

### ref
- [Java Collection Framework(JCF)의 이해](https://dodo-factory.tistory.com/6)
- [자료구조 요약: 구조/성능/용도](https://gem1n1.tistory.com/97#set)
- [오라클 공식 문서 ArrayList](https://docs.oracle.com/javase/8/docs/api/java/util/ArrayList.html)
- [오라클 공식 문서 LinkedList](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedList.html)
- [오라클 공식 문서 HashSet](https://docs.oracle.com/javase/8/docs/api/java/util/HashSet.html)
- [오라클 공식 문서 TreeSet](https://docs.oracle.com/javase/8/docs/api/java/util/TreeSet.html)
- [오라클 공식 문서 LinkedHashSet](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashSet.html)
- [오라클 공식 문서 HashMap](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)
- [오라클 공식 문서 LinkedHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/LinkedHashMap.html)
- [오라클 공식 문서 TreeMap](https://docs.oracle.com/javase/8/docs/api/java/util/TreeMap.html)
- [오라클 공식 문서 HashTable](https://docs.oracle.com/javase/8/docs/api/java/util/Hashtable.html)
