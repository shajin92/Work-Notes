# Week 0 — Basics from zero (Days B1–B7)

Start here if you have never studied DSA. No prior knowledge needed beyond writing Kotlin.

Each day has three parts: **read** the explanation, **type** the example code yourself and run it, then **solve** the exercises on LeetCode by hand, with no AI help. About 2 hours a day.

## What DSA is, in plain words

- A **data structure** is a way to store data so that some operation is fast. A list is good for "give me item number 5". A hash map is good for "do I have this key?".
- An **algorithm** is the steps you take to solve a problem using that data.
- An interview problem is nearly always: "here is some data, compute something, and do it fast". Your job is to pick the right structure so the steps stay cheap.

You already use these at work. A Room index is a tree. A `HashMap` cache is hashing. A back stack is a stack. A WorkManager queue is a queue. This week gives them names and rules.

## How to use LeetCode

1. Open the problem link, set the language to **Kotlin**.
2. Read the examples and the constraints at the bottom.
3. Write your code in the editor, press **Run** to try the samples, then **Submit**.
4. If it fails, read the failing input, work it out on paper, and fix it. That loop is the learning.

---

## Day B1 — Arrays, loops and Big-O

**Array.** A row of boxes, numbered from 0. Reading box number `i` is instant.

```
index:   0   1   2   3
value: [ 7 | 2 | 9 | 4 ]
```

```kotlin
fun sumOfArray(nums: IntArray): Int {
    var total = 0
    for (x in nums) total += x
    return total
}

fun largest(nums: IntArray): Int {
    var best = nums[0]
    for (x in nums) if (x > best) best = x
    return best
}
```

**Big-O: how the work grows as the input grows.** Do not count seconds; count how many times the inner line runs for `n` items.

| Name | Meaning | Example |
|---|---|---|
| O(1) | Same work whatever the size | Read `nums[3]` |
| O(log n) | Halve the problem each step | Find a word in a dictionary |
| O(n) | Touch every item once | `sumOfArray` above |
| O(n log n) | Sort | `nums.sort()` |
| O(n²) | Every item against every other item | The function below |

```kotlin
fun hasPairWithSum(nums: IntArray, target: Int): Boolean {
    for (i in nums.indices) {
        for (j in i + 1 until nums.size) {      // loop inside a loop: O(n²)
            if (nums[i] + nums[j] == target) return true
        }
    }
    return false
}
```

For 100,000 items, O(n) is 100,000 steps and O(n²) is 10,000,000,000. That is why interviews care: the slow version times out.

**Space** is the same idea for memory: a new array of `n` items is O(n) space; a few variables is O(1).

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Running Sum of 1d Array](https://leetcode.com/problems/running-sum-of-1d-array/) | One loop, building a result array |
| 2 | [Richest Customer Wealth](https://leetcode.com/problems/richest-customer-wealth/) | Loop inside a loop on a grid |
| 3 | [Max Consecutive Ones](https://leetcode.com/problems/max-consecutive-ones/) | Keeping a running count and a best |

**Done when:** you can look at a loop and say whether it is O(n) or O(n²).

---

## Day B2 — Strings and characters

**String.** An array of characters that you cannot change in place. To edit, convert to a `CharArray` or build a new one with `StringBuilder`.

```kotlin
fun reverseText(s: String): String {
    val chars = s.toCharArray()
    var l = 0
    var r = chars.lastIndex
    while (l < r) {                              // swap the two ends, move inward
        val tmp = chars[l]
        chars[l] = chars[r]
        chars[r] = tmp
        l++
        r--
    }
    return String(chars)
}

fun countVowels(s: String): Int {
    var n = 0
    for (c in s) if (c in "aeiouAEIOU") n++
    return n
}
```

Useful facts:
- `s[i]` reads one character; `s.length` is the size.
- `c - 'a'` turns a lowercase letter into a number 0–25, handy for counting letters in an `IntArray(26)`.
- `c.isLetterOrDigit()`, `c.lowercaseChar()`, `c.isDigit()` exist.
- Adding to a string inside a loop (`s += c`) copies the whole string each time. Use `StringBuilder`.

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Reverse String](https://leetcode.com/problems/reverse-string/) | Two indexes moving toward each other |
| 2 | [Length of Last Word](https://leetcode.com/problems/length-of-last-word/) | Walking a string from the end |
| 3 | [Fizz Buzz](https://leetcode.com/problems/fizz-buzz/) | Conditions and building a list of strings |

**Done when:** you can reverse a string without looking at the example.

---

## Day B3 — Hash map and hash set

This is the most useful structure in interviews. Learn it well.

**Hash set.** A bag of unique values that answers "have I seen this?" instantly.
**Hash map.** A set of keys, each with a value attached: "how many times have I seen this?", "where did I see it?".

Both find, add and remove in O(1) on average, no matter how many items are inside. A list needs O(n) for the same question.

```kotlin
fun letterCounts(s: String): Map<Char, Int> {
    val count = HashMap<Char, Int>()
    for (c in s) count[c] = (count[c] ?: 0) + 1   // ?: 0 handles "not seen yet"
    return count
}

fun hasDuplicate(nums: IntArray): Boolean {
    val seen = HashSet<Int>()
    for (x in nums) {
        if (!seen.add(x)) return true              // add returns false if x was already there
    }
    return false
}
```

The habit to build: whenever you write a loop inside a loop to search for something, ask "could a map or set remember that for me?" That turns O(n²) into O(n).

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Jewels and Stones](https://leetcode.com/problems/jewels-and-stones/) | Set membership |
| 2 | [Number of Good Pairs](https://leetcode.com/problems/number-of-good-pairs/) | Counting with a map |
| 3 | [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/) | Count first, then scan again |
| 4 | [Intersection of Two Arrays](https://leetcode.com/problems/intersection-of-two-arrays/) | Two sets |

**Done when:** you can write the letter-count loop from memory.

---

## Day B4 — Stack and queue

**Stack.** Last in, first out, like a pile of plates. You only touch the top. Android's back stack works this way.
**Queue.** First in, first out, like a line at a counter.

```
stack:  push 1, push 2, push 3  ->  pop gives 3
queue:  add 1,  add 2,  add 3   ->  remove gives 1
```

In Kotlin use `ArrayDeque` for both.

```kotlin
fun stackAndQueueDemo(): List<Int> {
    val stack = ArrayDeque<Int>()
    stack.addLast(1); stack.addLast(2); stack.addLast(3)
    val fromStack = stack.removeLast()             // 3

    val queue = ArrayDeque<Int>()
    queue.addLast(1); queue.addLast(2); queue.addLast(3)
    val fromQueue = queue.removeFirst()            // 1

    return listOf(fromStack, fromQueue)
}

fun removeAdjacentPairs(s: String): String {       // "abbaca" -> "ca"
    val stack = ArrayDeque<Char>()
    for (c in s) {
        if (stack.isNotEmpty() && stack.last() == c) stack.removeLast()
        else stack.addLast(c)
    }
    return stack.joinToString("")
}
```

Use a stack when the newest unfinished thing must be handled first (matching brackets, undo). Use a queue when things must be handled in arrival order (level by level, nearest first).

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Baseball Game](https://leetcode.com/problems/baseball-game/) | Push, pop and peek |
| 2 | [Remove All Adjacent Duplicates In String](https://leetcode.com/problems/remove-all-adjacent-duplicates-in-string/) | The example above; write it without looking |
| 3 | [Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/) | Understanding both orders |

**Done when:** you can say which of the two you need for a given situation, and why.

---

## Day B5 — Recursion

A function that calls itself on a smaller version of the same problem. Every recursive function needs two parts:

1. **Base case:** the smallest input, answered directly, with no further call.
2. **Recursive case:** shrink the input and call yourself.

Without a base case it never stops and crashes with a stack overflow.

```kotlin
fun factorial(n: Int): Long =
    if (n <= 1) 1L                                  // base case
    else n * factorial(n - 1)                       // smaller problem

fun sumDigits(n: Int): Int =
    if (n < 10) n
    else n % 10 + sumDigits(n / 10)

fun fib(n: Int): Int =
    if (n < 2) n
    else fib(n - 1) + fib(n - 2)
```

Trace `factorial(4)` on paper: it waits for `factorial(3)`, which waits for `factorial(2)`, which waits for `factorial(1)` = 1, then the answers come back up: 2, 6, 24. Do this trace by hand once; it is the whole idea.

`fib` above is correct but slow (it recomputes the same values many times). Fixing that by storing answers is called dynamic programming; you will meet it in week 2.

Recursion matters because trees and graphs are solved with it.

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | Base case and recursive case |
| 2 | [Power of Two](https://leetcode.com/problems/power-of-two/) | Solve it with recursion: halve until 1 |
| 3 | [Reverse String](https://leetcode.com/problems/reverse-string/) | Redo day B2's problem recursively |

**Done when:** you can point to the base case in any recursive function.

---

## Day B6 — Sorting and searching

**Sorting.** You almost never write a sort in an interview; you call the built-in one and know it costs O(n log n). Sorting first often makes a hard problem easy, because equal or close values end up next to each other.

```kotlin
fun sortingDemo(): List<Int> {
    val a = intArrayOf(5, 2, 9, 1)
    a.sort()                                        // [1, 2, 5, 9], in place
    val people = listOf("Ravi" to 31, "Anu" to 25)
    val youngestFirst = people.sortedBy { it.second }
    return a.toList() + youngestFirst.first().second
}
```

**Searching.**
- **Linear search:** check every item. O(n). Works on anything.
- **Binary search:** only on sorted data. Look at the middle; if your target is smaller, throw away the right half, otherwise the left half. Repeat. O(log n): a million items take about 20 steps.

```
find 7 in [1, 3, 5, 7, 9, 11, 13]
middle is 7 -> found in 1 step
find 3: middle 7 (too big) -> left half [1, 3, 5] -> middle 3 -> found
```

You will write binary search yourself on day 5 of week 1. Today, understand why it needs sorted data.

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Majority Element](https://leetcode.com/problems/majority-element/) | Solve it by sorting, then again with a map |
| 2 | [Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/) | Square then sort; note the complexity |
| 3 | [Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array/) | Walking two sorted arrays together |

**Done when:** you can explain to someone why binary search is faster and when it cannot be used.

---

## Day B7 — Linked lists, trees, and a checkpoint

**Linked list.** Each item (a node) holds a value and a pointer to the next node. There are no index numbers; to reach the fifth node you walk from the first.

```
[3] -> [8] -> [1] -> null
```

```kotlin
class BasicListNode(val value: Int) { var next: BasicListNode? = null }

fun listLength(head: BasicListNode?): Int {
    var count = 0
    var cur = head
    while (cur != null) {                           // walk until the end
        count++
        cur = cur.next
    }
    return count
}
```

**Tree.** Each node can point to several children. In a binary tree, at most two: left and right. The top node is the root; a node with no children is a leaf.

```
      5
     / \
    3   8
   /
  1
```

```kotlin
class BasicTreeNode(val value: Int) {
    var left: BasicTreeNode? = null
    var right: BasicTreeNode? = null
}

fun countNodes(root: BasicTreeNode?): Int =
    if (root == null) 0                             // base case: empty tree
    else 1 + countNodes(root.left) + countNodes(root.right)
```

Notice that `countNodes` is recursion from day B5: an empty tree is the base case, and each child is a smaller tree.

**Graph.** Like a tree, but any node can connect to any other, and there can be loops. A map of cities and roads is a graph. You will work with them in week 2.

| # | Exercise | What it practises |
|---|---|---|
| 1 | [Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/) | Walking a list |
| 2 | [Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/) | Recursion on a tree: left, node, right |

### Checkpoint (60 minutes, no help)

Try these three. They are the first real interview problems, and all three return in week 1 with full explanations.

1. [Two Sum](https://leetcode.com/problems/two-sum/)
2. [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
3. [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

| Solved alone | What to do |
|---|---|
| 0 | Normal. Redo days B3 and B4 once more, then start week 1. |
| 1–2 | On track. Start week 1. |
| 3 | Ahead. Start week 1 and skip its easy problems. |

A brute-force answer that passes counts as solved. Speed comes later.
