# 🎯 6-Month Android + DSA Learning Plan
> **Goal:** Crack any level of Android interview in 6 months  
> **Current Level:** Intermediate Android Developer  
> **Books:** Manifest Android Interview (Jaewoong Eum) + Practical Kotlin Deep Dive  

---

## 📋 How to Use This Plan
- Each week has **Android/Kotlin topics** + **DSA topics**
- Check off items as you complete them
- Spend ~2 hrs/day on Android concepts, ~1 hr/day on DSA
- For each topic: read → understand internals → write code → solve practice Qs

---

## 🗓️ PHASE 1 — Months 1–2: Foundations & Core Android (Weeks 1–8)

### ✅ Week 1 — Kotlin Language Fundamentals I
**Kotlin (Practical Kotlin Deep Dive — Ch. 0)**
- [ ] Null safety — how `?`, `!!`, `?.`, `?:` work under the hood
- [ ] `Any`, `Unit`, `Nothing` — when and why each is used
- [ ] `data class` vs regular class — `copy()`, `equals()`, `hashCode()` internals
- [ ] `sealed class` — use cases and how they differ from enums
- [ ] `inner class` vs `nested class` — memory implications

**DSA — Arrays & Strings**
- [ ] Array traversal, two-pointer technique
- [ ] Sliding window pattern
- [ ] String manipulation basics
- [ ] Practice: 5 easy LeetCode array problems

---

### ✅ Week 2 — Kotlin Language Fundamentals II
**Kotlin (Ch. 0 continued)**
- [ ] `enum class` — when to use vs sealed class
- [ ] `value class` (inline class) — performance benefits
- [ ] `object` vs regular class — singletons, companion objects
- [ ] `var` vs `val` — backing fields and backing properties
- [ ] Delegated properties — `by lazy`, `by observable`, custom delegates

**DSA — Linked Lists**
- [ ] Singly & doubly linked lists
- [ ] Reverse a linked list
- [ ] Detect cycle (Floyd's algorithm)
- [ ] Practice: 5 linked list problems

---

### ✅ Week 3 — Kotlin Language Fundamentals III
**Kotlin (Ch. 0 continued)**
- [ ] `lazy` delegate vs `lateinit` — lifecycle differences
- [ ] Visibility modifiers — `internal`, `protected` in depth
- [ ] `init` block — invocation order with constructor
- [ ] Higher-order functions — advantages and real use cases
- [ ] `inline` keyword — benefits and limitations (no lambda object creation)

**DSA — Stacks & Queues**
- [ ] Stack using array and linked list
- [ ] Queue, Deque
- [ ] Monotonic stack
- [ ] Practice: valid parentheses, min stack, daily temperatures

---

### ✅ Week 4 — Kotlin Language Fundamentals IV
**Kotlin (Ch. 0 continued)**
- [ ] `reified` keyword — why generics are erased and how reified solves it
- [ ] Lambda expressions — how they compile (SAM conversion)
- [ ] Functional (SAM) interfaces
- [ ] Extension functions — pros, cons, and how they compile
- [ ] Structural vs referential equality (`==` vs `===`)
- [ ] DSL building in Kotlin

**DSA — Hashing**
- [ ] HashMap internals
- [ ] HashSet
- [ ] Frequency counting, grouping patterns
- [ ] Practice: two sum, group anagrams, top K frequent elements

---

### ✅ Week 5 — Kotlin Standard Library
**Kotlin (Ch. 1)**
- [ ] Collection types: `List`, `Set`, `Map` — mutable vs immutable
- [ ] Transformation operators: `map`, `filter`, `flatMap`, `fold`, `reduce`
- [ ] Iterator mechanism — how `for` loops work internally
- [ ] `Sequence` vs `Iterable` — lazy evaluation, when to prefer each
- [ ] Ordering: `Comparable`, `Comparator`, `sortedBy`, `sortedWith`
- [ ] Scope functions: `let`, `run`, `with`, `apply`, `also` — when to use each

**DSA — Binary Search**
- [ ] Classic binary search
- [ ] Search in rotated array
- [ ] Find first/last position
- [ ] Practice: 5 binary search problems

---

### ✅ Week 6 — Android Framework I
**Android (Manifest Android Interview — Category 0)**
- [ ] What is Android? Architecture layers (Linux Kernel → HAL → ART → Framework → Apps)
- [ ] Android Runtime (ART) vs Dalvik — AOT, JIT, DEX compilation
- [ ] Intent — explicit vs implicit, Intent filters in manifest
- [ ] PendingIntent — why it exists, flags (`FLAG_UPDATE_CURRENT`, `FLAG_IMMUTABLE`)
- [ ] Serializable vs Parcelable — performance difference, why Parcelable is preferred
- [ ] Context — types: Application, Activity, Service; memory leak risks

**DSA — Trees I**
- [ ] Binary tree traversals (inorder, preorder, postorder, BFS)
- [ ] Tree height, diameter
- [ ] Practice: 5 tree problems

---

### ✅ Week 7 — Android Framework II
**Android (Category 0 continued)**
- [ ] `Application` class — use cases, initialization patterns
- [ ] `AndroidManifest.xml` — all key elements, permissions, components
- [ ] Activity lifecycle — every callback with real scenarios
- [ ] Fragment lifecycle — `onCreateView` vs `onViewCreated`, memory leak with views
- [ ] Configuration changes — `onSaveInstanceState`, `ViewModel` retention

**DSA — Trees II**
- [ ] Binary Search Tree (BST) — insert, delete, search
- [ ] Lowest Common Ancestor (LCA)
- [ ] Validate BST
- [ ] Practice: 5 BST problems

---

### ✅ Week 8 — Android Framework III
**Android (Category 0 continued)**
- [ ] `Service` — started vs bound service, foreground service
- [ ] `BroadcastReceiver` — static vs dynamic, `LocalBroadcastManager`
- [ ] `ContentProvider` — when to use, URI structure
- [ ] Memory management — GC, memory leaks, `LeakCanary`
- [ ] ANR — causes, how to detect and prevent
- [ ] Deep links — implementation and testing

**DSA — Recursion & Backtracking**
- [ ] Recursion fundamentals and call stack
- [ ] Permutations and combinations
- [ ] Backtracking template
- [ ] Practice: subsets, N-queens, word search

---

## 🗓️ PHASE 2 — Month 3: Android UI & Jetpack Core (Weeks 9–13)

### ✅ Week 9 — Android UI Internals
**Android (Category 1 — Android UI Views)**
- [ ] View lifecycle — `onMeasure`, `onLayout`, `onDraw`
- [ ] `View` vs `ViewGroup` — how layout passes work
- [ ] Custom Views — when and how to create them
- [ ] Canvas API — drawing primitives, Paint object
- [ ] View invalidation — `invalidate()` vs `requestLayout()`

**DSA — Sorting Algorithms**
- [ ] Merge sort, quick sort (understand complexity)
- [ ] Counting sort, radix sort
- [ ] Practice: sort colors, merge intervals, meeting rooms

---

### ✅ Week 10 — Android UI — RecyclerView & Layouts
**Android (Category 1 continued)**
- [ ] `RecyclerView` internals — `RecycledViewPool`, `LayoutManager`, `ItemAnimator`
- [ ] `DiffUtil` — how it works, why it's important
- [ ] `ConstraintLayout` — constraint chains, barriers, guidelines
- [ ] `ViewStub` — lazy inflation use case
- [ ] `SurfaceView` vs `TextureView` — when to use each
- [ ] `ViewBinding` vs `DataBinding` vs `findViewById`

**DSA — Graphs I**
- [ ] Graph representation (adjacency list/matrix)
- [ ] BFS and DFS traversal
- [ ] Connected components
- [ ] Practice: number of islands, clone graph

---

### ✅ Week 11 — Android UI — Drawing, Animations & Bitmaps
**Android (Category 1 continued)**
- [ ] Animations — `ObjectAnimator`, `ValueAnimator`, `MotionLayout`
- [ ] `Drawable` types — `LayerDrawable`, `StateListDrawable`, `VectorDrawable`
- [ ] Bitmap handling — efficient loading, `BitmapFactory.Options`, sampling
- [ ] Nine-patch images — when they're needed
- [ ] `Window` system — how Activities attach to WindowManager
- [ ] Dp vs Sp — display density, accessibility scaling

**DSA — Graphs II**
- [ ] Topological sort
- [ ] Dijkstra's shortest path
- [ ] Union-Find (Disjoint Set)
- [ ] Practice: course schedule, network delay time

---

### ✅ Week 12 — Jetpack Libraries I
**Android (Category 2 — Jetpack Library)**
- [ ] `LiveData` — lifecycle awareness, `MutableLiveData`, `Transformations`
- [ ] `ViewModel` — survives config changes, `ViewModelProvider`, scoping
- [ ] `Room` — entities, DAOs, migrations, relations
- [ ] `Navigation` — NavGraph, SafeArgs, back stack management, deep links
- [ ] `WorkManager` — constraints, chaining, one-time vs periodic

**DSA — Dynamic Programming I**
- [ ] Memoization vs tabulation
- [ ] Fibonacci, climbing stairs, coin change
- [ ] Practice: house robber, longest common subsequence

---

### ✅ Week 13 — Jetpack Libraries II + Business Logic
**Android (Category 2–3)**
- [ ] `Hilt` — how it wraps Dagger, component hierarchy, scopes
- [ ] `Paging 3` — `PagingSource`, `RemoteMediator`, how it handles loading states
- [ ] Baseline Profiles — what they do, how to generate them
- [ ] JSON serialization — Gson vs Moshi vs kotlinx-serialization
- [ ] Network requests — Retrofit + OkHttp, interceptors, error handling

**DSA — Dynamic Programming II**
- [ ] Knapsack problem (0/1 and unbounded)
- [ ] Longest increasing subsequence
- [ ] Matrix chain multiplication
- [ ] Practice: 5 medium DP problems

---

## 🗓️ PHASE 3 — Month 4: Coroutines & Flow (Weeks 14–17)

### ✅ Week 14 — Coroutines I
**Kotlin (Ch. 2 — Coroutines)**
- [ ] What are coroutines? — how they differ from threads
- [ ] `suspend` functions — `Continuation` under the hood (state machine compilation)
- [ ] Coroutine builders — `launch`, `async`, `runBlocking`
- [ ] `CoroutineScope` — `viewModelScope`, `lifecycleScope`, custom scopes
- [ ] `Job` — lifecycle, `cancel()`, `join()`

**DSA — Heaps / Priority Queue**
- [ ] Min heap, max heap
- [ ] K largest/smallest elements
- [ ] Merge K sorted lists
- [ ] Practice: top K frequent, find median from data stream

---

### ✅ Week 15 — Coroutines II
**Kotlin (Ch. 2 continued)**
- [ ] `CoroutineContext` — combining elements
- [ ] Dispatchers — `Main`, `IO`, `Default`, `Unconfined` — when to use each
- [ ] Exception handling — `SupervisorJob`, `CoroutineExceptionHandler`, `try/catch`
- [ ] `channels` — hot streams, `produce`, `actor`
- [ ] `yield()` and `join()` — cooperative cancellation

**DSA — Trie**
- [ ] Trie data structure implementation
- [ ] Insert, search, startsWith
- [ ] Practice: word search II, implement trie, design search autocomplete

---

### ✅ Week 16 — Kotlin Flow I
**Kotlin (Ch. 2 continued)**
- [ ] Cold flow vs Hot flow — fundamental difference
- [ ] `Flow` builders — `flow {}`, `flowOf`, `asFlow`
- [ ] `StateFlow` vs `SharedFlow` — differences, use cases in Android
- [ ] `flowOn()` — upstream context switching
- [ ] `buffer()` — how it decouples producer and collector
- [ ] Collecting flows safely in Android — `repeatOnLifecycle`

**DSA — Bit Manipulation**
- [ ] AND, OR, XOR, NOT, shifts
- [ ] Common tricks: check bit, set bit, clear bit
- [ ] Practice: single number, counting bits, reverse bits

---

### ✅ Week 17 — Kotlin Flow II
**Kotlin (Ch. 2 continued)**
- [ ] `flatMapLatest` vs `flatMapMerge` vs `flatMapConcat` — cancellation behavior
- [ ] `callbackFlow` and `channelFlow` — bridging callback APIs
- [ ] `FusibleFlow` and `ChannelFlow` — internal optimization
- [ ] `launchIn(scope)` vs `scope.launch { collect() }` — difference
- [ ] Common flow operators: `debounce`, `distinctUntilChanged`, `combine`, `zip`

**DSA — Intervals & Greedy**
- [ ] Interval merging, insertion
- [ ] Activity selection, jump game
- [ ] Greedy technique patterns
- [ ] Practice: 5 interval/greedy problems

---

## 🗓️ PHASE 4 — Month 5: Jetpack Compose (Weeks 18–22)

### ✅ Week 18 — Compose Fundamentals
**Android (Jetpack Compose — Category 0)**
- [ ] What is Compose? — declarative vs imperative UI
- [ ] Composable functions — `@Composable` annotation internals
- [ ] Recomposition — when it triggers, how to minimize it
- [ ] `remember` vs `rememberSaveable` — survival across recompositions/config change
- [ ] `State` and `MutableState` — `by remember { mutableStateOf() }`
- [ ] State hoisting pattern

**DSA — Advanced Trees**
- [ ] AVL trees, Red-Black trees (conceptual)
- [ ] Segment tree basics
- [ ] Practice: range sum query, LRU cache

---

### ✅ Week 19 — Compose Runtime
**Android (Jetpack Compose — Category 1: Compose Runtime)**
- [ ] Compose Runtime internals — slot table, gap buffer
- [ ] `CompositionLocal` — how it propagates values down the tree
- [ ] `SideEffect`, `LaunchedEffect`, `DisposableEffect` — when to use each
- [ ] `derivedStateOf` — preventing unnecessary recompositions
- [ ] `snapshotFlow` — converting State to Flow
- [ ] `produceState` — converting non-Compose state

**DSA — Math & Number Theory**
- [ ] GCD, LCM, prime sieve
- [ ] Modular arithmetic
- [ ] Practice: 5 math problems

---

### ✅ Week 20 — Compose UI I
**Android (Jetpack Compose — Category 2: Compose UI)**
- [ ] Modifier — internals, ordering matters
- [ ] Layout composables — `Box`, `Row`, `Column`, `LazyColumn`
- [ ] Custom layouts — `Layout` composable, measuring children
- [ ] `Canvas` in Compose — `drawWithContent`, `drawBehind`
- [ ] Gestures — `detectTapGestures`, `pointerInput`, `Draggable`

**DSA — Sliding Window (Advanced)**
- [ ] Variable size sliding window
- [ ] Minimum window substring
- [ ] Longest substring without repeating
- [ ] Practice: 5 medium sliding window problems

---

### ✅ Week 21 — Compose UI II + Navigation
**Android (Compose continued)**
- [ ] Animations in Compose — `animate*AsState`, `AnimatedVisibility`, `Crossfade`, `Transition`
- [ ] Compose Navigation — `NavHost`, `NavController`, back stack, deep links
- [ ] `ViewModel` with Compose — `viewModel()`, `hiltViewModel()`
- [ ] Compose + `LazyColumn` performance — `key`, `items` vs `itemsIndexed`
- [ ] Theming — `MaterialTheme`, `ColorScheme`, `Typography`, custom themes

**DSA — Two Pointers (Advanced)**
- [ ] 3Sum, 4Sum
- [ ] Container with most water
- [ ] Trapping rain water
- [ ] Practice: 5 two-pointer problems

---

### ✅ Week 22 — Compose Testing + Interop
**Android (Compose continued)**
- [ ] Compose testing — `ComposeTestRule`, `onNodeWithText`, semantics
- [ ] Compose + XML interop — `AndroidView`, `ComposeView`
- [ ] Compose performance — baseline profiles, `@Stable`, `@Immutable` annotations
- [ ] `collectAsState()` vs `collectAsStateWithLifecycle()`
- [ ] Lazy layout optimization — `contentType`, `key`

**DSA — Design Patterns in DSA**
- [ ] Iterator pattern
- [ ] Design problems: LRU Cache, LFU Cache
- [ ] Practice: design Twitter, design file system

---

## 🗓️ PHASE 5 — Month 6: Advanced Topics + Interview Prep (Weeks 23–26)

### ✅ Week 23 — Kotlin Advanced: KotlinX + Compiler
**Kotlin (Ch. 3–4)**
- [ ] `kotlinx-serialization` — how it works, `@Serializable`, custom serializers
- [ ] `kotlinx-datetime` — why it's better than Java time APIs
- [ ] `kotlinx-collections-immutable` — `PersistentList`, `PersistentMap`
- [ ] KAPT vs KSP — symbol processing, annotation processors
- [ ] K2 compiler — FIR, IR backend, what changed
- [ ] Kotlin Compiler Plugins — real-world examples (Compose, Parcelize, Serialization)

**DSA — Hard Problem Practice**
- [ ] Median of two sorted arrays
- [ ] Longest valid parentheses
- [ ] Practice: 5 hard problems from LeetCode

---

### ✅ Week 24 — Kotlin Multiplatform + Advanced Android
**Kotlin (Ch. 5) + Android Advanced**
- [ ] KMP — expect/actual mechanism, source sets
- [ ] Compose Multiplatform — current state and limitations
- [ ] Kotlin/Native memory management — the new MM model
- [ ] Android Process management — foreground/background process priority
- [ ] R8 / ProGuard — how code shrinking works, keep rules
- [ ] APK vs AAB — Play App Signing, Dynamic Delivery

**DSA — Graph Advanced**
- [ ] Bellman-Ford, Floyd-Warshall
- [ ] Minimum spanning tree (Kruskal, Prim)
- [ ] Practice: 5 hard graph problems

---

### ✅ Week 25 — Architecture & Testing Deep Dive
**Android Architecture (not in books — essential additions)**
- [ ] Clean Architecture — data/domain/presentation layers
- [ ] MVVM vs MVI — unidirectional data flow, why MVI suits Compose
- [ ] Repository pattern — single source of truth
- [ ] Dependency Injection — manual DI → Hilt module design
- [ ] Unit testing — `JUnit`, `MockK`, `Turbine` (for Flow testing)
- [ ] UI testing — Espresso, Compose test APIs
- [ ] `FakeRepository` pattern vs mocking

**DSA — Final Sprint I**
- [ ] Revisit weakest DSA areas
- [ ] Practice: 2 mock interview sessions on NeetCode/LeetCode

---

### ✅ Week 26 — Full Interview Simulation Week
**Android Mock Questions (from Manifest Android Interview book)**
- [ ] Do 5 Android framework questions without notes — time yourself
- [ ] Do 5 Jetpack Compose questions without notes
- [ ] Do 5 Kotlin coroutine/flow questions without notes
- [ ] Review all "Pro Tips for Mastery" sections in the book
- [ ] Write your answers to: "Tell me about yourself", "Biggest Android challenge you solved"

**DSA — Final Sprint II**
- [ ] 2 full timed mock interviews (45 min each)
- [ ] Review all DSA topics you felt weak in
- [ ] Study time/space complexity for every pattern learned

---

## 📚 Topics NOT in Books — Must Cover Separately

These are essential for senior/lead level interviews, not fully covered in the books:

### Android Architecture
- [ ] Clean Architecture (Uncle Bob) applied to Android
- [ ] MVVM with Compose — proper ViewModel → StateFlow → Compose
- [ ] MVI pattern — `Intent → State → Effect`
- [ ] Repository + UseCases pattern

### Performance & Profiling
- [ ] Android Profiler — CPU, Memory, Network
- [ ] Systrace / Perfetto
- [ ] Startup optimization — `App Startup` library, splash screen
- [ ] Reducing jank — 16ms frame budget, `Choreographer`

### Security
- [ ] `ProGuard` / R8 — obfuscation
- [ ] Encrypted SharedPreferences
- [ ] Certificate pinning with OkHttp
- [ ] Android Keystore

### Testing
- [ ] TDD in Android
- [ ] `MockK` vs `Mockito`
- [ ] `Turbine` for testing Flows
- [ ] End-to-end testing with `UiAutomator`

### Networking & Storage
- [ ] OkHttp interceptors — auth, logging, caching
- [ ] Room advanced — `@Transaction`, `@Relation`, migrations
- [ ] DataStore vs SharedPreferences
- [ ] File I/O, Scoped Storage (Android 10+)

### CI/CD & Build
- [ ] Gradle — build types, flavors, `buildSrc` / version catalogs
- [ ] GitHub Actions / Bitrise for Android
- [ ] Play Store deployment, internal testing tracks

---

## 🧮 DSA Topic Checklist (Full Reference)

| Topic                     | Status |
| ------------------------- | ------ |
| Arrays & Strings          | ⬜      |
| Linked Lists              | ⬜      |
| Stacks & Queues           | ⬜      |
| Hashing                   | ⬜      |
| Binary Search             | ⬜      |
| Trees (Binary, BST)       | ⬜      |
| Heaps / Priority Queue    | ⬜      |
| Graphs (BFS, DFS)         | ⬜      |
| Graphs (Dijkstra, MST)    | ⬜      |
| Union-Find                | ⬜      |
| Trie                      | ⬜      |
| Dynamic Programming       | ⬜      |
| Backtracking              | ⬜      |
| Greedy                    | ⬜      |
| Sliding Window            | ⬜      |
| Two Pointers              | ⬜      |
| Sorting                   | ⬜      |
| Bit Manipulation          | ⬜      |
| Math / Number Theory      | ⬜      |
| System Design (LRU, etc.) | ⬜      |

---

## 📖 Recommended LeetCode Lists
- **NeetCode 150** — best curated list for interviews
- **Blind 75** — classic must-do problems
- After each weekly DSA section, solve the matching NeetCode problems

## 🔗 Additional Resources
- Android Developers documentation: https://developer.android.com
- Kotlin Coroutines guide: https://kotlinlang.org/docs/coroutines-guide.html
- NeetCode: https://neetcode.io
- Jaewoong Eum's DoveLetter newsletter
- Android Weekly newsletter

---

*Plan generated based on: Manifest Android Interview (Jaewoong Eum) + Practical Kotlin Deep Dive*  
*Last updated: June 2026*
