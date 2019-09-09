---
title: ConcurrentHashMap
description: ConcurrentHashMap
categories:
 - java
tags: java
---

## Map
우리가 흔히 쓰는 자료구조에는 HashMap이 있습니다. Map에서 테이블은 bucket으로 나누어져있고, Key-value 쌍을 bucket에 저장합니다. 자료를 저장HashMap은 저장과 수정 연산이 상수 시간, 즉 O(1)에 일어납니다.
Java에서는 [java.util.Map](https://docs.oracle.com/javase/8/docs/api/java/util/Map.html) 인터페이스를 제공합니다. 이 Map 인터페이스는 다른 List나 Set 인터페이스와 다르게 Collection 인터페이스를 상속받고 있지 않습니다. Map 인터페이스를 상속받는다면 Key-value 구조를 구축할 수 있습니다.

<br/>

## HashTable
```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {
        
    public synchronized boolean isEmpty() {
        ...
    }

    public synchronized V get(Object key) {
        ...
    }

    public synchronized V put(K key, V value) {
        ...
    }
}
```
HashTable은 key, value에 `null`을 입력할 수 없습니다. 또한, 대부분의 public 메서드는 synchronized로 선언되어 있습니다. synchronized로 선언된 메서드의 경우 멀티쓰레드 환경에서 Thread-Safe하게 작동합니다. 즉, 메서드를 호출하기 전에 쓰레드간 락을 이용해서 한 번에 하나의 스레드만 접근이 가능합니다. 따라서, 하나의 스레드가 해당 메서드 실행을 완료해야지만 다른 스레드가 그 메서드를 실행할 수 있게 됩니다. 따라서, 싱글 스레드 환경이나 동기화 보장일 필요 없는 경우 HashMap을 사용하는게 더 좋습니다.

<br/>

그렇다면 멀티쓰레드 환경에서 동기화가 보장되어야 한다면 HashTable을 사용해야 할까요? HashTable은 동기화 비용이 높기 때문에 HashMap을 감싸서 사용하는 형태가 더 선호되는데요
```java
Map m = Collections.synchronizedMap(new HashMap(...));
```

<br/>


<br/>

## HashMap
```java
public class HashMap<K,V> extends AbstractMap<K,V>
    implements Map<K,V>, Cloneable, Serializable {

    public boolean isEmpty() {
        ...
    }

    public V get(Object key) {
        ...
    }

    public V put(K key, V value) {
        ...
    }
}
```
반면, HashMap은 key, value에 `null`을 입력할 수 있습니다. 또한, 모든 public 메서드에 synchronized 선언이 없습니다. 멀티 쓰레드 환경에서 여러 쓰레드가 동시에 데이터를 조작한다면 데이터의 값이 유효하지 않을 수 있습니다. 다만, 이 동기화 락에는 높은 비용이 요구되는데 HashMap은 이를 사용하지 않으니 HashTable 보다 훨씬 빨리 수행됩니다.

<br/>

그렇다면 멀티스레드 환경에서 동기화가 보장이 되어야 한다면 HashTable을 사용해야 할까요? 멀티스레드 환경이라고 해도 HashTable은 너무 느리기 때문에 HashMap을 wrapper하는 형태로 많이 사용합니다.
```java
Map m = Collections.synchronizedMap(new HashMap(...));
```

## ConcurrentHashMap
```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {

    public boolean isEmpty() {
        ...
    }

    public V get(Object key) {
        ...
    }

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        
        ...
        synchronized (f) {...}
            
        ...
    }
}
```
ConcurrentHashMap은 HashMap이 멀티스레드 환경에서 동기화가 보장되지 않는 단점을 보완한 클래스입니다. 하지만, HashMap과는 달리 key, value에 `null`값이 허용되지 않습니다.
클래스를 확인해보면 값을 변경하는 public 메서드가 synchronized로 선언이 되어 있는게 아니라, 이 메서드에서 호출하는 메서드의 내부에 synchronized 로직이 있습니다.

<br/>


### 참조
- https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentMap.html
- https://en.wikipedia.org/wiki/Hash_table
- https://www.baeldung.com/java-concurrent-map
- https://jdm.kr/blog/197
- http://blog.breakingthat.com/2019/04/04/java-collection-map-concurrenthashmap/