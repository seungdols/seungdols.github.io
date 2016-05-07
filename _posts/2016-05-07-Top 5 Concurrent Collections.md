---
layout: post
title:  "Top 5 Concurrent Collections from JDK 5 and 6 Java Programmer Should Know"
description: "번역글 - 다섯가지 병행 컬렉션 - Java5 and 6"
tags: [java, collections, concurrency]
comments: true
---

## Top 5 Concurrent Collections from JDK 5 and 6 Java Programmer Should Know

원문은 이 곳에서 보실 수 있습니다. [concurrent-collections-from-jdk-56-java-example-tutorial](http://javarevisited.blogspot.kr/2013/02/concurrent-collections-from-jdk-56-java-example-tutorial.html)


Java 5와 java 6에서 표준 동기화의 병행 대안으로 [ArrayList](http://javarevisited.blogspot.com/2011/05/example-of-arraylist-in-java-tutorial.html), [Hashtable](http://javarevisited.blogspot.sg/2012/01/java-hashtable-example-tutorial-code.html) 그리고 [Synchronized HashMap](http://javarevisited.blogspot.com/2011/05/example-of-arraylist-in-java-tutorial.html)  몇개의 새로운 콜렉션이 추가가 되었다.
많은 자바 프로그래머는 확장 가능한 고 성능의 자바 응용프로그램을 제작하는데 활용 할 수 있는 새로운 기능성의 집합의 전체 새로운 콜렉션 클래스들과 java.util.concurrent 패키지에 여전히 익숙하지 않다.
우리가 유용한 콜렉션 클래스의 일부를 [ConcurrentHashMap](http://javarevisited.blogspot.sg/2011/04/difference-between-concurrenthashmap.html), [BlockingQueue](http://javarevisited.blogspot.com/2012/12/blocking-queue-in-java-example-ArrayBlockingQueue-LinkedBlockingQueue.html) 예로 들고 매우 유용한 기능인 동시성 자바 프로그램을  구축하는 데 자바 튜토리얼에서 제공 할 것이다.
그런데 이 글은 모든 병행 콜렉션들의 각 특징을 설명하는 포괄적인 기사가 아니다. 대신에 나는 그들이 교체하거나 콜렉션 클래스 대안을 제공하기하는 그것에 이유를 말하려고 한다. 유용한 java.util.concurrent collections들의 주요 핵심을 표시하는 동안 그것을 짧고 간결하게 유지하게 할 생각이다.

#### 1. ConcurrentHashMap

ConcurrentHashMap은 의심할 것 없이 가장 인기 있는 콜렉션 클래스로 java 5에서 소개가 되었고, 우리는 이미 그것을 거의 사용하고 있다.
ConcurrentHashMap은 [HashTable 또는 Synchronized Map](http://javarevisited.blogspot.com/2011/04/difference-between-concurrenthashmap.html) 과 함께 세밀한 Loking을 구현하기 위해서 높은 수준의 병행성을 지원하는 것을 목표로 제공 된다.
다중 reader는 맵의 동시 레벨에 의존적인 맵의 일부가 쓰기 기능을 위해 잠겨있는 동안 동시적으로 Map에 접근 할 수 있다.
concurrentHashMap은  그들의 동기 카운터 부분보다 더 나은 확장성을 제공한다.
ConcurrentHashMap의 [반복자](http://javarevisited.blogspot.com/2011/10/java-iterator-tutorial-example-list.html)는 그것의 결과로 확장성과 성능이 더 나은 다른 lock의 요구사항을 반복하는 동안 ConcurrencModificationException의 예외를 던질 수 없으므로 [실패에 안정적](http://javarevisited.blogspot.com/2012/02/fail-safe-vs-fail-fast-iterator-in-java.html)이다.

#### 2. CopyOnWriteArrayList and CopyOnWriteArraySet

CopyOnWriteArrayList은 synchronized List의 병행대안이다. CopyOnWriteArrayList는 다중 병행 reader 그리고 전체 목록에 쓰기 작업을 변환하는 것을 허용하므로 synchronized List보다 더 나은 병행성을 제공한다. 그렇다, 쓰기 작업은  CopyOnWriteArrayList은 다중 reader와 반복자의 요구사항을 쓸 때보다 더 수행하는데 비용이 비싸다. CopyOnWriteArrayList 반복자는 그 콜렉션이 반복하는 동안 그것을 잠금하는 데에 필요로 하지 않으며 또한 ConcurrencModificationException을 던질 수 없다. ConcurrentHashMap과 CopyOnWriteArrayList은 잠금의 수준과  쓰레드에 안전함을 얻는데에 있어서 잠금 및 가변 전략을 달성하는데  Synchronized Collection처럼 제공하지 않는다. 그래서 그들은 만약 그들 성질을  요구사항들을 맞춘다면 더 좋게 수행 될 것이다. 비슷하게, CopyOnWriteArraySet은 동기화 set으로 현재 바뀌었다.

[CopyOnWriteArrayList](http://java67.blogspot.com/2012/09/what-is-copyonwritearraylist-in-java-example-vs-arraylist.html)

#### 3. BlockingQueue

BlockingQueue는 또한 java 5에서 많이 알려진 콜렉션 중 하나다. BlockingQueue는 고유하게 blocking을 지원하기 위한 put() 그리고 take() 메소드를 [생상자-소비자 디자인 패턴](http://javarevisited.blogspot.com/2012/02/producer-consumer-design-pattern-with.html)을 구현하여 쉽게 만든다. put() 메소드는 Queue가 가득 찬 동안 block 될 것이다. 만약  만약 Queue가 비어있다면 take() 메소드는 블럭 될 것이다. 자바 5 API는 [ArrayBlockingQueue 등과 LinkedBlockingQueue](http://javarevisited.blogspot.com/2012/12/blocking-queue-in-java-example-ArrayBlockingQueue-LinkedBlockingQueue.html) 등의 형태로 BlockingQueue를 두 구체적인 구현을 제공, 둘 다 요소의 FIFO 순서로 구현합니다. ArrayBlockingQueue는 배열에 의해 그리고 그것은 LinkBlockingQueue는 선택적인 경계가 있는 동안  참조된다. 자바에서 생산자 소비자 문제를 해결 하기 위해서 대기-통지 코드를 작성하는 것 대신에 BlockingQueue를 사용하는 것을 고려해야 한다. Java 5 또한 또 다른 BlockingQueue의 구현체인 어떤 것은 우선순위 그리고 유용성으로 배치되는데  만약 네가 FIFO보다 원하는 다른 순서로 요소를 처리를 하기 위해서 PriorityBlockingQueue를 제공한다.

#### 4. Deque and BlockingDeque

java 6에서 deque interface는 추가 되었다. 그리고 그것은 Queue의 양 끝과 관련이 있는 Head 그리고 tail로부터 삽입 그리고 삭제를 지원하기 위해서 Queue interface를 상속 받는다.  
java 6 또한 ArrayDeque 그리고 LinkedBlockingDeque처럼 Deque의 병행구현을 제공한다.
Deque는  Deque의 이중 소비 특성을 활용 하는 것으로 작업 부하의 일부를 다른 쓰레드로부터 Deque를 돕기 위해서 각 다른 작업 쓰레드 집합을 허용함으로 프로그램의 병렬 처리를 증가시키기 위해서 효율적이게 사용 될 수 있다. 그래서 만약 모든 쓰레드가 그들 자신의 Queue의 작업 집합을 그들의 Head로부터 소비하며, 그리고 Helper 쓰레드는 또한 tail을 거쳐 소비 되는 몇 몇의 작업 부하를 공유 할 수 있다.

#### 5. ConcurrentSkipListMap and ConcurrentSkipListSet

마찬가지로 ConcurrentHashMap은 [동기화 HashMap](http://javarevisited.blogspot.com/2010/10/difference-between-hashmap-and.html)의 병행 대안을 제공한다.
ConcurrentSkipListMap과 ConcurrentSkipListSet은 동기화를 위한 SortedMap과 SortedSet의 버전으로 병행 대안 제공한다.
예를 들어 동기화 콜렉션 안에 있는 TreeMap과 TreeSet을 사용하는 것 대신에, 당신은 java.util.concurrent의 package 에 있는 ConcurrentSkipListMap 혹은 ConcurrentSkipListSet을 사용하는 것을 고려할 수 있다.
그들은 또한 NavaigableMap과 NavigableSet에 추가하기위해서 navigation 메소드를 추가적으로 구현한다. 우리는 Java에서 [NavigableMap](http://javarevisited.blogspot.sg/2013/01/what-is-navigablemap-in-java-6-example-submap-head-tail.html)을 어떻게 사용하는지에  대한 우리의 글을 보았었다.
즉, Java 5 와 Java 6의 병행 콜렉션 클래스의 목록에 있다. 그것들은 동기 카운터부분의 병행 대안으로서 java.util.concurrent 패키지에 추가되었다. 이것은 Java 콜렉션 프레임워크로부터 다른 유명한 클래스와 함께 이들의 콜렉션 클래스를 배우는 것이 좋다.


** 번역 오류가 존재 할 수 있습니다. **



> Written with [StackEdit](https://stackedit.io/).
