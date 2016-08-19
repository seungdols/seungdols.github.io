---
layout: "post"
title: "Top 10 Mistakes Java Developers Make"
description:
date: "2016-07-15 22:44"
tags: [java, programming]
comments: true
---

## Top 10 Mistakes Java Developers Make

 [Top 10 Mistakes Java Developers Make](http://www.programcreek.com/2014/05/top-10-mistakes-java-developers-make)

자바 개발자가 가장 자주 실수하는 10가지에 대해 알아보자.

#### 1. ConvertArray To ArrayList

 배열을 ArrayList로 변환할때, 개발자들은 종종 이렇게 한다 :

```java
List<String> list = Arrays.asList(arr);
```

Arrays.asList() Arrays 안에 있는 private static class ArrayList를 반환 할 것이다.

그리고 그것은 java.util.ArrayList 클래스 안에 있는 것이 아니다.

그것은 java. util.Arrays.ArrayList 클래스 `set() , get (), contains()` 메소드들을 가지고 있다.

그러나 원소들을 추가하기 위해서 어떤 메소드도 가질 수 없다.

그래서 그것의 size는 고정 된다. 진짜 ArrayList를 생성 하고 싶다면, 당신은 이렇게 해야 한다.

```java
ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(arr));
```

 ArrayList의 생성자는 Collection Type을 수용 할 수 있고, 또한 java.util.Arrays.ArrayList에 대한 부모타입이다.



#### 2. Check if an Array Contains a Value

  개발자들은 종종 이렇게 한다.  

```java
Set<String> set = new HashSet<String>(Arrays.asList(arr));
return set.contains(targetValue);
```

그 코드 작업들은 그러나 list를 set으로 변환하기 위해서 필요하지 않다. list를 set으로 변환하는 것은 긴시간을 요구된다.
그것을 간단하게 할 수 있다.  

```java
Arrays.asList(arr).contains(targetValue);
```
or
```java
for(String s: arr){ if(s.equals(targetVaule)) return true; } return false;
```

두 번째 코드 보다 첫번째 코드가 더 읽기 쉽다.



#### 3. Remove an Element from a List Inside a Loop

반복하는 동안 원소들을 지우는 코드에 대해 생각해보자.

```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a","b","c","d"));
for ( int i = 0; i <list.size(); i++ ){
    list.remove(i);
}
System.out.println(list);
```

위 코드의 결과는

```bash
[b, d]
```

이 메소드에는 심각한 문제가 있다.

원소가 지워질때, List의 size는 감소하고, 첨자는 변화한다.
그래서 만약 루프안에서 당신이 원하는 여러 원소들을 지우고 싶다면 첨자를 사용하는 방법은 제대로 동작하지 않을 것이다.

당신은 루프안에서 원소들을 옳은 방법으로 삭제하는 것은 반복자를 사용하는 것으로 알았을 것이다.
그리고 당신은 자바에서 `foreach loop`를 반복자처럼 동작하는 것으로 알았다. 그러나 정확히 그렇지 않다. 아래의 코드의 경우를 보자.


```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a,"b","c","d"));

for (String s : list ) {
    if ( s.equals("a"))
        list.remove(s);
}
```

그것은 [ConcurrentModificationException](http://www.programcreek.com/2014/01/java-util-concurrentmodificationexception/)을 던질 것이다.

다음 코드를 보자 :

```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("a","b","c","d"));
Iterator<String> iter = list.iterator();
while(iter.hasNext()){
    String s = iter.next();
    if(s.equals("a")){
        iter.remove();
    }
}
```

`remove()`를 호출하기 전에 .next() 한다. foreach loop에서 컴파일러는 원소를 지우는 것 후에 .next() 호출을 만들 것이다. 그것은 ConcurrentModificationException을 발생시킨다. 당신은 [ArrayList.iterator()](http://www.programcreek.com/2014/01/deep-understanding-of-arraylist-iterator/)의 소스코드를 살펴 볼 수 있다.



#### 4. Hashtable vs HashMap

Hashtable은 알고리즘 규칙에 의한 `자료 구조의 이름`이다. 그러나 자바에서 자료 구조의 이름은 HashMap이다.

Hashtable과 HashMap은 주요한 하나가 다른데 그것은 Hashtable은 동기화되어 있다. 그래서 자주 당신은 Hashtable은 필요 하지 않고, 대신에 **HashMap**을 사용한다.

[HashMap vs. TreeMap vs. Hashtable vs. LinkedHashMap](http://www.programcreek.com/2013/03/hashmap-vs-treemap-vs-hashtable-vs-linkedhashmap/)

[Top 10 questions about Map](http://www.programcreek.com/2013/09/top-9-questions-for-java-map)



#### 5. Use Raw Type of Collection

자바에서 raw type과 unbounded wildcard type은 쉽게 함께 섞인다. Set을 예로 들면, Set은 raw type, while Set<?>은 **unbounded wildcard type**이라고 할 수 있다.

다음의 코드로 LIst 의 parameter로 raw type을 사용하는 것을 고려해보자.

```java
public static void add(List list, Object o){
    list.add(o);
}
public static void main(String[] args){
    List<String> list = new ArrayList<String>();
    add(list.10);
    String s = list.get(0);
}
```

 이 코드는 예외를 발생 시킨다.

```bash
Exception in thread "main" java.lang.ClassCastException: java.lang.Integer cannot be cast to java.lang.String at ...
```

raw type 콜렉션을 사용하는 것은 raw type 콜렉션들은 generic type 검사하는 것 그리고 안전성을 생략하기에 위험하다.

그들의 1Set, Set<?>1 그리고 1Set<Object>1 는 큰 차이들이 있다. [Raw type vs Unbounded wildcard](http://www.programcreek.com/2013/12/raw-type-set-vs-unbounded-wildcard-set/) and [Type Erasure](http://www.programcreek.com/2011/12/java-type-erasure-mechanism-example/)을 확인해보자.

**Java Generic의 유일한 단점인 Generic Erasure에 대한 것 같습니다. Generic Erasure에 대한 것은 블로그 링크를 첨부합니다.**
[Java-Generic_Erasure](http://seungdols.tistory.com/entry/Java-Generic-Erasure)



#### 6. Access Level

 개발자들은 종종 클래스 필드를 위해 public을 사용한다. 그것은 필드 값을 쉽게 얻기 위한 직접적인 참조하는 것이며, 그런데 이것은 매우 좋지 않은 설계이다. 좋은 설계는 멤버들에 접근하는데에 낮은 가능성을 부여하는 것이다.  [public, default, protected, and private](http://www.programcreek.com/2011/11/java-access-level-public-protected-private/)

접근 제한자에 대해 이야기 하는 단락인 것 같습니다. 이 부분은 객체지향의 특징인 캡슐화에 대한 이야기로 이어지게 됩니다.



#### 7. ArrayList vs. LinkedList

 개발자들은 ArrayList와 LinkedList의 차이점을 잘 알지 못할 때, 그들은 종종 ArrayList를 사용하며 이유는 그것이 친숙하기때문이다.
 그런데, 그들 사이에 **큰 성능의 차이**가 존재한다. 실제로 만약 추가/삭제 기능들과 무작위접근 기능이 없는 큰 수의 경우라면 LinkedList가 우선적이다. 둘의 성능적인 정보를 얻고 싶으면 [ArrayList vs. LinkedList](http://www.programcreek.com/2013/03/arraylist-vs-linkedlist-vs-vector/) 확인 해보자.  

자료구조를 생각해보면, 차이를 알 수 있겠죠 ??



#### 8. Mutable vs. Immutable

변경불가능 객체는 간결함, 안정성 등에 많은 이점이 있다. 그러나 그것은 각 값들이 구별되야하므로 객체는 분리 되어야 하고, 많은 객체들은 **GC**의 비싼 비용을 발생 할 수 있다. 변경 가능과 변경 불가능 사이에서 선택 할 때 균형을 맞춰야 한다.

일반적으로, 변경 가능 객체들은 많은 임시 객체들을 생산하는 것을 피하기위해 사용 된다. 최고의 예 하나만 들면, 많은 **Strings**을 연결하는 것이다. 만약 당신이 변경불가능 객체를 사용한다면, 당신은 즉각 GC의 대상이 되는 수 많은 객체들을 생산 할 수 있다. 이것은 _시간과 CPU의 성능을 낭비하고_, 옳은 해결법은 예를들면 StringBuilder 같은 변경 가능 객체를 사용하는 것이다.  

```java
String result="";
for(String s: arr){
    result = result + s;
}
```

 변경 가능한 객체가 바람직한 또 다른 상황이 있다. 예를 들어 메소드로 변경 가능한 객체를 전달하면 많은 구문을 통해 건너 뛰는 것 없이 복수의 결과를 수집 할 수 있습니다.
  ( For example passing mutable objects into methods lets you collect multiple results without jumping through too many syntactic hoops.)

  또 다른 예는 정렬과 필터링이 있다. 당연하게도 당신은 기존 콜렉션를 이용하는 정렬된 하나를 반환하는 메소드를 만들 수 있고, 그리고 그것은 극도로 큰 콜렉션들을 낭비할 수 있다.  

[Why String is Immutable?](http://www.programcreek.com/2013/04/why-string-is-immutable-in-java/)



#### 9. Constructor of Super and Sub

![constructor](http://www.programcreek.com/wp-content/uploads/2013/04/Implicit-super-constructor-is-undefined-for-default-constructor.png)

 이 오류 모음은 발생한다 이유는 **기본 부모 생성자**가 정의되지 않았다.

 자바에서 만약 한 클래스가 생성자를 정의하지 않으면, 컴파일러가 클래스를 위한 인자 없는 기본의 생정자를 삽입 해줄 것이다.

 만약 부모클래스에 생성자가 정의 되어 있다면, 이 경우 `Super(String s)` 컴파일러 인자 없는 기본 생성자를 삽입 해주지 않을 것이다. 이 상황이 위의 부모 클래스의 상황이다.

서브 클래스의 생성자는 둘 다 인자가 있거나 아무것도 없고, 인자가 없는 부모 생성자를 호출하게 될 것이다.

컴파일러가 서브클래스의 2 개의 생성자에 super() 를 삽입하기 위해 노력하는 동안 그러나 부모는 **기본 생성자**가 정의되어 있지 않아서 컴파일러는 에러 메시지를 출력한다.

이 문제를 해결하기 위해 간단하게

1) `Super()` 생성자를 부모 클래스에 추가한다.

```java
public Super(){
    System.out.println("Super");
}
```

또는

2) 스스로 정의된 부모 생성자를 지운다.
3) 하위 클래스 생성자에 `supuer(value)`를 추가한다.

[Constructor of Super and Sub](http://www.programcreek.com/2013/04/what-are-the-frequently-asked-questions-about-constructors-in-java/)

#### 10. "" or Constructor ?

문자열은 2가지 방법으로 생성 될 수 있다.

```java
//1. use double quotes
String x = "abc";
//2. use constructor
String y = new String("abc");
```

무엇이 다를까 ?

다음의 예들을 제공 할테니 빨리 대답해보자.

```java
String a = "abcd";
String b = "abcd";
System.out.println(a == b);//True
System.out.println(a.equals(b));//True

String c = new String("abcd");
String d = new String("abcd"):
System.out.println(c == d); // false
System.out.println(c.equals(d));
```

 메모리상에서 어떻게 할당되는지 더 알고 싶다면, [remove the self-defined Super constructor](http://www.programcreek.com/2014/03/create-java-string-by-double-quotes-vs-by-constructor/) 확인해보자.
