# Java Collections Framework — Complete Study Notes

## 1. What is the Java Collections Framework?

The **Java Collections Framework (JCF)** is a unified set of interfaces, classes, and utility methods used to store, retrieve, manipulate, and process groups of objects.

It is mainly organized around two major interface families:

- `Collection<E>` — represents a group of individual elements.
- `Map<K, V>` — represents key-value mappings and is **not** a subtype of `Collection`.

The framework provides standard data structures such as:

- `ArrayList`
- `LinkedList`
- `HashSet`
- `LinkedHashSet`
- `TreeSet`
- `PriorityQueue`
- `ArrayDeque`
- `HashMap`
- `LinkedHashMap`
- `TreeMap`
- `Hashtable`
- `ConcurrentHashMap`

`E`, `K`, and `V` are generic type parameters:

- `E` = element type
- `K` = key type
- `V` = value type

---

# 2. Java Collections Hierarchy

## 2.1 Main hierarchy

```text
                            Iterable<E>
                                |
                                v
                         Collection<E>
                  ____________|____________
                 |             |            |
                 v             v            v
              List<E>       Set<E>       Queue<E>
                 |             |            |
          _______|_______   ____|____       |
         |       |       | |    |     |     |
         v       v       v v    v     v     v
     ArrayList LinkedList Vector HashSet LinkedHashSet TreeSet
    |          |        |
    |          |        |
       |          +--------+
       |
       +---------------------------------------------+
                                                     |
                                                     v
                                                  Deque<E>
                                                     |
                                          ______________|______________
                                         |                             |
                                         v                             v
                                   ArrayDeque<E>                 LinkedList<E>

Queue implementations commonly include:

Queue<E>
   |
   +---- PriorityQueue<E>
   |
   +---- LinkedList<E>
   |
   +---- ArrayDeque<E>  (through Deque<E>)
```

## 2.2 Map hierarchy — separate from Collection

```text
                           Map<K,V>
                  ___________|________________
                 |            |               |
                 v            v               v
             HashMap     LinkedHashMap     SortedMap
                                              |
                                              v
                                           NavigableMap
                                              |
                                              v
                                           TreeMap

Other important Map implementations:

Map
 |
 +---- Hashtable
 +---- ConcurrentHashMap
 +---- EnumMap
 +---- WeakHashMap
 +---- IdentityHashMap
```

> **Important:** `Map` does not extend `Collection`. A map stores mappings (`key -> value`), while a collection stores individual elements.

---

# 3. Important Generic Syntax

A collection normally uses a generic type:

```java
ArrayList<String> names = new ArrayList<>();
HashSet<Integer> numbers = new HashSet<>();
Queue<String> queue = new LinkedList<>();
Map<Integer, String> students = new HashMap<>();
```

The generic type provides **compile-time type safety**.

```java
List<String> names = new ArrayList<>();
names.add("Likith");
// names.add(10); // Compile-time error
```

---

# 4. `Iterable` — Root of Collection Traversal

`Collection` extends `Iterable`.

```java
public interface Collection<E> extends Iterable<E>
```

The most important method inherited from `Iterable` is:

```java
Iterator<E> iterator();
```

Example:

```java
List<String> names = new ArrayList<>();
names.add("A");
names.add("B");

Iterator<String> it = names.iterator();

while (it.hasNext()) {
    System.out.println(it.next());
}
```

The enhanced `for` loop also works because collections are `Iterable`:

```java
for (String name : names) {
    System.out.println(name);
}
```

---

# 5. `Collection<E>` Interface

`Collection` is the parent interface for `List`, `Set`, and `Queue`.

```java
Collection<String> data = new ArrayList<>();
```

You usually program to the interface and instantiate a concrete implementation:

```java
List<String> list = new ArrayList<>();
Set<String> set = new HashSet<>();
Queue<String> queue = new LinkedList<>();
```

This is preferred because the variable depends on the required behavior, not on a specific implementation.

---

# 6. Common Methods for `Set`, `List`, and `Queue`

All three interfaces inherit the common methods defined by `Collection`.

## 6.1 `add(E e)`

**Purpose:** Adds one element.

```java
list.add("Java");
set.add("Java");
queue.add("Java");
```

**Parameter:** `e` — the element to add.

**Return type:** `boolean`

- Usually `true` when the collection changes.
- A `Set` may return `false` if an equivalent element already exists.
- Some queue implementations can throw an exception when insertion fails.

---

## 6.2 `addAll(Collection<? extends E> c)`

**Purpose:** Adds all elements from another collection.

```java
List<String> a = new ArrayList<>();
a.add("A");
a.add("B");

List<String> b = new ArrayList<>();
b.addAll(a);
```

**Parameter:** `c` — source collection.

**Return:** `true` if the target collection changed.

---

## 6.3 `remove(Object o)`

**Purpose:** Removes one matching element.

```java
list.remove("Java");
set.remove("Java");
```

**Parameter:** `o` — object to remove.

**Return:** `true` if an element was removed.

> For `List<Integer>`, be careful: `remove(1)` is interpreted as removing index `1` because `List` overloads `remove` with `remove(int index)`. To remove the value `1`, use `remove(Integer.valueOf(1))`.

---

## 6.4 `removeAll(Collection<?> c)`

**Purpose:** Removes all elements that are also present in another collection.

```java
set.removeAll(otherSet);
```

**Parameter:** `c` — collection containing elements to remove.

**Return:** `true` if the target collection changed.

---

## 6.5 `retainAll(Collection<?> c)`

**Purpose:** Keeps only elements that are present in the supplied collection.

```java
List<String> names = new ArrayList<>(List.of("A", "B", "C"));
names.retainAll(List.of("B", "C"));
```

Result:

```text
[B, C]
```

**Parameter:** `c` — collection of elements to retain.

---

## 6.6 `clear()`

**Purpose:** Removes all elements.

```java
list.clear();
```

**Parameters:** None.

**Return:** `void`.

---

## 6.7 `contains(Object o)`

**Purpose:** Checks whether an element exists.

```java
if (set.contains("Java")) {
    System.out.println("Found");
}
```

**Parameter:** `o` — object to search for.

**Return:** `boolean`.

---

## 6.8 `containsAll(Collection<?> c)`

**Purpose:** Checks whether all elements of another collection exist in this collection.

```java
boolean result = set.containsAll(otherSet);
```

**Return:** `true` if every supplied element is present.

---

## 6.9 `size()`

**Purpose:** Returns the number of elements.

```java
int n = list.size();
```

**Return:** `int`.

---

## 6.10 `isEmpty()`

**Purpose:** Checks whether the collection contains zero elements.

```java
if (queue.isEmpty()) {
    System.out.println("Queue is empty");
}
```

**Return:** `boolean`.

---

## 6.11 `toArray()`

**Purpose:** Converts the collection into an `Object[]`.

```java
Object[] arr = list.toArray();
```

---

## 6.12 `toArray(T[] a)`

**Purpose:** Converts the collection into a typed array.

```java
String[] arr = list.toArray(new String[0]);
```

**Parameter:** Array that controls the desired runtime array type and may receive the elements.

---

## 6.13 `removeIf(Predicate<? super E> filter)`

**Purpose:** Removes all elements matching a condition.

```java
list.removeIf(name -> name.length() < 4);
```

**Parameter:** Predicate that returns `true` for elements that should be removed.

---

## 6.14 `stream()`

**Purpose:** Creates a sequential `Stream` for functional-style processing.

```java
list.stream()
    .filter(s -> s.length() > 3)
    .forEach(System.out::println);
```

---

## 6.15 `parallelStream()`

**Purpose:** Creates a potentially parallel `Stream`.

```java
list.parallelStream().forEach(System.out::println);
```

Use parallel streams only when parallel execution is actually beneficial; they are not automatically faster for small/simple workloads.

---

## 6.16 `equals(Object o)` and `hashCode()`

Collections also inherit the standard `Object` contract through Java's type system and implementations provide equality/hash behavior appropriate to the interface.

```java
list.equals(otherList);
list.hashCode();
```

The exact equality semantics depend on the collection type. For example, `List` equality is order-sensitive, while `Set` equality is based on membership.

---

# 7. `List<E>` Interface

`List` represents an **ordered collection**.

Key properties:

- Maintains insertion order.
- Allows duplicate elements.
- Supports index-based access.
- Allows `null` in most implementations.
- Implementations differ in performance characteristics.

Common implementations:

```text
List<E>
 |
 +---- ArrayList<E>
 +---- LinkedList<E>
 +---- Vector<E>
        |
        +---- Stack<E>   (legacy class)
```

## 7.1 Creating a List

Preferred style:

```java
List<String> names = new ArrayList<>();
```

Using another implementation:

```java
List<String> names = new LinkedList<>();
```

With initial values:

```java
List<String> names = new ArrayList<>(List.of("A", "B", "C"));
```

> `List.of(...)` creates an **unmodifiable** list. It does not permit structural modification or `null` elements.

---

# 8. Specialized `List` Methods

## 8.1 `add(int index, E element)`

Inserts an element at a specific position.

```java
names.add(1, "Java");
```

**Parameters:**

- `index` — insertion position, from `0` through `size()`.
- `element` — element to insert.

**Return:** `void`.

---

## 8.2 `addAll(int index, Collection<? extends E> c)`

Inserts all elements of another collection starting at a specified index.

```java
names.addAll(1, List.of("X", "Y"));
```

---

## 8.3 `get(int index)`

Returns the element at an index.

```java
String name = names.get(0);
```

**Parameter:** `index` — zero-based index.

**Return:** element at that position.

**Common error:** `IndexOutOfBoundsException` when the index is invalid.

---

## 8.4 `set(int index, E element)`

Replaces an existing element.

```java
names.set(0, "NewName");
```

**Parameters:** index and replacement element.

**Return:** previously stored element.

---

## 8.5 `remove(int index)`

Removes and returns the element at an index.

```java
String removed = names.remove(0);
```

**Return:** removed element.

---

## 8.6 `indexOf(Object o)`

Returns the index of the first matching element.

```java
int index = names.indexOf("Java");
```

Returns `-1` when not found.

---

## 8.7 `lastIndexOf(Object o)`

Returns the index of the last matching element.

```java
int index = names.lastIndexOf("Java");
```

Returns `-1` when not found.

---

## 8.8 `listIterator()`

Returns a bidirectional list iterator.

```java
ListIterator<String> it = names.listIterator();
```

A `ListIterator` can move forward and backward and can also insert, replace, or remove elements during iteration.

---

## 8.9 `listIterator(int index)`

Creates a `ListIterator` starting at a specific position.

```java
ListIterator<String> it = names.listIterator(1);
```

---

## 8.10 `subList(int fromIndex, int toIndex)`

Returns a **view** of a portion of the list.

```java
List<String> part = names.subList(1, 3);
```

The range is:

```text
fromIndex inclusive
        |
        v
[A, B, C, D]
    ^     ^
    1     3

Result = [B, C]
```

> The upper bound is exclusive. Changes to the sublist can affect the original list because it is a view, not an independent copy.

---

# 9. `ArrayList<E>`

`ArrayList` is a resizable-array implementation of `List`.

```java
List<String> names = new ArrayList<>();
```

### Characteristics

- Excellent random-access performance: `get(index)` is generally O(1).
- Append is amortized O(1).
- Insertion/removal in the middle is generally O(n).
- Allows duplicates and `null`.
- Not synchronized.

Example:

```java
ArrayList<Integer> numbers = new ArrayList<>();
numbers.add(10);
numbers.add(20);
numbers.add(30);

System.out.println(numbers.get(1)); // 20
```

### Capacity constructor

```java
ArrayList<Integer> numbers = new ArrayList<>(100);
```

The `100` specifies initial internal capacity; it does **not** mean the list already contains 100 elements.

---

# 10. `LinkedList<E>`

`LinkedList` is a doubly linked list and implements both `List` and `Deque`.

```java
LinkedList<String> data = new LinkedList<>();
```

Because it implements `Deque`, it can be used as a queue, deque, or list.

```java
Queue<String> q = new LinkedList<>();
Deque<String> dq = new LinkedList<>();
List<String> list = new LinkedList<>();
```

### Characteristics

- Index-based access is generally O(n).
- Adding/removing at known ends is efficient.
- Higher memory overhead than `ArrayList` because nodes store links.
- Not synchronized.

---

# 11. `Vector<E>` and `Stack<E>`

`Vector` is a legacy synchronized resizable-array implementation.

```java
Vector<Integer> v = new Vector<>();
v.add(10);
```

`Stack<E>` extends `Vector` and is a legacy LIFO stack.

```java
Stack<Integer> stack = new Stack<>();
stack.push(10);
stack.push(20);
System.out.println(stack.pop()); // 20
```

For modern stack/deque behavior, prefer:

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
System.out.println(stack.pop());
```

---

# 12. `Set<E>` Interface

A `Set` represents a collection that contains **no duplicate elements** according to the set's equality semantics.

Common implementations:

```text
Set<E>
 |
 +---- HashSet<E>
 |
 +---- LinkedHashSet<E>
 |
 +---- SortedSet<E>
         |
         v
     NavigableSet<E>
         |
         v
       TreeSet<E>
```

Creating sets:

```java
Set<String> set1 = new HashSet<>();
Set<String> set2 = new LinkedHashSet<>();
Set<String> set3 = new TreeSet<>();
```

---

# 13. Specialized `Set` Methods

The basic `Set` interface itself adds very few methods beyond `Collection`. Its main specialization is **uniqueness**.

## 13.1 `Set.of(...)`

Creates an unmodifiable set.

```java
Set<String> set = Set.of("A", "B", "C");
```

Characteristics:

- No duplicates.
- Unmodifiable.
- Does not permit `null` elements.
- Iteration order should not be relied upon.

---

# 14. `HashSet<E>`

`HashSet` stores unique elements using hash-based lookup.

```java
Set<String> languages = new HashSet<>();
languages.add("Java");
languages.add("Python");
languages.add("Java");

System.out.println(languages);
```

`Java` appears only once.

### Characteristics

- No duplicate elements.
- No guaranteed iteration order.
- Average-case constant-time basic operations when hashing is well distributed.
- Allows one `null` element.
- Not synchronized.

### Important concept: `equals()` + `hashCode()`

For custom objects stored in hash-based collections, `equals()` and `hashCode()` must follow their contract.

```java
class Student {
    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (!(obj instanceof Student other)) return false;
        return id == other.id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

---

# 15. `LinkedHashSet<E>`

`LinkedHashSet` combines hash-based set behavior with predictable insertion-order iteration.

```java
Set<String> set = new LinkedHashSet<>();
set.add("C");
set.add("A");
set.add("B");

System.out.println(set); // [C, A, B]
```

Use it when you need:

- uniqueness, and
- insertion-order iteration.

---

# 16. `SortedSet<E>` and `NavigableSet<E>`

These provide sorted-set behavior.

```java
SortedSet<Integer> numbers = new TreeSet<>();
numbers.add(30);
numbers.add(10);
numbers.add(20);
```

Result:

```text
[10, 20, 30]
```

`NavigableSet` extends `SortedSet` and provides navigation methods.

Important methods:

```java
TreeSet<Integer> nums = new TreeSet<>(List.of(10, 20, 30, 40));

nums.lower(25);   // 20
nums.floor(20);   // 20
nums.ceiling(25); // 30
nums.higher(20);  // 30
```

### Meaning

| Method | Meaning |
|---|---|
| `lower(e)` | Greatest element strictly less than `e` |
| `floor(e)` | Greatest element less than or equal to `e` |
| `ceiling(e)` | Smallest element greater than or equal to `e` |
| `higher(e)` | Smallest element strictly greater than `e` |

Other useful methods:

```java
nums.pollFirst();
nums.pollLast();
nums.first();
nums.last();
```

---

# 17. `TreeSet<E>`

`TreeSet` is a sorted, navigable set, generally backed by a balanced search tree.

```java
TreeSet<Integer> numbers = new TreeSet<>();
numbers.add(50);
numbers.add(10);
numbers.add(30);
```

Iteration is sorted according to natural ordering or a supplied `Comparator`.

Custom ordering:

```java
TreeSet<String> names = new TreeSet<>(Comparator.reverseOrder());
names.add("A");
names.add("C");
names.add("B");
```

---

# 18. `Queue<E>` Interface

A `Queue` is generally used for processing elements in a queueing order, commonly **FIFO**.

```java
Queue<String> queue = new LinkedList<>();
```

Common implementations:

```text
Queue<E>
 |
 +---- LinkedList<E>
 |
 +---- PriorityQueue<E>
 |
 +---- Deque<E>
         |
         +---- ArrayDeque<E>
```

> Queue ordering is determined by the implementation. `PriorityQueue` is not FIFO; it orders elements by priority.

---

# 19. Specialized `Queue` Methods

Queue adds paired methods for insertion, inspection, and removal.

## 19.1 `add(E e)` vs `offer(E e)`

Both attempt to insert an element.

```java
queue.add("A");
queue.offer("B");
```

Difference:

| Method | Failure behavior |
|---|---|
| `add(e)` | Throws exception if insertion cannot be performed |
| `offer(e)` | Returns `false` if insertion cannot be performed |

For bounded queues, `offer()` is often preferable when you want non-exceptional failure handling.

---

## 19.2 `remove()` vs `poll()`

Both remove the queue head.

```java
String a = queue.remove();
String b = queue.poll();
```

Difference:

| Method | Empty queue behavior |
|---|---|
| `remove()` | Throws `NoSuchElementException` |
| `poll()` | Returns `null` |

---

## 19.3 `element()` vs `peek()`

Both inspect the queue head without removing it.

```java
String a = queue.element();
String b = queue.peek();
```

Difference:

| Method | Empty queue behavior |
|---|---|
| `element()` | Throws `NoSuchElementException` |
| `peek()` | Returns `null` |

### Queue method table

| Operation | Exception form | Special-value form |
|---|---|---|
| Insert | `add(e)` | `offer(e)` |
| Remove head | `remove()` | `poll()` |
| Inspect head | `element()` | `peek()` |

This is one of the most important Java Queue interview concepts.

---

# 20. `PriorityQueue<E>`

`PriorityQueue` orders elements according to their natural ordering or a supplied `Comparator`.

```java
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(30);
pq.offer(10);
pq.offer(20);

System.out.println(pq.peek()); // 10
```

The head is the least element according to the queue's ordering.

Max-oriented queue:

```java
PriorityQueue<Integer> maxHeap =
        new PriorityQueue<>(Comparator.reverseOrder());
```

### Important point

Printing a `PriorityQueue` does **not** mean the entire output is sorted.

The queue guarantees that the head is the highest-priority element according to its ordering; repeatedly calling `poll()` retrieves elements in priority order.

```java
while (!pq.isEmpty()) {
    System.out.println(pq.poll());
}
```

---

# 21. `Deque<E>` — Double-Ended Queue

`Deque` extends `Queue` and allows insertion/removal at both ends.

```java
Deque<String> deque = new ArrayDeque<>();
```

It can act as:

- Queue (FIFO)
- Stack (LIFO)
- Double-ended queue

---

# 22. Important `Deque` Methods

## First-end operations

```java
deque.addFirst("A");
deque.offerFirst("B");
deque.removeFirst();
deque.pollFirst();
deque.getFirst();
deque.peekFirst();
```

## Last-end operations

```java
deque.addLast("A");
deque.offerLast("B");
deque.removeLast();
deque.pollLast();
deque.getLast();
deque.peekLast();
```

### Difference pattern

As with `Queue`, methods come in exception/special-value pairs:

```text
addFirst / offerFirst
removeFirst / pollFirst
getFirst / peekFirst

addLast / offerLast
removeLast / pollLast
getLast / peekLast
```

---

# 23. Using `ArrayDeque` as a Queue

```java
Queue<String> queue = new ArrayDeque<>();

queue.offer("A");
queue.offer("B");
queue.offer("C");

System.out.println(queue.poll()); // A
```

---

# 24. Using `ArrayDeque` as a Stack

Modern Java code generally prefers `Deque` instead of legacy `Stack`.

```java
Deque<Integer> stack = new ArrayDeque<>();

stack.push(10);
stack.push(20);
stack.push(30);

System.out.println(stack.pop()); // 30
```

Useful stack-style methods:

```java
push(e)
pop()
peek()
```

---

# 25. Common Comparison — List vs Set vs Queue

| Property | List | Set | Queue |
|---|---|---|---|
| Duplicates | Allowed | Not allowed | Usually allowed |
| Main purpose | Ordered/indexed data | Unique data | Processing/order |
| Index access | Yes | No | No |
| Insertion order | Typically maintained | Depends on implementation | Depends on implementation |
| Typical implementation | `ArrayList` | `HashSet` | `ArrayDeque` / `PriorityQueue` |
| Special behavior | Index operations | Uniqueness | Head/tail or priority operations |

---

# 26. Which Collection Should You Choose?

### `ArrayList`

Use when:

- You need frequent reads by index.
- Insertions/removals are mostly at the end.
- You do not need uniqueness.

### `LinkedList`

Use when:

- You specifically need linked-list/deque behavior.
- Operations happen at the ends or through a known list iterator.

For ordinary list usage, `ArrayList` is usually the better default.

### `HashSet`

Use when:

- Duplicates must be removed.
- Fast average membership checks are important.
- Ordering is not required.

### `LinkedHashSet`

Use when:

- You need uniqueness plus insertion-order iteration.

### `TreeSet`

Use when:

- You need uniqueness plus sorted order/navigation.

### `ArrayDeque`

Use when:

- You need a queue or stack.
- You want efficient operations at both ends.

### `PriorityQueue`

Use when:

- The next element should be selected by priority rather than insertion order.

---

# 27. MAP — Separate Hierarchy and Concept

`Map<K, V>` stores **key-value pairs**.

Example:

```text
101 -> "Likith"
102 -> "Rahul"
103 -> "Anita"
```

Keys are unique within a map.

A key maps to at most one value, although multiple keys can map to equal values.

---

# 28. Creating a Map

```java
Map<Integer, String> students = new HashMap<>();
```

Other implementations:

```java
Map<Integer, String> linked = new LinkedHashMap<>();
Map<Integer, String> sorted = new TreeMap<>();
```

---

# 29. `put(K key, V value)`

Adds or replaces a key-value mapping.

```java
students.put(101, "Likith");
students.put(102, "Rahul");
```

If the key already exists, the old value is replaced.

```java
students.put(101, "NewName");
```

**Parameters:**

- `key` — key used to identify the mapping.
- `value` — value associated with that key.

**Return:** previous value associated with the key, or `null` if there was no previous mapping (subject to the usual null-value ambiguity).

---

# 30. `get(Object key)`

Returns the value associated with a key.

```java
String name = students.get(101);
```

Returns `null` if no mapping is found.

---

# 31. `getOrDefault(Object key, V defaultValue)`

Returns the mapped value or a fallback value.

```java
String name = students.getOrDefault(999, "Unknown");
```

---

# 32. `containsKey(Object key)`

Checks whether a key exists.

```java
if (students.containsKey(101)) {
    System.out.println("Student exists");
}
```

---

# 33. `containsValue(Object value)`

Checks whether at least one mapping has the specified value.

```java
students.containsValue("Likith");
```

Searching values is generally less direct than key lookup.

---

# 34. `remove(Object key)`

Removes the mapping for a key.

```java
students.remove(101);
```

Returns the previously associated value, or `null` if no mapping existed.

---

# 35. `remove(Object key, Object value)`

Removes a mapping only when both key and value match.

```java
students.remove(101, "Likith");
```

Returns `true` if the mapping was removed.

---

# 36. `replace(K key, V value)`

Replaces the value only if the key is already present.

```java
students.replace(101, "UpdatedName");
```

---

# 37. `replace(K key, V oldValue, V newValue)`

Replaces the value only if the current mapping matches `oldValue`.

```java
students.replace(101, "OldName", "NewName");
```

Returns `true` if replacement occurred.

---

# 38. `putIfAbsent(K key, V value)`

Adds a mapping only when the key has no current mapping.

```java
students.putIfAbsent(101, "DefaultName");
```

Useful when you do not want to overwrite an existing value.

---

# 39. `computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)`

Computes and stores a value only when the key is absent (or currently maps to `null`, subject to the method's contract).

```java
Map<String, List<Integer>> marks = new HashMap<>();

marks.computeIfAbsent("Java", k -> new ArrayList<>())
     .add(95);
```

The lambda parameter `k` is the missing key.

---

# 40. `computeIfPresent(...)`

Recomputes a value only when a mapping is present.

```java
students.computeIfPresent(101,
        (key, value) -> value.toUpperCase());
```

The lambda receives:

- `key`
- current `value`

and returns the new value.

---

# 41. `compute(...)`

Recomputes the mapping for a key whether or not it previously existed.

```java
students.compute(101,
        (key, value) -> "Student-" + key);
```

---

# 42. `merge(...)`

Combines an existing value with a new value using a function.

```java
Map<String, Integer> frequency = new HashMap<>();

frequency.merge("Java", 1, Integer::sum);
frequency.merge("Java", 1, Integer::sum);
```

Result:

```text
Java -> 2
```

This is especially useful for frequency counting.

---

# 43. `size()`, `isEmpty()`, `clear()`

These work similarly to `Collection` operations.

```java
students.size();
students.isEmpty();
students.clear();
```

---

# 44. `keySet()`

Returns a `Set<K>` view of all keys.

```java
Set<Integer> keys = students.keySet();
```

Because it is a view, changes to the map can be reflected in the key set.

---

# 45. `values()`

Returns a `Collection<V>` view of all values.

```java
Collection<String> values = students.values();
```

Values are not guaranteed to be unique.

---

# 46. `entrySet()`

Returns a `Set<Map.Entry<K, V>>` view of all key-value mappings.

This is generally the most efficient and clean way to iterate over both keys and values together.

```java
for (Map.Entry<Integer, String> entry : students.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

---

# 47. `Map.Entry<K,V>`

Important methods:

```java
entry.getKey();
entry.getValue();
entry.setValue(newValue);
```

Example:

```java
for (Map.Entry<Integer, String> entry : students.entrySet()) {
    Integer id = entry.getKey();
    String name = entry.getValue();
}
```

---

# 48. `forEach(BiConsumer<? super K, ? super V>)`

Processes each mapping.

```java
students.forEach((id, name) ->
        System.out.println(id + " -> " + name));
```

The lambda receives two parameters:

- `id` = key
- `name` = value

---

# 49. `HashMap<K,V>`

`HashMap` is the standard hash-based map implementation.

```java
Map<String, Integer> marks = new HashMap<>();
marks.put("Java", 95);
marks.put("DBMS", 88);
```

### Characteristics

- No guaranteed iteration order.
- Average-case O(1) basic lookup/update under normal hashing assumptions.
- Allows one `null` key and multiple `null` values.
- Not synchronized.

---

# 50. `LinkedHashMap<K,V>`

Maintains predictable insertion-order iteration by default.

```java
Map<Integer, String> map = new LinkedHashMap<>();
map.put(3, "C");
map.put(1, "A");
map.put(2, "B");

System.out.println(map);
```

Useful when map order should follow insertion order.

`LinkedHashMap` can also be configured for **access-order**, which is useful for LRU-style caches.

---

# 51. `TreeMap<K,V>`

`TreeMap` implements `NavigableMap` and stores keys in sorted order.

```java
TreeMap<Integer, String> map = new TreeMap<>();
map.put(30, "C");
map.put(10, "A");
map.put(20, "B");
```

Iteration is sorted by key.

Useful navigation methods include:

```java
map.firstKey();
map.lastKey();
map.lowerKey(20);
map.floorKey(20);
map.ceilingKey(20);
map.higherKey(20);
```

---

# 52. `SortedMap` and `NavigableMap`

Hierarchy:

```text
Map<K,V>
   |
   v
SortedMap<K,V>
   |
   v
NavigableMap<K,V>
   |
   v
TreeMap<K,V>
```

Important `NavigableMap` methods:

| Method | Meaning |
|---|---|
| `lowerKey(k)` | Greatest key strictly less than `k` |
| `floorKey(k)` | Greatest key <= `k` |
| `ceilingKey(k)` | Smallest key >= `k` |
| `higherKey(k)` | Smallest key strictly greater than `k` |
| `firstKey()` | Lowest key |
| `lastKey()` | Highest key |

---

# 53. `Hashtable`

`Hashtable` is a legacy synchronized map implementation.

```java
Hashtable<Integer, String> table = new Hashtable<>();
table.put(1, "A");
```

Important restrictions:

- Does not permit `null` keys.
- Does not permit `null` values.
- Legacy API.

For new concurrent applications, `ConcurrentHashMap` is generally preferred over `Hashtable`.

---

# 54. `ConcurrentHashMap`

Used for concurrent access by multiple threads.

```java
ConcurrentHashMap<String, Integer> counts = new ConcurrentHashMap<>();
counts.put("Java", 1);
```

It provides thread-safe map operations designed for concurrent access without using one global lock for the entire map in the traditional `Hashtable` style.

---

# 55. Important Map Comparison

| Map | Ordering | Null key | Null values | Typical use |
|---|---|---:|---:|---|
| `HashMap` | None guaranteed | Yes | Yes | General-purpose map |
| `LinkedHashMap` | Insertion/access order | Yes | Yes | Predictable iteration / cache patterns |
| `TreeMap` | Sorted by key | No `null` key under natural ordering | Yes | Sorted/navigable keys |
| `Hashtable` | None guaranteed | No | No | Legacy synchronized map |
| `ConcurrentHashMap` | None guaranteed | No | No | Concurrent applications |

---

# 56. `Collection` vs `Map`

```text
Collection                         Map
    |                               |
    +-- List                       +-- key -> value
    +-- Set                        +-- unique keys
    +-- Queue                      +-- key lookup

Stores elements                   Stores mappings
```

Example:

```java
List<String> list = new ArrayList<>();
list.add("Java");
```

Here the collection stores one element:

```text
Java
```

Map:

```java
Map<Integer, String> map = new HashMap<>();
map.put(101, "Java");
```

Stores:

```text
101 -> Java
```

---

# 57. Full Practical Example

```java
import java.util.*;

public class CollectionsDemo {
    public static void main(String[] args) {

        // LIST
        List<String> list = new ArrayList<>();
        list.add("Java");
        list.add("Python");
        list.add("Java");

        System.out.println(list);          // [Java, Python, Java]
        System.out.println(list.get(0));   // Java

        // SET
        Set<String> set = new HashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("Java");

        System.out.println(set);            // Duplicate Java removed
        System.out.println(set.contains("Java"));

        // QUEUE
        Queue<String> queue = new ArrayDeque<>();
        queue.offer("A");
        queue.offer("B");
        queue.offer("C");

        System.out.println(queue.peek());  // A
        System.out.println(queue.poll());  // A

        // MAP
        Map<Integer, String> map = new HashMap<>();
        map.put(101, "Likith");
        map.put(102, "Rahul");

        System.out.println(map.get(101));

        for (Map.Entry<Integer, String> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

---

# 58. Interface-Type vs Class-Type Declaration

Prefer:

```java
List<Integer> numbers = new ArrayList<>();
Set<Integer> numbers = new HashSet<>();
Queue<Integer> numbers = new ArrayDeque<>();
Map<Integer, String> map = new HashMap<>();
```

Instead of unnecessarily exposing the concrete type:

```java
ArrayList<Integer> numbers = new ArrayList<>();
HashSet<Integer> numbers = new HashSet<>();
```

The interface form makes changing implementations easier:

```java
List<Integer> numbers = new ArrayList<>();
```

can later become:

```java
List<Integer> numbers = new LinkedList<>();
```

without changing code that only depends on `List` behavior.

---

# 59. Common Interview-Level Differences

## ArrayList vs LinkedList

```text
ArrayList
- Backed by dynamic array
- Fast random access
- Middle insertion/removal can be expensive
- Usually preferred for general List usage

LinkedList
- Doubly linked structure
- Poor random access
- Efficient operations at known ends
- Also implements Deque
```

## HashSet vs LinkedHashSet vs TreeSet

```text
HashSet
- Unique elements
- No guaranteed order
- Hash-based

LinkedHashSet
- Unique elements
- Insertion-order iteration
- Hash table + linked ordering

TreeSet
- Unique elements
- Sorted order
- Navigable operations
```

## HashMap vs LinkedHashMap vs TreeMap

```text
HashMap
- No guaranteed order
- Hash-based

LinkedHashMap
- Predictable insertion/access order

TreeMap
- Sorted keys
- NavigableMap operations
```

## Queue vs Deque

```text
Queue
- Primary queue abstraction
- Usually process from the head

Deque
- Double-ended queue
- Add/remove/inspect at both ends
- Can implement FIFO queue or LIFO stack behavior
```

---

# 60. Method Cheat Sheet

## Common `Collection` methods — used by List, Set, Queue

| Method | Purpose | Key parameter(s) | Return |
|---|---|---|---|
| `add(e)` | Add one element | `e` | `boolean` |
| `addAll(c)` | Add all elements | `c` | `boolean` |
| `remove(o)` | Remove matching element | `o` | `boolean` |
| `removeAll(c)` | Remove matching group | `c` | `boolean` |
| `retainAll(c)` | Keep only common elements | `c` | `boolean` |
| `clear()` | Remove everything | — | `void` |
| `contains(o)` | Check membership | `o` | `boolean` |
| `containsAll(c)` | Check all memberships | `c` | `boolean` |
| `size()` | Count elements | — | `int` |
| `isEmpty()` | Check empty | — | `boolean` |
| `toArray()` | Convert to array | — | `Object[]` |
| `toArray(T[] a)` | Convert to typed array | `a` | `T[]` |
| `removeIf(p)` | Remove matching predicate | `p` | `boolean` |
| `stream()` | Sequential stream | — | `Stream<E>` |
| `parallelStream()` | Parallel-capable stream | — | `Stream<E>` |

## Specialized `List` methods

| Method | Purpose |
|---|---|
| `add(index, e)` | Insert at index |
| `addAll(index, c)` | Insert collection at index |
| `get(index)` | Read element at index |
| `set(index, e)` | Replace element |
| `remove(index)` | Remove element at index |
| `indexOf(o)` | First matching index |
| `lastIndexOf(o)` | Last matching index |
| `listIterator()` | Bidirectional iterator |
| `listIterator(index)` | Iterator starting at index |
| `subList(from, to)` | View over range |

## Specialized `Queue` methods

| Method pair | Purpose |
|---|---|
| `add(e)` / `offer(e)` | Insert |
| `remove()` / `poll()` | Remove head |
| `element()` / `peek()` | Inspect head |

## Specialized `Deque` methods

| First end | Last end |
|---|---|
| `addFirst(e)` | `addLast(e)` |
| `offerFirst(e)` | `offerLast(e)` |
| `removeFirst()` | `removeLast()` |
| `pollFirst()` | `pollLast()` |
| `getFirst()` | `getLast()` |
| `peekFirst()` | `peekLast()` |

## Important `NavigableSet` / `NavigableMap` methods

```text
lower(x)
floor(x)
ceiling(x)
higher(x)
```

Interpretation:

```text
lower    < x
floor    <= x
ceiling  >= x
higher   > x
```

---

# 61. Parameter Notation You Will See in Java APIs

Java collection documentation frequently uses generic wildcards.

## `Collection<? extends E>`

Meaning: a collection containing some subtype of `E`.

Example:

```java
List<Integer> ints = List.of(1, 2, 3);
List<Number> nums = new ArrayList<>();
nums.addAll(ints);
```

This works because `Integer` extends `Number`.

## `Collection<?>`

Means a collection of an unknown type.

```java
Collection<?> collection = list;
```

You can inspect elements as `Object`, but cannot safely add arbitrary typed elements.

## `? super E`

Means a type that is `E` or a supertype of `E`.

This commonly appears where a consumer accepts values.

```java
Comparator<? super String>
```

A useful rule for understanding generic APIs is:

```text
Producer -> ? extends E
Consumer -> ? super E
```

This is commonly known as the **PECS** principle: Producer Extends, Consumer Super.

---

# 62. Iteration Techniques

## Enhanced for loop

```java
for (String value : list) {
    System.out.println(value);
}
```

## Iterator

```java
Iterator<String> it = list.iterator();

while (it.hasNext()) {
    String value = it.next();
}
```

Removing safely through an iterator:

```java
Iterator<String> it = list.iterator();
while (it.hasNext()) {
    if (it.next().equals("Java")) {
        it.remove();
    }
}
```

## ListIterator

```java
ListIterator<String> it = list.listIterator();

while (it.hasNext()) {
    System.out.println(it.next());
}

while (it.hasPrevious()) {
    System.out.println(it.previous());
}
```

## Map iteration

Preferred when both key and value are needed:

```java
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

---

# 63. Immutability and Factory Methods

Modern Java provides convenient factory methods:

```java
List<String> list = List.of("A", "B", "C");
Set<String> set = Set.of("A", "B", "C");
Map<Integer, String> map = Map.of(
    1, "A",
    2, "B"
);
```

These collections are **unmodifiable**.

This will fail:

```java
list.add("D");
```

Use a mutable copy when modification is required:

```java
List<String> mutable = new ArrayList<>(List.of("A", "B", "C"));
mutable.add("D");
```

---

# 64. `Collections` Utility Class

Do not confuse the `Collection` interface with the `Collections` utility class.

```text
Collection  -> interface
Collections -> utility class
```

Common utility methods:

```java
Collections.sort(list);
Collections.reverse(list);
Collections.shuffle(list);
Collections.max(list);
Collections.min(list);
Collections.frequency(list, "Java");
Collections.binarySearch(list, "Java");
```

Example:

```java
List<Integer> numbers = new ArrayList<>(List.of(50, 10, 30));

Collections.sort(numbers);
System.out.println(numbers); // [10, 30, 50]
```

---

# 65. `Comparator` and Custom Sorting

For custom sorting, use `Comparator`.

```java
List<String> names = new ArrayList<>(List.of("John", "Alexander", "Bob"));

names.sort(Comparator.comparingInt(String::length));
```

Reverse order:

```java
names.sort(Comparator.reverseOrder());
```

Multiple sorting conditions:

```java
names.sort(
    Comparator.comparingInt(String::length)
              .thenComparing(Comparator.naturalOrder())
);
```

---

# 66. Quick Decision Tree

```text
Need key-value pairs?
        |
       YES ----> Map
        |
       NO
        |
Need unique elements?
        |
       YES ----> Set
        |
       NO
        |
Need index-based access/order?
        |
       YES ----> List
        |
       NO
        |
Need FIFO/priority processing?
        |
       YES ----> Queue / PriorityQueue
        |
Need both ends or stack behavior?
        |
       YES ----> Deque / ArrayDeque
```

---

# 67. Final Mental Model

Remember the framework as four major ideas:

```text
COLLECTION
   |
   +---- LIST
   |      +---- ArrayList
   |      +---- LinkedList
   |      +---- Vector
   |      +---- Stack (legacy)
   |
   +---- SET
   |      +---- HashSet
   |      +---- LinkedHashSet
   |      +---- TreeSet
   |
   +---- QUEUE
          +---- PriorityQueue
          +---- LinkedList
          +---- Deque
                 +---- ArrayDeque

MAP  [separate hierarchy]
   |
   +---- HashMap
   +---- LinkedHashMap
   +---- TreeMap
   +---- Hashtable
   +---- ConcurrentHashMap
```

### The three core distinctions

**List:**

> Ordered, index-based, duplicates allowed.

**Set:**

> Unique elements, no index-based access.

**Queue:**

> Elements are processed according to a queue ordering such as FIFO or priority.

**Map:**

> Key-value mappings, unique keys, separate from `Collection`.

---

# 68. Exam and Interview Checklist

Before an interview or exam, make sure you can explain these without referring to notes:

- Why `Map` is not a child of `Collection`.
- Difference between `List`, `Set`, and `Queue`.
- `ArrayList` vs `LinkedList`.
- `HashSet` vs `LinkedHashSet` vs `TreeSet`.
- `HashMap` vs `LinkedHashMap` vs `TreeMap`.
- `Queue`: `add/offer`, `remove/poll`, `element/peek`.
- Why `ArrayDeque` is preferred over legacy `Stack` for stack usage.
- How `PriorityQueue` differs from FIFO queues.
- How `equals()` and `hashCode()` affect `HashSet` and `HashMap`.
- Difference between `Comparable` and `Comparator`.
- Meaning of `? extends E` and `? super E`.
- Difference between `Collection`, `Collections`, and `Collectors`.
- Why interface-based declarations such as `List<E> list = new ArrayList<>();` are preferred.

---

# 69. One-Page Syntax Reference

```java
// LIST
List<String> list = new ArrayList<>();
list.add("A");
list.add(0, "B");
list.get(0);
list.set(0, "C");
list.remove(0);
list.indexOf("A");

// SET
Set<String> set = new HashSet<>();
set.add("A");
set.remove("A");
set.contains("A");

// SORTED SET
NavigableSet<Integer> sortedSet = new TreeSet<>();
sortedSet.add(10);
sortedSet.floor(15);
sortedSet.ceiling(15);

// QUEUE
Queue<String> queue = new ArrayDeque<>();
queue.offer("A");
queue.peek();
queue.poll();

// PRIORITY QUEUE
PriorityQueue<Integer> pq = new PriorityQueue<>();
pq.offer(20);
pq.offer(10);
pq.peek();
pq.poll();

// DEQUE / STACK
Deque<Integer> stack = new ArrayDeque<>();
stack.push(10);
stack.push(20);
stack.peek();
stack.pop();

// MAP
Map<Integer, String> map = new HashMap<>();
map.put(101, "A");
map.get(101);
map.containsKey(101);
map.remove(101);

// MAP ITERATION
for (Map.Entry<Integer, String> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " -> " + entry.getValue());
}
```

---

## End of Notes

The most important principle is to choose the **interface based on the required behavior** and then select the implementation based on ordering, performance, memory, concurrency, and mutability requirements.
