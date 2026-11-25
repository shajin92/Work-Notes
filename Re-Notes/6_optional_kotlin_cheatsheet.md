# Kotlin cheat sheet for DSA

Everything here is standard library. LeetCode's Kotlin environment supports all of it.

## The tools

```kotlin
fun cheatsheet() {
    // Arrays
    val a = IntArray(5)                    // [0,0,0,0,0]
    val b = intArrayOf(3, 1, 2)
    b.sort()                               // in place, O(n log n)
    val grid = Array(3) { IntArray(4) }    // 3 rows, 4 columns
    val last = b[b.lastIndex]
    for (i in b.indices) { }               // 0 until b.size
    for ((i, x) in b.withIndex()) { }

    // Lists
    val list = ArrayList<Int>()
    list.add(1); list.removeAt(list.lastIndex)
    val sortedDesc = list.sortedDescending()

    // Hash map and hash set: O(1) average for get, put, contains
    val count = HashMap<Char, Int>()
    for (c in "hello") count[c] = (count[c] ?: 0) + 1
    val seen = HashSet<Int>()
    if (seen.add(7)) { /* add returns false if already present */ }
    val groups = HashMap<String, MutableList<String>>()
    groups.getOrPut("key") { ArrayList() }.add("value")

    // Stack and queue: use ArrayDeque for both
    val stack = ArrayDeque<Int>()
    stack.addLast(1); val top = stack.last(); stack.removeLast()
    val queue = ArrayDeque<Int>()
    queue.addLast(1); val front = queue.removeFirst()

    // Heap (priority queue)
    val minHeap = java.util.PriorityQueue<Int>()
    val maxHeap = java.util.PriorityQueue<Int>(compareByDescending { it })
    val byFirst = java.util.PriorityQueue<IntArray>(compareBy { it[0] })
    minHeap.add(5); minHeap.peek(); minHeap.poll()

    // Strings
    val s = "abc"
    val chars = s.toCharArray()
    val sb = StringBuilder()
    sb.append('x'); sb.toString()
    val idx = s[0] - 'a'                   // letter to 0..25
    val key = s.toCharArray().sorted().joinToString("")

    // Math
    val mid = 0 + (10 - 0) / 2             // avoids overflow
    val big = Int.MAX_VALUE; val small = Int.MIN_VALUE
    val m = maxOf(1, 2); val n = minOf(1, 2)
    val total: Long = 1L * 100000 * 100000 // use Long when sums can exceed 2 billion
}
```

## Complexity you should know by heart

| Operation | Cost |
|---|---|
| Array index, HashMap get/put, HashSet contains | O(1) |
| ArrayDeque add/remove at either end | O(1) |
| PriorityQueue add / poll | O(log n) |
| PriorityQueue peek | O(1) |
| Sorting | O(n log n) |
| Binary search | O(log n) |
| `list.contains`, `list.indexOf`, `list.removeAt(0)` | O(n) — avoid inside loops |
| String `+` inside a loop | O(n) each time — use StringBuilder |

## Reading the constraints

The input size tells you the complexity they expect.

| n up to | Expected solution |
|---|---|
| 20 | O(2^n), backtracking |
| 1,000 | O(n²) |
| 100,000 | O(n log n) or O(n) |
| 1,000,000 or more | O(n) or O(log n) |

## Common mistakes in Kotlin

- `IntArray` and `Array<Int>` are different types. LeetCode signatures use `IntArray`.
- `for (i in 0..n)` includes `n`. Use `0 until n` for indices.
- `map[key]` returns a nullable value. Handle it with `?: 0` or a null check.
- `list.remove(x)` on a `MutableList<Int>` removes the value `x`, not index `x`. Use `removeAt(i)` for an index.
- `java.util.Stack` works but is slow and old. Use `ArrayDeque`.
- Deep recursion (over about 10,000 levels) can overflow the stack. Switch to a loop with your own stack.
