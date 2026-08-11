# PART 1 — MASTER PLACEMENT CHEAT SHEET

This handbook follows the uploaded specification: the objective is not merely to cover a syllabus, but to move from fundamentals → intermediate → advanced → OA-ready → interview-ready, with explicit prioritization and practice progression.

### Priority legend

| Priority                  | Meaning                                                   |
| ------------------------- | --------------------------------------------------------- |
| 🔴**MUST KNOW**     | Expected for serious placement readiness                  |
| 🟠**HIGH PRIORITY** | Frequently useful in OAs/interviews                       |
| 🟡**IMPORTANT**     | Valuable after fundamentals                               |
| 🟢**GOOD TO KNOW**  | Advanced/specialized; don't sacrifice fundamentals for it |

### Difficulty

**Beginner → Easy → Medium → Hard → Advanced**

---

# 1. Aptitude

Aptitude is primarily a **speed + pattern-recognition + accuracy** game. Do not spend weeks memorizing isolated tricks before understanding the underlying mathematics.

## 1.1 Quantitative Aptitude

| Topic               | Priority | What you must know                            | Typical OA pattern            |
| ------------------- | -------: | --------------------------------------------- | ----------------------------- |
| Number System       |       🔴 | divisibility, factors, primes, remainders     | find possible numbers         |
| Divisibility        |       🔴 | 2,3,4,5,6,8,9,11 rules                        | identify divisible number     |
| HCF & LCM           |       🔴 | prime factorization, Euclid                   | work/rational-number problems |
| Factors & Multiples |       🔴 | number of factors, divisors                   | count factors                 |
| Remainders          |       🔴 | modular arithmetic                            | huge powers                   |
| Unit Digit          |       🟠 | cyclicity                                     | last digit                    |
| Simplification      |       🔴 | BODMAS, fractions                             | rapid calculation             |
| Approximation       |       🟠 | estimation                                    | numerical comparison          |
| Percentages         |       🔴 | percentage change                             | salary/population/price       |
| Profit & Loss       |       🔴 | CP, SP, MP, discount                          | successive discounts          |
| SI                  |       🔴 | $\text{SI} = \frac{P \cdot R \cdot T}{100}$ | investment                    |
| CI                  |       🔴 | compound growth                               | yearly/half-yearly            |
| Ratio & Proportion  |       🔴 | equivalent ratios                             | distribution                  |
| Partnership         |       🟠 | capital × time                               | profit sharing                |
| Averages            |       🔴 | weighted average                              | replacement                   |
| Mixtures            |       🟠 | alligation                                    | concentration                 |
| Time & Work         |       🔴 | work rates                                    | combined workers              |
| Pipes & Cisterns    |       🟠 | positive/negative rates                       | fill/drain                    |
| Time-Speed-Distance |       🔴 | $D = S \cdot T$                             | journeys                      |
| Trains              |       🟠 | relative speed                                | crossing                      |
| Boats & Streams     |       🟠 | upstream/downstream                           | effective speed               |
| Relative Speed      |       🔴 | same/opposite direction                       | trains/races                  |
| P&C                 |       🟠 | ${}^n P_r$, ${}^n C_r$                    | arrangements                  |
| Probability         |       🟠 | favorable/total                               | cards/dice                    |
| Algebra             |       🔴 | equations, identities                         | unknown quantities            |
| Linear Equations    |       🔴 | simultaneous equations                        | ages/work                     |
| Quadratic           |       🟠 | roots/discriminant                            | equation properties           |
| Progressions        |       🟡 | AP/GP                                         | nth term/sum                  |
| Geometry            |       🟠 | triangles, circles, angles                    | area/angles                   |
| Mensuration         |       🟠 | 2D/3D formulas                                | volume/surface                |
| DI                  |       🔴 | tables, graphs, percentages                   | data interpretation           |
| Clocks              |       🟡 | angle formula                                 | clock angle                   |
| Calendars           |       🟡 | odd days                                      | day calculation               |
| Ages                |       🔴 | ratios/differences                            | age relationships             |

### Core formulas

#### Percentages

- $x\% = \frac{x}{100}$
- Percentage change:

$$
\text{Percentage Change} = \frac{\text{New} - \text{Old}}{\text{Old}} \times 100
$$

Successive changes $a\%$ and $b\%$:

$$
\text{Net Change} = a + b + \frac{a \cdot b}{100}
$$

For a decrease, use a negative percentage.

---

#### Profit & Loss

$$
\text{Profit} = \text{SP} - \text{CP}
$$

$$
\text{Loss} = \text{CP} - \text{SP}
$$

$$
\text{Profit}\% = \frac{\text{Profit}}{\text{CP}} \times 100
$$

$$
\text{Loss}\% = \frac{\text{Loss}}{\text{CP}} \times 100
$$

$$
\text{SP} = \text{CP} \left(1 + \frac{P}{100}\right)
$$

---

#### Simple Interest

$$
\text{SI} = \frac{P \cdot R \cdot T}{100}
$$

$$
A = P + \text{SI}
$$

---

#### Compound Interest

$$
A = P \left(1 + \frac{r}{100}\right)^n
$$

$$
\text{CI} = A - P
$$

For depreciation:

$$
A = P \left(1 - \frac{r}{100}\right)^n
$$

---

#### Ratio

If

$$
a : b = m : n
$$

then

$$
a = k \cdot m, \quad b = k \cdot n
$$

---

#### Average

$$
\text{Average} = \frac{\text{Sum}}{\text{Count}}
$$

When one value is replaced:

$$
\text{New Sum} = \text{Old Sum} - \text{Old Value} + \text{New Value}
$$

---

#### Time & Work

If A completes work in $x$ days:

$$
\text{Rate}_A = \frac{1}{x}
$$

If A and B work together:

$$
\text{Rate}_{\text{combined}} = \frac{1}{x} + \frac{1}{y}
$$

---

#### Speed

$$
\text{Speed} = \frac{\text{Distance}}{\text{Time}}
$$

$$
\text{Distance} = \text{Speed} \times \text{Time}
$$

$$
\text{Time} = \frac{\text{Distance}}{\text{Speed}}
$$

Conversions:

- $\text{km/h} \to \text{m/s}$: multiply by $\frac{5}{18}$
- $\text{m/s} \to \text{km/h}$: multiply by $\frac{18}{5}$

---

#### Relative speed

Same direction:

$$
v_{\text{rel}} = |v_1 - v_2|
$$

Opposite direction:

$$
v_{\text{rel}} = v_1 + v_2
$$

---

#### Permutation

$$
{}^n P_r = \frac{n!}{(n - r)!}
$$

#### Combination

$$
{}^n C_r = \frac{n!}{r!(n - r)!}
$$

---

#### Probability

$$
P(E) = \frac{\text{Favorable Outcomes}}{\text{Total Outcomes}}
$$

Complement:

$$
P(\bar{E}) = 1 - P(E)
$$

---

## Aptitude traps

1. Confusing percentage increase with percentage-point increase.
2. Using selling price instead of cost price as the denominator for profit percentage.
3. Forgetting relative speed.
4. Adding percentages directly when changes are successive.
5. Treating probability events as independent without justification.
6. Using average of averages without weighting.
7. Ignoring units.
8. Spending 5 minutes on one aptitude question in an OA.

---

# 1.2 Logical Reasoning

| Topic                  | Priority | Core skill                                |
| ---------------------- | -------: | ----------------------------------------- |
| Number Series          |       🔴 | detect arithmetic/multiplicative patterns |
| Letter Series          |       🟠 | alphabet positions                        |
| Coding-Decoding        |       🔴 | transformation rules                      |
| Analogy                |       🟡 | relationship recognition                  |
| Classification         |       🟡 | identify odd element                      |
| Blood Relations        |       🔴 | relationship graph                        |
| Direction Sense        |       🔴 | coordinate/direction reasoning            |
| Ranking                |       🔴 | position equations                        |
| Seating Arrangement    |       🔴 | constraint management                     |
| Puzzles                |       🔴 | systematic elimination                    |
| Syllogisms             |       🟠 | logical deduction                         |
| Statements/Conclusions |       🟠 | inference                                 |
| Assumptions            |       🟠 | implicit premise                          |
| Data Sufficiency       |       🔴 | information adequacy                      |
| Venn Diagrams          |       🟠 | set relationships                         |
| Input-Output           |       🟡 | transformation                            |
| Logical Deductions     |       🔴 | constraints                               |

### Puzzle method

1. Extract entities.
2. Extract constraints.
3. Create a table/grid.
4. Place fixed information first.
5. Use negative constraints.
6. Branch only when necessary.
7. Eliminate contradictions.
8. Verify all conditions.

---

# 1.3 Verbal Ability

### 🔴 Must know

- Reading comprehension
- Grammar
- Sentence correction
- Error detection
- Vocabulary
- Para jumbles
- Sentence completion
- Fill in the blanks

### Grammar checklist

- Subject–verb agreement
- Tenses
- Articles
- Prepositions
- Pronouns
- Modifiers
- Parallelism
- Conjunctions
- Active/passive voice
- Direct/indirect speech

### RC strategy

**Question → identify evidence → return to passage → eliminate unsupported options.**

Do not answer based on what is merely plausible.

---

# 2. Java + OOP

Java should be prepared at three levels:

**Language syntax → Java internals → interview reasoning.**

---

## 2.1 Java fundamentals

### JVM / JRE / JDK

| Component | Meaning                         |
| --------- | ------------------------------- |
| JVM       | executes Java bytecode          |
| JRE       | JVM + runtime libraries         |
| JDK       | JRE/runtime + development tools |

Flow:

```text
.java
  ↓ javac
.class bytecode
  ↓
JVM
  ↓
Machine execution
```

### Primitive types

| Type    |                    Typical size |
| ------- | ------------------------------: |
| byte    |                           8 bit |
| short   |                          16 bit |
| int     |                          32 bit |
| long    |                          64 bit |
| float   |                          32 bit |
| double  |                          64 bit |
| char    |                          16 bit |
| boolean | language-defined representation |

### 🔴 Understand deeply

- Primitive vs reference types
- Stack vs heap concept
- String pool
- `==` vs `equals()`
- pass-by-value
- `static`
- `final`
- access modifiers
- wrapper classes
- autoboxing
- exceptions
- garbage collection

---

# 2.2 Strings

### String

Immutable.

```java
String s = "hello";
```

Changing the string creates another object rather than mutating the original object.

### StringBuilder

- Mutable
- Not synchronized
- Usually preferred for local string construction

### StringBuffer

- Mutable
- Synchronized
- Usually slower than `StringBuilder`

### Common trap

> Java is pass-by-reference.

**Incorrect.**

Java is **always pass-by-value**.

For objects, the value being passed is a copy of the reference.

---

# 2.3 OOP

| Concept       | Meaning                                           | Priority |
| ------------- | ------------------------------------------------- | -------: |
| Class         | blueprint/type definition                         |       🔴 |
| Object        | runtime instance                                  |       🔴 |
| Encapsulation | bundle data + behavior, control access            |       🔴 |
| Abstraction   | expose essential behavior, hide implementation    |       🔴 |
| Inheritance   | derive one class from another                     |       🔴 |
| Polymorphism  | one interface/reference, multiple implementations |       🔴 |
| Association   | general relationship                              |       🟠 |
| Aggregation   | weak whole-part relationship                      |       🟠 |
| Composition   | strong ownership relationship                     |       🟠 |

### Encapsulation

Use access control to protect object state.

```java
class BankAccount {
    private double balance;

    public void deposit(double amount) {
        if (amount > 0) balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

---

## Abstraction

Focus on **what** rather than **how**.

Implemented using:

- abstract classes
- interfaces

---

## Inheritance

```java
class Animal {
    void sound() {}
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```

---

## Polymorphism

### Compile-time

Method overloading.

```java
add(int a, int b)
add(double a, double b)
```

### Runtime

Method overriding + dynamic dispatch.

```java
Animal a = new Dog();
a.sound();
```

The actual object determines the overridden method executed.

---

# 2.4 Overloading vs overriding

|                   | Overloading          | Overriding             |
| ----------------- | -------------------- | ---------------------- |
| Time              | Compile-time         | Runtime                |
| Class             | Usually same class   | Parent-child           |
| Signature         | Different parameters | Same signature         |
| Return type alone | Cannot distinguish   | Compatible return type |
| Polymorphism      | Compile-time         | Runtime                |

---

# 2.5 `this` vs `super`

### `this`

Current object.

### `super`

Parent-class portion/object reference.

Common uses:

```java
this.x
this()
super.x
super()
super.method()
```

---

# 2.6 Abstract class vs interface

| Feature              | Abstract class                   | Interface                      |
| -------------------- | -------------------------------- | ------------------------------ |
| State                | Can have instance state          | Usually constants/static state |
| Constructor          | Yes                              | No                             |
| Abstract methods     | Yes                              | Yes                            |
| Concrete methods     | Yes                              | Yes                            |
| Multiple inheritance | No class inheritance             | Multiple interfaces            |
| Use                  | shared base implementation/state | contract/capability            |

### Modern interface features

Java 8 introduced:

- default methods
- static methods
- functional interfaces
- lambda expressions

Later Java versions added additional interface-related capabilities and language improvements.

**Interview trap:** interfaces are no longer simply "100% abstract classes."

---

# 2.7 Exception handling

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    └── checked exceptions
```

### Checked

Compiler requires handling/declaring.

### Unchecked

Runtime exceptions, subclasses of `RuntimeException`.

### `throw` vs `throws`

| `throw`                 | `throws`                   |
| ------------------------- | ---------------------------- |
| actually throws exception | declares possible exceptions |
| used inside method        | method signature             |

---

# 2.8 Collections Framework

### Core hierarchy

```text
Collection
├── List
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
├── Set
│   ├── HashSet
│   ├── LinkedHashSet
│   └── TreeSet
└── Queue
    ├── PriorityQueue
    └── Deque
        └── ArrayDeque

Map
├── HashMap
├── LinkedHashMap
├── TreeMap
└── Hashtable
```

### Critical comparison

| Structure     | Ordering               | Duplicates  | Typical lookup |
| ------------- | ---------------------- | ----------- | -------------- |
| ArrayList     | insertion/index        | yes         | O(1) index     |
| LinkedList    | insertion              | yes         | O(n)           |
| HashSet       | none guaranteed        | no          | O(1) average   |
| LinkedHashSet | insertion              | no          | O(1) average   |
| TreeSet       | sorted                 | no          | O(log n)       |
| HashMap       | none guaranteed        | keys unique | O(1) average   |
| LinkedHashMap | insertion/access order | keys unique | O(1) average   |
| TreeMap       | sorted keys            | keys unique | O(log n)       |
| PriorityQueue | heap priority          | yes         | O(1) peek      |
| ArrayDeque    | deque order            | yes         | O(1) ends      |

### HashMap interview essentials

Understand:

1. hash function
2. hash code
3. bucket
4. collision
5. equality check
6. resizing
7. load factor
8. collision handling
9. why mutable keys are dangerous

### `equals()` + `hashCode()`

Contract:

> If two objects are equal according to `equals()`, they must have the same hash code.

The reverse is **not required**.

---

# 2.9 Comparable vs Comparator

### Comparable

Defines natural ordering inside the class.

```java
class Student implements Comparable<Student>
```

### Comparator

External/custom ordering.

```java
Comparator<Student>
```

Use Comparator when you need multiple sorting strategies.

---

# 2.10 Generics

```java
List<String> names;
```

Benefits:

- type safety
- fewer casts
- reusable APIs

Know:

- generic classes
- generic methods
- bounded types
- wildcards
- `? extends`
- `? super`

---

# 2.11 Lambda + Streams

Example:

```java
List<Integer> result =
    numbers.stream()
           .filter(x -> x % 2 == 0)
           .map(x -> x * x)
           .toList();
```

Know:

- `filter`
- `map`
- `sorted`
- `distinct`
- `limit`
- `reduce`
- `collect`
- `forEach`
- intermediate vs terminal operations

### Stream trap

Streams are not simply "faster loops."

They provide a declarative processing model and have different evaluation characteristics.

---

# 2.12 Modern Java interview topics

🟡/🟢 depending on target:

- records
- sealed classes
- pattern matching
- enhanced switch
- text blocks
- Optional
- functional interfaces
- method references
- modern collection APIs

Do **not** spend more time on these than on OOP, Collections, exceptions, and core language semantics.

---

# Java rapid revision

Before an interview, be able to answer immediately:

1. JVM vs JRE vs JDK
2. Why Java is platform-independent
3. Why String is immutable
4. `==` vs `equals`
5. `equals`/`hashCode` contract
6. pass-by-value
7. overloading vs overriding
8. abstract class vs interface
9. static vs instance
10. final vs finally vs finalize
11. checked vs unchecked exceptions
12. HashMap internals
13. HashMap vs Hashtable
14. ArrayList vs LinkedList
15. HashSet vs TreeSet
16. HashMap vs TreeMap
17. Comparable vs Comparator
18. StringBuilder vs StringBuffer
19. stack vs heap
20. garbage collection

---

# 3. DSA

The DSA target should not be "I know algorithms."

The target is:

> **Given an unfamiliar problem, identify the underlying pattern and construct an efficient solution.**

---

# 3.1 Level 1 — Programming Fundamentals

### 🔴 Must know

- variables
- conditions
- loops
- functions
- arrays
- strings
- recursion
- basic mathematics
- bit manipulation

Before moving ahead, you should comfortably implement:

- frequency counting
- max/min
- reversal
- searching
- sorting basics
- prefix calculations
- recursion
- basic matrix traversal

---

# 3.2 Complexity

| Complexity      | Typical interpretation         |
| --------------- | ------------------------------ |
| $O(1)$        | constant                       |
| $O(\log n)$   | binary search                  |
| $O(n)$        | single scan                    |
| $O(n \log n)$ | efficient comparison sorting   |
| $O(n^2)$      | pairwise comparisons           |
| $O(2^n)$      | many subset recursion problems |
| $O(n!)$       | permutations/brute force       |

### Know

- Big-O
- Big-Theta
- Big-Omega
- best/average/worst case
- auxiliary space
- recursion stack
- amortized analysis

### Critical skill

Given code:

```java
for (int i = 0; i < n; i++) {
    for (int j = i; j < n; j++) {
        ...
    }
}
```

You should be able to reason that the number of iterations is approximately:

$$
\sum_{i=1}^{n} i = \frac{n(n + 1)}{2} = O(n^2)
$$

---

# 3.3 Basic data structures

### Arrays

Master:

- traversal
- insertion/deletion reasoning
- prefix sums
- frequency arrays
- two pointers
- binary search
- sorting

### Linked list

Master:

- traversal
- insertion
- deletion
- reversal
- middle node
- cycle detection
- merge lists
- intersection

### Stack

Common problems:

- valid parentheses
- next greater element
- monotonic stack
- expression evaluation
- histogram

### Queue

Common problems:

- BFS
- level-order traversal
- sliding-window maximum
- scheduling

### Hashing

Extremely high placement value.

Master:

- frequency maps
- duplicate detection
- complement lookup
- prefix-sum + hashmap
- grouping

---

# 3.4 Pattern recognition

| Pattern           | Recognition signal                 | Priority |
| ----------------- | ---------------------------------- | -------: |
| Two pointers      | sorted array/pair relationship     |       🔴 |
| Sliding window    | contiguous subarray/substring      |       🔴 |
| Prefix sum        | repeated range sum                 |       🔴 |
| Hashing           | lookup/frequency                   |       🔴 |
| Binary search     | monotonic search space             |       🔴 |
| Sorting           | ordering simplifies problem        |       🔴 |
| Fast/slow pointer | linked list/cycle                  |       🟠 |
| Intervals         | overlapping ranges                 |       🟠 |
| Heap              | top-K / repeated min/max           |       🟠 |
| Greedy            | local optimal choice               |       🟠 |
| Backtracking      | generate constrained possibilities |       🟠 |
| Monotonic stack   | next greater/smaller               |       🟠 |
| Difference array  | range updates                      |       🟡 |
| Monotonic queue   | window extrema                     |       🟡 |
| Matrix traversal  | grid movement                      |       🔴 |
| Bit manipulation  | binary-state operations            |       🟡 |

---

## Two pointers

Typical signals:

- sorted array
- pair sum
- remove duplicates
- opposite ends

```text
left →          ← right
```

Complexity often:

**$O(n)$** instead of $O(n^2)$.

---

## Sliding window

Signals:

> contiguous subarray / substring + condition

Template:

```text
left = 0

for right in range(n):
    add right

    while window invalid:
        remove left
        left++

    update answer
```

---

## Prefix sum

$$
\text{prefix}[i] = a_0 + a_1 + \dots + a_i
$$

Range sum:

$$
\text{sum}(l, r) = \text{prefix}[r] - \text{prefix}[l - 1]
$$

---

## Binary search

Do not memorize only the standard array implementation.

Recognize:

> **Can the answer space be ordered such that one side can be eliminated?**

Applications:

- sorted arrays
- first/last occurrence
- minimum feasible value
- maximum feasible value
- capacity problems
- allocation problems

---

# 3.5 Sorting

| Algorithm |         Average |           Worst |              Space |
| --------- | --------------: | --------------: | -----------------: |
| Bubble    |      $O(n^2)$ |      $O(n^2)$ |           $O(1)$ |
| Selection |      $O(n^2)$ |      $O(n^2)$ |           $O(1)$ |
| Insertion |      $O(n^2)$ |      $O(n^2)$ |           $O(1)$ |
| Merge     | $O(n \log n)$ | $O(n \log n)$ |           $O(n)$ |
| Quick     | $O(n \log n)$ |      $O(n^2)$ |          recursion |
| Heap      | $O(n \log n)$ | $O(n \log n)$ | $O(1)$ auxiliary |

For placements, prioritize understanding **why/when** to use sorting rather than memorizing every implementation detail.

---

# 3.6 Trees

### 🔴 Must know

- binary tree
- BST
- preorder
- inorder
- postorder
- level order
- DFS
- BFS
- height
- depth
- diameter
- balanced trees
- LCA
- path problems

### Key property

For a BST:

```text
left < root < right
```

Therefore inorder traversal produces sorted order.

### Common interview problems

- maximum depth
- invert tree
- diameter
- level-order traversal
- validate BST
- lowest common ancestor
- kth smallest BST
- path sum
- serialize/deserialize

---

# 3.7 Graphs

### Representation

Adjacency list is generally preferred for sparse graphs.

### 🔴 Must know

- BFS
- DFS
- connected components
- cycle detection
- topological sort
- shortest path
- MST
- DSU

### Algorithm selection

| Problem                         | Algorithm        |
| ------------------------------- | ---------------- |
| unweighted shortest path        | BFS              |
| positive weighted shortest path | Dijkstra         |
| negative edges                  | Bellman-Ford     |
| all-pairs shortest path         | Floyd-Warshall   |
| MST                             | Prim/Kruskal     |
| dependency ordering             | Topological sort |
| connectivity merging            | DSU              |
| SCC                             | Kosaraju/Tarjan  |

### Dijkstra trap

Standard Dijkstra does **not** handle negative-weight edges correctly.

---

# 3.8 Advanced DSA

### Dynamic Programming

The most important mental model:

```text
State
↓
Transition
↓
Base case
↓
Memoization / Tabulation
↓
Space optimization
```

### Identify DP when you see:

- overlapping subproblems
- optimal substructure
- decisions at each state
- "maximum/minimum number of ways"
- repeated recursive states

### Example

For climbing stairs:

$$
\text{dp}[i] = \text{dp}[i - 1] + \text{dp}[i - 2]
$$

Do not memorize this problem. Learn how the state was derived.

---

## DP families

| Family      | Examples                        |
| ----------- | ------------------------------- |
| 1D          | climbing stairs, house robber   |
| 2D          | grid paths                      |
| Knapsack    | 0/1, unbounded                  |
| subsequence | LIS, LCS                        |
| partition   | partition DP                    |
| interval    | matrix-chain style problems     |
| bitmask     | subset/state problems           |
| tree DP     | DP over tree                    |
| digit DP    | constrained number construction |

---

# 3.9 Advanced structures

🟡/🟢 depending on target:

- Trie
- Segment Tree
- Fenwick Tree
- advanced string algorithms
- bridges
- articulation points
- SCC
- advanced graph algorithms

For ordinary service-company placements, these are often unnecessary.

For top product-company interviews, they become useful after the core DSA foundation is strong.

---

# DSA pattern recognition cheat sheet

```text
Sorted + pair → Two pointers

Contiguous + longest/shortest → Sliding window

Range sum → Prefix sum

Range update → Difference array

Frequency/existence → HashMap/HashSet

Sorted/searchable answer → Binary search

Top K → Heap

Next greater/smaller → Monotonic stack

Shortest unweighted path → BFS

Shortest positive weighted path → Dijkstra

Dependency ordering → Topological sort

Components/connectivity → DFS/BFS/DSU

Repeated choices + overlapping states → DP

Generate all valid possibilities → Backtracking

Overlapping intervals → Sort + interval processing

Prefix-based strings → Trie
```

---

# 4. Operating Systems

# 4.1 Fundamentals

### 🔴 Must know

- OS purpose
- kernel
- system calls
- user mode
- kernel mode
- processes
- threads
- memory
- scheduling
- synchronization
- deadlocks
- virtual memory
- file systems
- IPC

---

# Process vs Thread

| Process                                 | Thread                        |
| --------------------------------------- | ----------------------------- |
| independent execution unit              | execution unit inside process |
| separate address space                  | shares process memory         |
| heavier                                 | lighter                       |
| context switch generally more expensive | generally cheaper             |
| IPC required for communication          | shared memory available       |

### Interview trap

Threads within the same process **share process resources**, but each thread has its own execution context such as stack and registers.

---

# Process states

```text
New
 ↓
Ready ↔ Running
          ↓
       Waiting
          ↓
        Ready
          ↓
     Terminated
```

---

# Context switch

CPU changes from one execution context to another.

Costs include:

- saving registers
- loading another context
- scheduler overhead
- cache/TLB effects

---

# Scheduling

| Algorithm        | Key idea                   |
| ---------------- | -------------------------- |
| FCFS             | first arrival              |
| SJF              | shortest burst             |
| SRTF             | shortest remaining time    |
| Priority         | highest priority           |
| Round Robin      | time quantum               |
| Multilevel Queue | fixed queues               |
| MLFQ             | dynamically adjusts queues |

### Important metrics

- turnaround time
- waiting time
- response time
- throughput
- CPU utilization

For non-overlapping arrival examples:

$$
\text{TAT} = \text{Completion Time} - \text{Arrival Time}
$$

$$
\text{WT} = \text{TAT} - \text{Burst Time}
$$

---

# Synchronization

### Critical section

Code accessing shared resources.

### Race condition

Result depends on timing/order of concurrent execution.

### Mutex

Mutual exclusion lock.

### Semaphore

Synchronization primitive using a counter.

### Monitor

Higher-level synchronization abstraction.

---

# Deadlock

Four Coffman conditions:

1. Mutual exclusion
2. Hold and wait
3. No preemption
4. Circular wait

Remove at least one → deadlock can be prevented.

### Prevention vs avoidance

**Prevention:** structurally break a necessary condition.

**Avoidance:** dynamically ensure system remains in a safe state.

Banker's Algorithm → avoidance.

---

# Memory management

### Paging

Logical memory divided into pages.

Physical memory divided into frames.

### Segmentation

Memory divided according to logical segments.

### Virtual memory

Allows processes to use an address space larger than physical RAM through mechanisms such as paging.

---

# Page replacement

| Algorithm | Key concept                             |
| --------- | --------------------------------------- |
| FIFO      | oldest page                             |
| LRU       | least recently used                     |
| Optimal   | future knowledge; theoretical benchmark |

### Thrashing

System spends excessive time paging rather than executing useful work.

---

# Fragmentation

### Internal

Unused space **inside** allocated block.

### External

Free memory exists but is fragmented into separate regions.

---

# High-frequency OS interview questions

1. Process vs thread?
2. What happens during context switching?
3. What is a system call?
4. User mode vs kernel mode?
5. Race condition?
6. Mutex vs semaphore?
7. What is deadlock?
8. Four deadlock conditions?
9. Deadlock prevention vs avoidance?
10. What is virtual memory?
11. Paging vs segmentation?
12. What is page fault?
13. FIFO vs LRU?
14. What is thrashing?
15. Why is Round Robin useful?
16. Starvation vs deadlock?
17. What is IPC?
18. Shared memory vs message passing?

---

# 5. Computer Networks

# 5.1 Models

### OSI

```text
7 Application
6 Presentation
5 Session
4 Transport
3 Network
2 Data Link
1 Physical
```

### TCP/IP

```text
Application
Transport
Internet
Link
```

### High-value mapping

| Function   | Typical protocol |
| ---------- | ---------------- |
| DNS        | Application      |
| HTTP/HTTPS | Application      |
| TCP/UDP    | Transport        |
| IP         | Network          |
| Ethernet   | Data Link        |

---

# MAC vs IP

| MAC                               | IP                          |
| --------------------------------- | --------------------------- |
| link-layer identifier             | network-layer address       |
| used within local networking      | used for routing            |
| associated with network interface | assigned/configured address |

---

# TCP vs UDP

| TCP                 | UDP                             |
| ------------------- | ------------------------------- |
| connection-oriented | connectionless                  |
| reliable            | no built-in delivery guarantee  |
| ordered             | no ordering guarantee           |
| flow control        | no TCP-style flow control       |
| congestion control  | no TCP-style congestion control |
| higher overhead     | lower overhead                  |

---

# TCP three-way handshake

```text
Client → SYN
Server → SYN + ACK
Client → ACK
```

Purpose: establish connection state and synchronize sequence information.

---

# TCP termination

Typically involves FIN/ACK exchanges and can involve four segments because each direction closes independently.

---

# Flow vs congestion control

**Flow control:** prevents sender from overwhelming receiver.

**Congestion control:** prevents sender/network from overwhelming the network.

This distinction is frequently tested.

---

# HTTP

Methods:

- GET
- POST
- PUT
- PATCH
- DELETE
- HEAD
- OPTIONS

### Status codes

| Range | Meaning           |
| ----- | ----------------- |
| 1xx   | informational     |
| 2xx   | success           |
| 3xx   | redirection       |
| 4xx   | client-side issue |
| 5xx   | server-side issue |

Know:

- 200
- 201
- 204
- 301
- 302
- 304
- 400
- 401
- 403
- 404
- 409
- 429
- 500
- 502
- 503

---

# HTTPS

HTTPS = HTTP over TLS.

TLS provides cryptographic protections including:

- confidentiality
- integrity
- authentication through certificates

---

# DNS

Basic flow:

```text
Browser/cache
 ↓
Resolver
 ↓
Root
 ↓
TLD
 ↓
Authoritative DNS
 ↓
IP address
```

---

# DHCP

Common conceptual sequence:

```text
Discover
Offer
Request
Acknowledge
```

---

# NAT

Allows address translation between networks, commonly private internal addresses and public addresses.

---

# REST

Core concepts:

- resources
- HTTP methods
- stateless requests
- representations
- standard HTTP semantics

REST is an architectural style, not simply "using JSON APIs."

---

# WebSockets

Persistent, bidirectional communication channel.

Useful for:

- chat
- live notifications
- collaborative applications
- real-time dashboards

---

# The classic interview question

## "What happens when you type google.com into a browser?"

Strong answer should cover:

```text
URL parsing
↓
Browser/cache checks
↓
DNS resolution
↓
Routing/network setup
↓
TCP connection
↓
TLS handshake for HTTPS
↓
HTTP request
↓
Server processing
↓
HTTP response
↓
Browser parses HTML
↓
CSS/JS/resources fetched
↓
DOM/CSSOM
↓
Rendering
```

A top-tier candidate should be able to explain this at multiple levels rather than reciting the sequence.

---

# 6. DBMS + SQL

# 6.1 Relational fundamentals

Know:

- relation/table
- tuple/row
- attribute/column
- schema
- keys
- constraints
- relationships
- cardinality
- normalization
- transactions
- indexes

---

# Keys

| Key           | Meaning                               |
| ------------- | ------------------------------------- |
| Super key     | uniquely identifies tuple             |
| Candidate key | minimal super key                     |
| Primary key   | chosen candidate key                  |
| Foreign key   | references another table              |
| Composite key | multiple attributes                   |
| Alternate key | candidate key not selected as primary |

---

# Normalization

### 1NF

Atomic values; no repeating groups.

### 2NF

1NF + no partial dependency on part of a composite candidate key.

### 3NF

2NF + no inappropriate transitive dependency of non-key attributes on candidate keys.

### BCNF

Every determinant is a candidate key.

### Why normalize?

Reduce:

- redundancy
- update anomalies
- insertion anomalies
- deletion anomalies

---

# SQL command groups

| Category | Examples               |
| -------- | ---------------------- |
| DDL      | CREATE, ALTER, DROP    |
| DML      | INSERT, UPDATE, DELETE |
| DQL      | SELECT                 |
| DCL      | GRANT, REVOKE          |
| TCL      | COMMIT, ROLLBACK       |

---

# SQL execution concepts

Know the conceptual processing order:

```text
FROM
JOIN
WHERE
GROUP BY
HAVING
SELECT
DISTINCT
ORDER BY
LIMIT/OFFSET
```

This helps explain why aliases generally cannot be used in `WHERE` the same way they can in later stages.

---

# JOINs

| Join  | Purpose                  |
| ----- | ------------------------ |
| INNER | matching rows            |
| LEFT  | all left + matches       |
| RIGHT | all right + matches      |
| FULL  | all rows from both sides |
| SELF  | table joined to itself   |
| CROSS | Cartesian product        |

---

# High-value SQL patterns

### Second highest salary

Know multiple approaches:

- `MAX` subquery
- `ORDER BY ... LIMIT`
- `DENSE_RANK()`

### Duplicate records

```sql
GROUP BY ...
HAVING COUNT(*) > 1
```

### Top N per group

Window functions:

```sql
ROW_NUMBER()
RANK()
DENSE_RANK()
```

### Running total

```sql
SUM(amount) OVER (...)
```

### Previous/next row

```sql
LAG()
LEAD()
```

---

# Transactions

### ACID

| Property    | Meaning                            |
| ----------- | ---------------------------------- |
| Atomicity   | all or nothing                     |
| Consistency | valid state → valid state         |
| Isolation   | concurrent transactions controlled |
| Durability  | committed data persists            |

---

# Isolation levels

Understand anomalies:

- dirty read
- non-repeatable read
- phantom read

And how isolation levels trade concurrency for consistency guarantees.

---

# Indexes

Indexes improve lookup performance but introduce:

- storage overhead
- write/update cost
- maintenance cost

### Clustered vs non-clustered

Know the conceptual distinction:

- clustered organization determines physical/logical storage ordering around indexed data
- non-clustered index is a separate index structure referencing underlying rows

Exact implementation differs by database system.

---

# SQL vs NoSQL

| SQL                     | NoSQL                                  |
| ----------------------- | -------------------------------------- |
| relational              | non-relational families                |
| structured schema       | often flexible schema                  |
| joins strong            | joins often less central               |
| strong relational model | scalability/data-model flexibility     |
| SQL query language      | database-specific APIs/query languages |

Do not claim "NoSQL is always faster" or "NoSQL doesn't have transactions."

---

# MongoDB

Know:

- database
- collection
- document
- BSON
- ObjectId
- embedding
- referencing
- indexes
- aggregation
- CRUD
- schema design

For interviews, be able to explain **why you selected MongoDB**, not merely that you used it.

---

# 7. Group Discussion

A GD is not a competition to speak the most.

Evaluation commonly centers around:

- clarity
- relevance
- reasoning
- communication
- listening
- teamwork
- leadership
- confidence
- content
- ability to develop an argument

---

# GD structure

## Opening

Use:

```text
Definition
→ Context
→ Position
→ First argument
```

Avoid:

> "Good morning everyone, today we are here to discuss..."

unless it adds something.

---

## Entering an ongoing discussion

Use:

> "Building on that point, there is another factor worth considering..."

Then add new reasoning.

---

## Disagreeing

Bad:

> "You're completely wrong."

Better:

> "I see the point, but the argument changes when we consider..."

---

## Strong argument structure

```text
Claim
↓
Reason
↓
Evidence/example
↓
Implication
```

---

## 25 practice topics

1. AI replacing jobs
2. AI in education
3. Generative AI and software developers
4. Remote work
5. Four-day work week
6. Social media and mental health
7. Data privacy vs personalization
8. Cryptocurrency
9. Digital payments
10. Automation
11. Cybersecurity
12. India's semiconductor ambitions
13. Electric vehicles
14. Climate technology
15. Online education
16. Startup culture
17. Work-life balance
18. Surveillance vs security
19. Should coding be mandatory?
20. Ethical AI
21. Technology and inequality
22. Space commercialization
23. Social media regulation
24. Cashless economy
25. Should companies prioritize skills over degrees?

For each topic, practice:

**2 arguments supporting + 2 opposing + 1 balanced conclusion.**

---

# 8. Technical Interview

Technical interviews test whether your knowledge is **usable**, not whether you can reproduce notes.

---

# 8.1 Programming

Prepare:

- syntax
- debugging
- complexity
- memory
- edge cases
- input handling
- clean code

Typical questions:

1. Reverse a string.
2. Find duplicates.
3. Find frequency.
4. Reverse linked list.
5. Detect cycle.
6. Binary search.
7. Tree traversal.
8. BFS/DFS.
9. Top-K elements.
10. Basic DP.

---

# 8.2 Java interview

Expect:

- JVM
- String pool
- immutability
- OOP
- inheritance
- interface
- exceptions
- collections
- HashMap
- generics
- streams
- multithreading basics
- Java memory concepts

---

# 8.3 DSA interview

Do not immediately code.

Use:

```text
Clarify
↓
Examples
↓
Brute force
↓
Complexity
↓
Optimization
↓
Code
↓
Test
↓
Complexity
```

### Interviewer trap

If asked:

> "Can you optimize this?"

Do not jump randomly to a more complicated algorithm.

First identify **which complexity is currently limiting the solution**.

---

# 8.4 OS interview

Must answer:

- process/thread
- context switch
- deadlock
- synchronization
- paging
- virtual memory
- scheduling

Scenario:

> "Two threads update the same bank balance. What can go wrong?"

Expected concepts:

- race condition
- atomicity
- synchronization
- locking

---

# 8.5 CN interview

Must answer:

- TCP/UDP
- HTTP/HTTPS
- DNS
- IP
- TCP handshake
- TLS
- cookies/session
- REST
- WebSockets

---

# 8.6 DBMS/SQL

Must be able to:

- write joins
- aggregation
- subqueries
- CTE
- window functions
- explain indexes
- explain transactions
- explain normalization

---

# 8.7 Projects

This is a major separator between average and strong candidates.

For **every project**, know:

```text
Problem
↓
Users
↓
Requirements
↓
Architecture
↓
Tech choices
↓
Database design
↓
API design
↓
Authentication
↓
Error handling
↓
Deployment
↓
Security
↓
Performance
↓
Trade-offs
↓
Failures
↓
Future improvements
```

If a project appears on your resume, assume the interviewer can ask **anything about it**.

### Project trap

> "Why MongoDB?"

"We needed a database" is a weak answer.

A strong answer connects:

**data model → access patterns → consistency → scaling → development constraints → alternatives.**

---

# 8.8 Git/GitHub

Know:

```text
git clone
git status
git add
git commit
git push
git pull
git fetch
git branch
git switch
git merge
git rebase
git stash
git reset
git revert
```

Understand:

- commit
- branch
- merge
- conflict
- remote
- HEAD
- staging area
- `.gitignore`

---

# 8.9 REST/backend

Know:

- HTTP lifecycle
- routing
- controllers
- services
- repositories/data access
- middleware
- authentication
- authorization
- validation
- error handling
- logging
- rate limiting
- caching
- database connection
- transactions

---

# 8.10 Frontend fundamentals

Know:

- HTML semantics
- CSS layout
- Flexbox
- Grid
- JavaScript fundamentals
- DOM
- events
- async/await
- promises
- fetch
- HTTP
- state
- component architecture
- accessibility
- responsive design

---

# 8.11 Basic system design

For junior candidates:

### Learn

- client/server
- API
- database
- cache
- load balancer
- reverse proxy
- CDN
- queues
- horizontal/vertical scaling
- replication
- partitioning
- authentication
- rate limiting

Example:

> Design a URL shortener.

Think:

```text
Client
 ↓
API
 ↓
Service
 ├── Cache
 └── Database
```

Then discuss:

- ID generation
- collisions
- redirects
- caching
- database indexing
- scale
- availability

---

# 8.12 AI/ML fundamentals

For AI-enabled software roles:

### Know

- supervised vs unsupervised learning
- training/validation/test
- overfitting
- underfitting
- classification
- regression
- precision
- recall
- F1
- embeddings
- LLM basics
- tokens
- inference
- RAG
- vector databases
- hallucination
- evaluation
- prompt injection
- model latency/cost

Do not allow AI knowledge to replace DSA/core CS for general CSE placements.

---

# 9. HR / Behavioral Interview

---

# STAR

```text
S — Situation
T — Task
A — Action
R — Result
```

Strong answers are:

- specific
- evidence-based
- concise
- personally owned
- reflective

Weak:

> "I am a hard worker and team player."

Strong:

> specific situation → what you personally did → measurable outcome → lesson.

---

# Common HR questions

Prepare evidence-based answers for:

1. Tell me about yourself.
2. Why should we hire you?
3. Why this company?
4. Why this role?
5. Strengths?
6. Weakness?
7. Biggest failure?
8. Conflict with teammate?
9. Leadership example?
10. Difficult deadline?
11. Technical mistake?
12. How do you learn something new?
13. Where do you see yourself in 5 years?
14. Will you relocate?
15. Salary expectations?
16. Why should we choose you over another candidate?
17. Why did you choose CSE?
18. What project are you most proud of?
19. What would you improve in your project?
20. Questions for us?

---

# PART 2 — PLACEMENT ROADMAP

The roadmap is designed as:

**Diagnostic → Foundation → Intermediate → Advanced → OA → Interview preparation.**

---

# Phase 0 — Diagnostic

## Day 1 diagnostic

### Aptitude

- 20 quantitative
- 10 logical
- 10 verbal
- 30–40 minutes

### Java

20 questions covering:

- syntax
- OOP
- strings
- exceptions
- collections

### DSA

5 coding problems:

- array
- string
- hashmap
- linked list
- binary search

### Core CS

10 OS
10 CN
10 DBMS

---

## Scoring

| Score   | Interpretation                      |
| ------- | ----------------------------------- |
| <40%    | foundation required                 |
| 40–60% | weak/intermediate                   |
| 60–75% | reasonable                          |
| 75–85% | strong                              |
| >85%    | move toward advanced/timed practice |

Also track **time per question**, not just correctness.

---

# Phase 1 — Foundation

### Duration: ~4–6 weeks

### Java

- syntax
- arrays
- strings
- methods
- OOP
- exceptions
- basic collections

### DSA

- arrays
- strings
- linked lists
- stacks
- queues
- hashing
- sorting
- binary search
- recursion

### Aptitude

- number system
- percentages
- ratio
- averages
- profit/loss
- SI/CI
- time/work
- speed/distance

### Core CS

OS:

- process
- thread
- scheduling
- synchronization

CN:

- OSI
- TCP/IP
- TCP/UDP
- IP
- DNS
- HTTP

DBMS:

- keys
- normalization
- SQL
- joins
- transactions

### Exit criterion

Do not advance because "4 weeks are over."

Advance when you can:

- solve most Easy DSA problems independently
- write basic SQL without assistance
- explain OOP
- explain process vs thread
- explain TCP vs UDP
- solve basic aptitude under time pressure

---

# Phase 2 — Intermediate

### Duration: ~5–7 weeks

### Java

- Collections deeply
- HashMap
- Set
- Queue
- generics
- Comparable/Comparator
- lambdas
- streams
- modern Java basics

### DSA

- two pointers
- sliding window
- prefix sum
- hashing
- intervals
- heaps
- monotonic stack
- trees
- BFS/DFS
- greedy

### SQL

- advanced joins
- subqueries
- CTEs
- window functions
- indexes

### Core CS

OS:

- deadlocks
- memory management
- paging
- virtual memory

CN:

- DNS
- DHCP
- NAT
- TLS
- HTTP internals

DBMS:

- ACID
- isolation
- locks
- indexes
- normalization

### Exit criterion

You should be able to solve a substantial set of Medium DSA problems with limited hints and explain your solution verbally.

---

# Phase 3 — Advanced

### Duration: ~5–8 weeks

### DSA

- advanced trees
- graphs
- shortest paths
- MST
- DSU
- topological sort
- DP
- Trie
- segment tree
- Fenwick tree
- advanced graph algorithms

### Core CS

Move from:

**definition → mechanism → scenario → trade-off**

Example:

Instead of:

> "LRU removes least recently used items."

Be able to explain:

- why it exists
- how HashMap + doubly linked list implements it
- complexity
- use cases
- trade-offs

### Interview preparation

Start:

- mock interviews
- project defense
- timed coding
- core CS rapid-fire

---

# Phase 4 — OA Preparation

The OA should be treated as a separate skill.

## Typical OA structure

Possible components:

```text
Aptitude
Logical reasoning
Verbal
Coding
CS MCQs
Debugging
```

Actual structure varies by company.

---

# OA strategy

## Before starting

Quickly classify questions:

```text
A — easy/high confidence
B — medium
C — uncertain/hard
```

Do A first.

---

## Coding

Use:

```text
Read
→ constraints
→ examples
→ brute force
→ optimize
→ code
→ edge cases
→ complexity
```

### Always inspect constraints

They often reveal the expected complexity.

For example:

```text
n ≤ 20
```

may permit exponential techniques.

```text
n ≤ 10^5
```

usually requires approximately $O(n \log n)$ or $O(n)$.

---

# Accuracy vs speed

Target:

> **high accuracy first → increase speed gradually.**

A candidate answering 85% correctly is often stronger than one attempting everything with 60% accuracy.

---

# Phase 5 — Interview Preparation

## Technical round

Practice:

- Java
- OOP
- DSA
- OS
- CN
- DBMS
- SQL
- projects

## Project round

Prepare a 2-minute explanation:

```text
Problem
→ Solution
→ Architecture
→ Your contribution
→ Technical challenge
→ Result
```

Then prepare a **15-minute deep dive**.

---

# PART 3 — DAILY/WEEKLY SCHEDULE

## 1 Hour/Day

| Activity |   Time |
| -------- | -----: |
| DSA      | 25 min |
| Java     | 10 min |
| Aptitude | 10 min |
| Core CS  | 10 min |
| Revision |  5 min |

### Weekly

- 5 DSA sessions
- 5 aptitude sessions
- 5 Java sessions
- 5 Core CS sessions
- 1 mixed test

---

# 2 Hours/Day

| Activity |   Time |
| -------- | -----: |
| DSA      | 45 min |
| Java     | 20 min |
| Aptitude | 20 min |
| Core CS  | 25 min |
| Revision | 10 min |

Weekend:

- 1 timed OA
- 1 project/interview session

---

# 3 Hours/Day

| Activity |   Time |
| -------- | -----: |
| DSA      | 75 min |
| Java     | 25 min |
| Aptitude | 25 min |
| Core CS  | 35 min |
| Revision | 20 min |

Weekly:

- 1 full OA
- 1 technical mock
- 1 project defense

---

# 4+ Hours/Day

| Activity          |    Time |
| ----------------- | ------: |
| DSA               | 100 min |
| Java              |  35 min |
| Aptitude          |  35 min |
| Core CS           |  50 min |
| Interview/project |  30 min |
| Revision          |  30 min |

### Do not turn 4 hours into 4 hours of passive study.

At least **50–60% should involve retrieval or problem solving.**

---

# Weekly cycle

## Monday

DSA + Java + OS

## Tuesday

DSA + Aptitude + CN

## Wednesday

DSA + Java + DBMS

## Thursday

DSA + Aptitude + OS/CN

## Friday

DSA + Java + SQL

## Saturday

Timed DSA + aptitude + Core CS test

## Sunday

```text
Mock OA
↓
Review mistakes
↓
Weak-topic revision
↓
Project/interview practice
```

---

# Monthly milestones

## Month 1

Foundation.

Target:

- Java basics
- OOP
- arrays/strings
- basic aptitude
- OS/CN/DBMS fundamentals

## Month 2

Core DSA patterns + Collections + SQL.

## Month 3

Trees + graphs + intermediate Core CS.

## Month 4

DP + advanced DSA + intensive OAs.

## Month 5+

Company-specific targeting + mocks + interview readiness.

---

# Question Practice Strategy

## DSA

```text
Learn
↓
Understand
↓
Implement
↓
Easy
↓
Medium
↓
Hard
↓
Timed
↓
Revision
```

## Aptitude

```text
Concept
↓
Formula
↓
Basic
↓
Timed
↓
Mixed
↓
Mock OA
```

## Core CS

```text
Concept
↓
Notes
↓
MCQs
↓
Numericals/scenarios
↓
Interview questions
↓
Revision
```

---

# COMPANY-ORIENTED PREPARATION

Preparation should change by company type.

| Company Type        | Aptitude       | DSA          | Core CS        | Coding   | Interview              |
| ------------------- | -------------- | ------------ | -------------- | -------- | ---------------------- |
| Service-based       | High           | Easy–Medium | High           | Moderate | Fundamentals           |
| Product-based       | Moderate–High | Medium–Hard | High           | High     | Deep technical         |
| Startups            | Variable       | Medium–Hard | Moderate–High | High     | Projects + practical   |
| High-paying product | Moderate       | High/Hard    | High           | High     | Deep DSA + CS + design |
| Core IT roles       | High           | Moderate     | High           | Moderate | Domain + CS            |

These are general preparation patterns, not guarantees of a specific company's hiring process.

---

# PART 4 — REVISION SYSTEM

Passive rereading is not an effective revision system.

Use **active recall + spaced repetition + error logs**.

## Daily

Spend 10–20 minutes recalling:

- formulas
- Java concepts
- DSA patterns
- Core CS concepts

without looking at notes.

---

## Weekly

Every Sunday:

```text
What did I learn?
What can I solve?
What did I forget?
What mistakes repeated?
What remains weak?
```

---

# Spaced repetition

Use approximately:

```text
Day 0 → Learn
Day 1 → Recall
Day 3 → Recall
Day 7 → Recall
Day 14 → Recall
Day 30 → Recall
```

---

# Mistake log

Every mistake should contain:

| Field        | Example                |
| ------------ | ---------------------- |
| Problem      | Two Sum                |
| Mistake      | forgot hashmap         |
| Cause        | pattern not recognized |
| Correct idea | complement lookup      |
| Complexity   | $O(n)$               |
| Revisit      | 3 days                 |

Do not merely write:

> "Got wrong."

Record **why**.

---

# Weak-topic tracker

Use:

| Topic         | Score | Status |
| ------------- | ----: | ------ |
| Percentages   |   60% | 🟠     |
| Binary search |   90% | 🟢     |
| Trees         |   45% | 🔴     |
| SQL joins     |   80% | 🟡     |
| Deadlocks     |   50% | 🔴     |

---

# PART 5 — MOCK TEST STRATEGY

## Month 1

1 mock/week.

## Month 2

1–2 mocks/week.

## Month 3

2 mocks/week.

## Active placement period

2–4 timed OAs/week depending on actual company pipeline.

---

# Mock review is more important than mock count

For every mock classify errors:

```text
Conceptual
↓
Pattern recognition
↓
Implementation
↓
Careless
↓
Time management
```

Then fix the category.

---

# Mock interview

Initially:

**1/week**

Later:

**2/week**

Include:

### Round 1

DSA

### Round 2

Java + OOP

### Round 3

OS + CN + DBMS

### Round 4

Project

### Round 5

HR

---

# PART 6 — LAST 30 DAYS STRATEGY

Do **not** attempt to learn the entire syllabus during the final month.

The final month is for:

> **consolidation + speed + accuracy + interview conversion.**

---

## Days 30–21

Focus:

- DSA patterns
- Java
- SQL
- Core CS
- aptitude weaknesses

Daily:

- 2–3 DSA problems
- 20–30 aptitude questions
- 1 Core CS topic
- 20 Java questions

---

## Days 20–14

Increase timed work.

- 2–3 OAs/week
- DSA under time
- SQL timed
- Core CS MCQs
- project defense

---

## Days 13–7

Focus on:

- repeated mistakes
- frequently asked questions
- company-specific requirements
- resume projects
- communication

---

## Days 6–1

No major new syllabus.

Focus on:

- formulas
- DSA patterns
- Java rapid revision
- OS/CN/DBMS questions
- SQL patterns
- project explanation
- HR answers
- sleep and logistics

---

# PART 7 — LAST 7 DAYS STRATEGY

## Day 7

Full OA.

Review every mistake.

## Day 6

DSA + Java.

## Day 5

OS + CN.

## Day 4

DBMS + SQL.

## Day 3

Projects + technical interview.

## Day 2

Mixed rapid revision + one timed coding set.

## Day 1

Light revision only.

Review:

- formulas
- patterns
- common traps
- project architecture
- resume

Do not attempt 8 hours of new material the night before.

---

# PART 8 — MASTER CHECKLIST

Use the following as the actual tracking system.

## Aptitude

### Quantitative

- [ ] Number system
- [ ] Divisibility
- [ ] HCF/LCM
- [ ] Factors
- [ ] Remainders
- [ ] Unit digits
- [ ] Simplification
- [ ] Approximation
- [ ] Percentages
- [ ] Profit/loss
- [ ] Discounts
- [ ] SI
- [ ] CI
- [ ] Ratio
- [ ] Proportion
- [ ] Partnership
- [ ] Averages
- [ ] Mixtures
- [ ] Time/work
- [ ] Pipes/cisterns
- [ ] Speed/distance
- [ ] Trains
- [ ] Boats
- [ ] Relative speed
- [ ] P&C
- [ ] Probability
- [ ] Algebra
- [ ] Linear equations
- [ ] Quadratics
- [ ] Progressions
- [ ] Geometry
- [ ] Mensuration
- [ ] DI
- [ ] Clocks
- [ ] Calendars
- [ ] Ages

### Reasoning

- [ ] Number series
- [ ] Letter series
- [ ] Coding-decoding
- [ ] Analogy
- [ ] Classification
- [ ] Blood relations
- [ ] Directions
- [ ] Ranking
- [ ] Seating
- [ ] Puzzles
- [ ] Syllogisms
- [ ] Conclusions
- [ ] Assumptions
- [ ] Data sufficiency
- [ ] Venn diagrams
- [ ] Input-output
- [ ] Logical deductions

### Verbal

- [ ] RC
- [ ] Grammar
- [ ] Sentence correction
- [ ] Vocabulary
- [ ] Synonyms/antonyms
- [ ] Para jumbles
- [ ] Sentence completion
- [ ] Error detection
- [ ] Fill blanks
- [ ] Active/passive
- [ ] Direct/indirect speech

---

# Java

- [ ] JVM
- [ ] JRE
- [ ] JDK
- [ ] Compilation
- [ ] Primitive types
- [ ] Reference types
- [ ] Variables
- [ ] Operators
- [ ] Control flow
- [ ] Arrays
- [ ] Strings
- [ ] String pool
- [ ] StringBuilder
- [ ] StringBuffer
- [ ] Methods
- [ ] Overloading
- [ ] Pass-by-value
- [ ] static
- [ ] final
- [ ] Packages
- [ ] Access modifiers
- [ ] Wrappers
- [ ] Autoboxing
- [ ] Exceptions
- [ ] GC
- [ ] Memory

## OOP

- [ ] Class/object
- [ ] Constructors
- [ ] Encapsulation
- [ ] Abstraction
- [ ] Inheritance
- [ ] Polymorphism
- [ ] Association
- [ ] Aggregation
- [ ] Composition
- [ ] Overloading
- [ ] Overriding
- [ ] Dynamic dispatch
- [ ] Upcasting
- [ ] Downcasting
- [ ] this
- [ ] super
- [ ] abstract
- [ ] interface
- [ ] default methods
- [ ] static interface methods
- [ ] functional interfaces

## Collections

- [ ] ArrayList
- [ ] LinkedList
- [ ] Vector
- [ ] Stack
- [ ] HashSet
- [ ] LinkedHashSet
- [ ] TreeSet
- [ ] HashMap
- [ ] LinkedHashMap
- [ ] TreeMap
- [ ] Hashtable
- [ ] PriorityQueue
- [ ] Deque
- [ ] ArrayDeque
- [ ] equals/hashCode
- [ ] Comparable
- [ ] Comparator
- [ ] Generics
- [ ] Lambdas
- [ ] Streams
- [ ] Optional
- [ ] Modern Java

---

# DSA

## Fundamentals

- [ ] Variables
- [ ] Conditions
- [ ] Loops
- [ ] Functions
- [ ] Recursion
- [ ] Arrays
- [ ] Strings
- [ ] Mathematics
- [ ] Bit manipulation

## Complexity

- [ ] Big-O
- [ ] Big-Theta
- [ ] Big-Omega
- [ ] Best/average/worst
- [ ] Space complexity
- [ ] Amortized analysis

## Data structures

- [ ] Array
- [ ] String
- [ ] Linked list
- [ ] Stack
- [ ] Queue
- [ ] Deque
- [ ] HashMap
- [ ] HashSet

## Patterns

- [ ] Two pointers
- [ ] Sliding window
- [ ] Prefix sum
- [ ] Difference array
- [ ] Fast/slow
- [ ] Binary search
- [ ] Sorting
- [ ] Hashing
- [ ] Greedy
- [ ] Recursion
- [ ] Backtracking
- [ ] Divide & conquer
- [ ] Monotonic stack
- [ ] Monotonic queue
- [ ] Heap
- [ ] Intervals
- [ ] Matrix traversal
- [ ] Bit manipulation

## Trees

- [ ] Binary tree
- [ ] BST
- [ ] DFS
- [ ] BFS
- [ ] Traversals
- [ ] Height
- [ ] Diameter
- [ ] Balanced tree
- [ ] LCA
- [ ] Views
- [ ] Path problems
- [ ] BST operations
- [ ] AVL basics
- [ ] Heap

## Graphs

- [ ] Representation
- [ ] BFS
- [ ] DFS
- [ ] Components
- [ ] Cycle detection
- [ ] Topological sort
- [ ] Kahn
- [ ] Dijkstra
- [ ] Bellman-Ford
- [ ] Floyd-Warshall
- [ ] MST
- [ ] Prim
- [ ] Kruskal
- [ ] DSU
- [ ] Bipartite
- [ ] Bridges
- [ ] Articulation points
- [ ] SCC
- [ ] DAG

## Advanced

- [ ] 1D DP
- [ ] 2D DP
- [ ] Knapsack
- [ ] Subsequence DP
- [ ] Grid DP
- [ ] Partition DP
- [ ] Interval DP
- [ ] Bitmask DP
- [ ] Tree DP
- [ ] Trie
- [ ] Segment tree
- [ ] Fenwick tree
- [ ] Advanced graphs
- [ ] Advanced strings

---

# OS

- [ ] OS fundamentals
- [ ] Kernel
- [ ] System calls
- [ ] User/kernel mode
- [ ] Processes
- [ ] Threads
- [ ] Process states
- [ ] PCB
- [ ] Context switching
- [ ] Scheduling
- [ ] FCFS
- [ ] SJF
- [ ] SRTF
- [ ] Priority
- [ ] Round Robin
- [ ] Multilevel queue
- [ ] MLFQ
- [ ] Synchronization
- [ ] Race conditions
- [ ] Critical sections
- [ ] Mutex
- [ ] Semaphore
- [ ] Monitor
- [ ] Deadlocks
- [ ] Banker's algorithm
- [ ] Paging
- [ ] Segmentation
- [ ] Virtual memory
- [ ] FIFO
- [ ] LRU
- [ ] Optimal
- [ ] Thrashing
- [ ] Fragmentation
- [ ] File systems
- [ ] Disk scheduling
- [ ] IPC

---

# CN

- [ ] Network fundamentals
- [ ] LAN/WAN/MAN
- [ ] OSI
- [ ] TCP/IP
- [ ] Layers
- [ ] MAC
- [ ] IP
- [ ] IPv4
- [ ] IPv6
- [ ] Subnetting
- [ ] CIDR
- [ ] ARP
- [ ] DHCP
- [ ] DNS
- [ ] NAT
- [ ] Routing
- [ ] Switching
- [ ] TCP
- [ ] UDP
- [ ] TCP handshake
- [ ] TCP termination
- [ ] Flow control
- [ ] Congestion control
- [ ] HTTP
- [ ] HTTPS
- [ ] TLS
- [ ] HTTP methods
- [ ] Status codes
- [ ] Cookies
- [ ] Sessions
- [ ] REST
- [ ] WebSockets
- [ ] SMTP
- [ ] FTP
- [ ] SSH
- [ ] Firewalls
- [ ] Proxies
- [ ] Browser request lifecycle

---

# DBMS + SQL

- [ ] DBMS fundamentals
- [ ] RDBMS
- [ ] Tables
- [ ] Rows
- [ ] Columns
- [ ] Keys
- [ ] Constraints
- [ ] ER diagrams
- [ ] Relationships
- [ ] Cardinality
- [ ] 1NF
- [ ] 2NF
- [ ] 3NF
- [ ] BCNF
- [ ] Functional dependencies
- [ ] Denormalization
- [ ] DDL
- [ ] DML
- [ ] DQL
- [ ] DCL
- [ ] TCL
- [ ] SELECT
- [ ] WHERE
- [ ] GROUP BY
- [ ] HAVING
- [ ] ORDER BY
- [ ] Joins
- [ ] Subqueries
- [ ] Correlated subqueries
- [ ] CTE
- [ ] Views
- [ ] Indexes
- [ ] Clustered/non-clustered
- [ ] Transactions
- [ ] ACID
- [ ] Isolation
- [ ] Locks
- [ ] Deadlocks
- [ ] CAP
- [ ] SQL vs NoSQL
- [ ] MongoDB

---

# GD

- [ ] Opening
- [ ] Entering discussion
- [ ] Supporting argument
- [ ] Disagreeing
- [ ] Evidence
- [ ] Listening
- [ ] Leadership
- [ ] Summarizing
- [ ] Current affairs
- [ ] Technology topics
- [ ] Business topics
- [ ] Social topics
- [ ] Ethical topics
- [ ] Abstract topics
- [ ] 25 practice topics

---

# Technical Interview

- [ ] Programming
- [ ] Java
- [ ] OOP
- [ ] DSA
- [ ] OS
- [ ] CN
- [ ] DBMS
- [ ] SQL
- [ ] Projects
- [ ] Git/GitHub
- [ ] REST
- [ ] Backend
- [ ] Frontend
- [ ] System design
- [ ] AI/ML fundamentals
- [ ] Debugging
- [ ] Scenario questions
- [ ] Follow-up questions
- [ ] Interview traps

---

# HR

- [ ] Tell me about yourself
- [ ] Strengths
- [ ] Weakness
- [ ] Why hire you?
- [ ] Why company?
- [ ] Why role?
- [ ] Career goals
- [ ] Leadership
- [ ] Conflict
- [ ] Failure
- [ ] Teamwork
- [ ] Pressure
- [ ] Deadlines
- [ ] Ethics
- [ ] Adaptability
- [ ] Relocation
- [ ] Salary
- [ ] Questions for interviewer
- [ ] STAR examples

---

# Final execution standard

Your preparation should eventually satisfy **all five levels** for each important topic:

```text
☐ Topic learned
☐ Questions solved
☐ Revised
☐ Timed practice completed
☐ Interview questions completed
```

The distinction that matters is this:

### Basic placement ready

You can:

- solve common Easy/Medium problems
- answer standard Java/OOP questions
- write basic SQL
- explain OS/CN/DBMS fundamentals
- clear ordinary aptitude sections
- explain your projects

### Strong placement ready

You can additionally:

- solve unfamiliar Medium problems
- recognize DSA patterns quickly
- reason about complexity
- debug under time pressure
- write advanced SQL
- explain Java Collections internally
- solve Core CS scenarios
- defend project architecture
- communicate technical reasoning clearly

### Top-tier placement ready

You can additionally:

- handle unfamiliar Hard/upper-Medium DSA
- derive solutions rather than recall them
- optimize systematically
- reason about system trade-offs
- explain OS/CN/DBMS mechanisms deeply
- handle interviewer follow-ups
- discuss project failures and design decisions honestly
- perform under timed OA conditions
- adapt when the interviewer changes the problem

**That last level—not syllabus completion—is the actual target.**
