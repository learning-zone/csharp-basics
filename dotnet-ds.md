# Data Structures and Algorithms in .NET Core

> *Click &#9733; if you like the project. Your contributions are heartily ♡ welcome.*

<br/>

## Table of Contents

* [Introduction](#-introduction)
* [Big O Notation](#-big-o-notation)
* [Arrays](#-arrays)
* [Stacks](#-stacks)
    * Operations on Stacks
        * [PUSH Operation](#-push-operation)
        * [POP Operation](#-pop-operation)
        * [Display Operation](#-display-operation)
        * [isEmpty Operation](#-isempty-operation)
    * Application of Stacks
        * [Recursion](#-recursion)
        * [Reversal of String](#-reversal-of-string)
        * [Checking the Parenthesis Matching](#-checking-the-parenthesis-matching)
        * [Polish Notation of Arithmetic Expressions](#-polish-notation-of-arithmetic-expressions)
        * [Conversion of the Expressions](#-conversion-of-the-expressions)
        * [Evaluation of POSTFIX Expression](#-evaluation-of-postfix-expression)
* [Queues](#-queues)
    * Operations on a Queue
        * [Insertion](#-insertion)
        * [Deletion](#-deletion)
        * [Qempty Operation](#-qempty-operation)
        * [Qfull Operation](#-qfull-operation)
        * [Display Operation](#-display-operation)
    * Types of Queues
        * [Simple Queue](#-simple-queue)
        * [Circular Queue](#-circular-queue)
        * [Priority Queue](#-priority-queue)
        * [Double Ended Queue](#-double-ended-queue)
* [Linked Lists](#-linked-lists)
    * Operations on Linked Lists
        * [Creating a Linked Lists](#-creating-a-lined-lists)
        * [Traversing a Linked Lists](#-creating-a-lined-lists)
        * [Displaying a Linked Lists](#-creating-a-lined-lists)
        * [Length Operation](#-creating-a-lined-lists)
        * [Searching a Linked Lists](#-creating-a-lined-lists)
        * [Insertion into a Linked Lists](#-creating-a-lined-lists)
        * [Deletion Operations on a singly Linked Lists](#-creating-a-lined-lists)
        * [Sorting a Linked Lists](#-creating-a-lined-lists)
        * [Reversing a Linked Lists](#-creating-a-lined-lists)
        * [Merging of two Sorted Lists](#-creating-a-lined-lists)
    * Types of Linked Lists
        * [Singly Linked Lists](#-singly-linked-lists)
        * [Circular Linked Lists](#-circular-singly-linked-lists)
            * [Creation of a CLL](#-creation-of-a-circular-linked-list)
            * [Display operation on a CLL](#-creation-of-a-circular-linked-list)
            * [Insertion into a CLL](#-creation-of-a-circular-linked-list)
            * [Deletion of a node from CLL](#-creation-of-a-circular-linked-list)
            * [Implementation of Stacks using CLL](#-creation-of-a-circular-linked-list)
            * [Implementation of Queue using CLL](#-creation-of-a-circular-linked-list)
        * [Doubly Linked Lists](#-doubly-linked-lists)
            * [Creating a DLL](#-creating-a-dll)
            * [Displaying a DLL](#-displaying-a-dll)
            * [Inserting a node into DLL](#-Inserting-a-node-into-dll)
            * [Deleting a node from DLL](#-deleting-a-node-from-dll)
        * [Header Linked Lists](#-header-linked-lists)
            * [Grounded Header Linked List](#-grounded-header-linked-list)
                * [Creation](#-creation)
                * [Insertion Operation](#-insertion-operation)
                * [Delete Operation](#-delete-operation)
            * [Circular Header Linked List](#-circular-header-linked-list)
                * [Creating a Linked List](#-creating-a-linked-list)
                * [Insertion](#-insertion)
            * Application of Linked Lists
                * [Addition of two Polynomials](#-addition-of-two-polynomials)
                * [Addition of two long positive Integers](#-addition-of-two-long-positive-integers)
    * [Linked List Implementation of Stacks](#-linked-list-implementation-of-stacks)
    * [Linked List Implementation of Queue](#-linked-list-implementation-of-queue)
    * [Linked List Implementation of Priority Queue](#-linked-list-implementation-of-priority-queue)
* [Trees](#-trees)
    * [Binary Trees](#-binary-tree)
        * [Strictly Binary Tree](#-strictly-binary-tree)
        * [Complete Binary Tree](#-complete-binary-tree)
        * [Almost Full Binary Tree](#-almost-full-binary-tree)
    * [Binary Search Trees](#-binary-search-tree)
        * Operations on a Binary Search Tree
            * [Constructing Binary Search Tree Insertion](#-constructing-binary-search-tree)
            * [Searching Operation on a BST](#-searching-operation-on-a-bst)
            * [Deletion operatin on BST](#-deletion-operatin-on-bst)
            * [Traversals](#-traversals)
            * [Finding Maximum value in BST](#-finding-maximum-value-in-bst)
            * [Finding Minimum value in BST](#-finding-minimum-value-in-bst)
    * [Threaded Binary Tree](#threaded-binary-tree)
        * [Right in Threaded Binary Trees](#-right-in-threaded-binary-trees)
        * [Left in Threaded Binary Trees](#-Left-in-threaded-binary-trees)
    * [Creation of BST from preorder and inorder traversals](#-creation-of-bst-from-preorder-and-inorder-traversals)
    * [Creation of BST from postorder and inorder traversals](#-creation-of-bst-from-postorder-and-inorder-traversals)
    * [AVL Trees](#-avl-tree)
    * [B- Trees](#-b-tree)
    * [B+ Trees](#-b-tree)
    * [Red-Black Trees](#-red-black-tree)
* [Graph](#-graph)
    * [Breadth First Search Algorithm](#-breadth-first-search-algorithm)
    * [Depth First Search Algorithm](#-depth-first-search-algorithm)
    * [Kruskal\'s Algorithm](#-Kruskals-algorithm)
    * [Dijkstra Algorithm](#-dijkstra-algorithm)
    * [Prim\'s Algorithm](#-prims-algorithm)
    * [Travelling Salesman Problem](#-travelling-salesman-problem)
    * [Floyd-Warshall Algorithm](#-floyd-warshall-algorithm)
* [Sorting](#-sorting-algorithms)
    * [Address Calculation Sort](#-address-calculation-sort)
    * [Binary Tree Sort](#-binary-tree-sort)
    * [Bubble Sort](#-bubble-sort)
    * [Bucket Sort](#-bucket-sort)
    * [Heap Sort](#-heap-sort)
    * [Insertion Sort](#-insertion-sort)
    * [Merge Sort](#-merge-sort)
    * [Quick Sort](#-quick-sort)
    * [Radix Sort](#-radix-sort)
    * [Selection Sort](#-selection-sort)
    * [Shell Sort](#-shell-sort)
* [Searching](#-searching-algorithms)
    * [Linear Search](#-linear-search)
    * [Binary Search](#-binary-search)
    * [Indexed Sequential Search](#-indexed-sequential-search)
* [Hash Table](#-hash-table)
* [Heap](#-heap)
* [Miscellaneous](#-miscellaneous)

<br/>

## # 1. INTRODUCTION

<br/>

## Q. What built-in data structures does .NET Core provide?

.NET Core ships a rich set of data structures in `System.Collections.Generic` and `System.Collections.Concurrent`.

| Data Structure         | Class / Type                         | Namespace                        |
|------------------------|--------------------------------------|----------------------------------|
| Dynamic array          | `List<T>`                            | System.Collections.Generic       |
| Stack                  | `Stack<T>`                           | System.Collections.Generic       |
| Queue                  | `Queue<T>`                           | System.Collections.Generic       |
| Double-ended queue     | `LinkedList<T>`                      | System.Collections.Generic       |
| Dictionary (hash map)  | `Dictionary<TKey, TValue>`           | System.Collections.Generic       |
| Sorted dictionary      | `SortedDictionary<TKey, TValue>`     | System.Collections.Generic       |
| Hash set               | `HashSet<T>`                         | System.Collections.Generic       |
| Sorted set             | `SortedSet<T>`                       | System.Collections.Generic       |
| Priority queue         | `PriorityQueue<TElement, TPriority>` | System.Collections.Generic       |
| Immutable list         | `ImmutableList<T>`                   | System.Collections.Immutable     |
| Thread-safe dictionary | `ConcurrentDictionary<TKey, TValue>` | System.Collections.Concurrent    |
| Thread-safe queue      | `ConcurrentQueue<T>`                 | System.Collections.Concurrent    |


<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 2. BIG O NOTATION

<br/>

## Q. What is Big O Notation? What are the common complexities?

Big O Notation describes the **upper bound** of an algorithm\'s time or space complexity as the input size `n` grows — it answers "how does performance scale?"

| Notation       | Name         | Example                                      |
|----------------|--------------|----------------------------------------------|
| O(1)           | Constant     | Array index access, Dictionary lookup        |
| O(log n)       | Logarithmic  | Binary search, balanced BST operations       |
| O(n)           | Linear       | Linear search, array traversal               |
| O(n log n)     | Linearithmic | Merge sort, Quick sort (average)             |
| O(n²)          | Quadratic    | Bubble sort, insertion sort, nested loops    |
| O(2ⁿ)          | Exponential  | Recursive Fibonacci, power set               |
| O(n!)          | Factorial    | Travelling Salesman (brute force)            |

```csharp
// O(1) — constant time
int GetFirst(int[] arr) => arr[0];

// O(n) — linear time
int LinearSearch(int[] arr, int target)
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i] == target) return i;
    return -1;
}

// O(n²) — quadratic time (nested loops)
void PrintPairs(int[] arr)
{
    for (int i = 0; i < arr.Length; i++)
        for (int j = 0; j < arr.Length; j++)
            Console.WriteLine($"({arr[i]}, {arr[j]})");
}

// O(log n) — logarithmic time
int BinarySearch(int[] arr, int target)
{
    int left = 0, right = arr.Length - 1;
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        if (arr[mid] < target)  left = mid + 1;
        else                    right = mid - 1;
    }
    return -1;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is the difference between time complexity and space complexity?

| Aspect           | Time Complexity                         | Space Complexity                              |
|------------------|-----------------------------------------|-----------------------------------------------|
| Measures         | Number of operations as input grows     | Amount of memory used as input grows          |
| Example O(n)     | Loop over n elements                    | Allocating an array of size n                 |
| Trade-off        | Cache results to reduce time (uses more space) | Compute on-the-fly to reduce space (uses more time) |

```csharp
// O(n) time, O(1) space — sum without extra allocation
long SumArray(int[] arr)
{
    long sum = 0;
    foreach (var n in arr) sum += n;
    return sum;
}

// O(n) time, O(n) space — store prefix sums
long[] PrefixSums(int[] arr)
{
    var prefix = new long[arr.Length];
    prefix[0] = arr[0];
    for (int i = 1; i < arr.Length; i++)
        prefix[i] = prefix[i - 1] + arr[i];
    return prefix;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 3. ARRAYS

<br/>

## Q. What is an array? What are the common array operations in .NET Core?

An array is a **fixed-size**, **contiguous** block of memory holding elements of the same type. Index access is O(1); insertion/deletion (except at end) is O(n) due to shifting.

```csharp
// Declaration and initialization
int[] numbers = new int[5];
int[] primes  = { 2, 3, 5, 7, 11 };

// Multi-dimensional arrays
int[,] matrix = new int[3, 3];
matrix[0, 0]  = 1;

// Jagged arrays
int[][] jagged = new int[3][];
jagged[0] = new int[] { 1, 2, 3 };
jagged[1] = new int[] { 4, 5 };

// Common operations
Array.Sort(primes);                                   // O(n log n)
int idx = Array.BinarySearch(primes, 5);              // O(log n)
Array.Reverse(primes);                                // O(n)
int[] copy = (int[])primes.Clone();

// LINQ on arrays
var evens = primes.Where(x => x % 2 == 0).ToArray();
int sum   = primes.Sum();
int max   = primes.Max();
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you find the two numbers in an array that sum to a target? (Two Sum problem)

```csharp
// Brute force — O(n²) time, O(1) space
public static int[] TwoSumBrute(int[] nums, int target)
{
    for (int i = 0; i < nums.Length; i++)
        for (int j = i + 1; j < nums.Length; j++)
            if (nums[i] + nums[j] == target)
                return new[] { i, j };
    return Array.Empty<int>();
}

// Hash map — O(n) time, O(n) space
public static int[] TwoSumOptimal(int[] nums, int target)
{
    var map = new Dictionary<int, int>(); // value → index

    for (int i = 0; i < nums.Length; i++)
    {
        int complement = target - nums[i];
        if (map.TryGetValue(complement, out int j))
            return new[] { j, i };
        map[nums[i]] = i;
    }
    return Array.Empty<int>();
}

// Usage
int[] result = TwoSumOptimal(new[] { 2, 7, 11, 15 }, 9); // [0, 1]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you find the maximum subarray sum? (Kadane\'s Algorithm)

```csharp
// Kadane\'s Algorithm — O(n) time, O(1) space
public static int MaxSubarraySum(int[] nums)
{
    int maxSoFar  = nums[0];
    int maxEndHere = nums[0];

    for (int i = 1; i < nums.Length; i++)
    {
        maxEndHere = Math.Max(nums[i], maxEndHere + nums[i]);
        maxSoFar   = Math.Max(maxSoFar, maxEndHere);
    }
    return maxSoFar;
}

// Usage
int result = MaxSubarraySum(new[] { -2, 1, -3, 4, -1, 2, 1, -5, 4 }); // 6 (subarray [4,-1,2,1])
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you rotate an array by k positions?

```csharp
// Reverse-based rotation — O(n) time, O(1) space
public static void RotateArray(int[] nums, int k)
{
    k %= nums.Length;
    Reverse(nums, 0, nums.Length - 1);
    Reverse(nums, 0, k - 1);
    Reverse(nums, k, nums.Length - 1);
}

private static void Reverse(int[] arr, int left, int right)
{
    while (left < right)
        (arr[left++], arr[right--]) = (arr[right], arr[left]); // tuple swap
}

// Usage
int[] arr = { 1, 2, 3, 4, 5, 6, 7 };
RotateArray(arr, 3); // [5, 6, 7, 1, 2, 3, 4]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 4. STACKS

<br/>

## Q. What is a Stack? How does it work in .NET Core?

A **Stack** is a LIFO (Last-In, First-Out) data structure. The last element pushed is the first to be popped.

**Operations:** `Push` O(1), `Pop` O(1), `Peek` O(1), `Count` O(1).

```csharp
var stack = new Stack<int>();

// Push (add to top)
stack.Push(10);
stack.Push(20);
stack.Push(30);

// Peek (view top without removing)
Console.WriteLine(stack.Peek()); // 30

// Pop (remove from top)
Console.WriteLine(stack.Pop()); // 30
Console.WriteLine(stack.Pop()); // 20

// isEmpty check
if (!stack.Any())
    Console.WriteLine("Stack is empty");

Console.WriteLine(stack.Count); // 1

// Iterate (does NOT remove)
foreach (var item in stack)
    Console.WriteLine(item);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a custom generic Stack from scratch.

```csharp
public class MyStack<T>
{
    private T[] _data;
    private int _top = -1;

    public MyStack(int capacity = 16)
    {
        _data = new T[capacity];
    }

    public void Push(T item)
    {
        if (_top == _data.Length - 1)
            Array.Resize(ref _data, _data.Length * 2);
        _data[++_top] = item;
    }

    public T Pop()
    {
        if (IsEmpty) throw new InvalidOperationException("Stack underflow");
        var item = _data[_top];
        _data[_top--] = default!;
        return item;
    }

    public T Peek()
    {
        if (IsEmpty) throw new InvalidOperationException("Stack is empty");
        return _data[_top];
    }

    public bool IsEmpty => _top == -1;
    public int  Count   => _top + 1;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you check balanced parentheses using a Stack?

```csharp
// Use case: compilers, expression validators, JSON/XML parsers
public static bool IsBalanced(string expression)
{
    var stack = new Stack<char>();
    var pairs = new Dictionary<char, char>
    {
        [')'] = '(',
        [']'] = '[',
        ['}'] = '{'
    };

    foreach (char c in expression)
    {
        if (c is '(' or '[' or '{')
        {
            stack.Push(c);
        }
        else if (pairs.TryGetValue(c, out char open))
        {
            if (!stack.Any() || stack.Pop() != open)
                return false;
        }
    }
    return !stack.Any();
}

// Usage
Console.WriteLine(IsBalanced("({[]})"));  // True
Console.WriteLine(IsBalanced("({[)]}"));  // False
Console.WriteLine(IsBalanced("{[}"));     // False
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Evaluate a postfix (RPN) expression using a Stack.

```csharp
// Use case: calculators, expression evaluators, compilers
public static double EvaluatePostfix(string expression)
{
    var stack = new Stack<double>();
    var tokens = expression.Split(' ');

    foreach (var token in tokens)
    {
        if (double.TryParse(token, out double num))
        {
            stack.Push(num);
        }
        else
        {
            double b = stack.Pop();
            double a = stack.Pop();
            stack.Push(token switch
            {
                "+" => a + b,
                "-" => a - b,
                "*" => a * b,
                "/" => a / b,
                _   => throw new ArgumentException($"Unknown operator: {token}")
            });
        }
    }
    return stack.Pop();
}

// Usage: "3 4 + 2 *" = (3+4)*2 = 14
Console.WriteLine(EvaluatePostfix("3 4 + 2 *")); // 14
Console.WriteLine(EvaluatePostfix("5 1 2 + 4 * + 3 -")); // 14
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Reverse a string using a Stack.

```csharp
public static string ReverseString(string input)
{
    var stack = new Stack<char>(input);
    return new string(stack.ToArray()); // Stack<char>(string) pushes chars in order; ToArray pops in reverse
}

// Manual approach
public static string ReverseStringManual(string input)
{
    var stack = new Stack<char>();
    foreach (char c in input) stack.Push(c);

    var sb = new StringBuilder();
    while (stack.Any()) sb.Append(stack.Pop());
    return sb.ToString();
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 5. QUEUES

<br/>

## Q. What is a Queue? How does it work in .NET Core?

A **Queue** is a FIFO (First-In, First-Out) data structure. The first element enqueued is the first to be dequeued.

**Operations:** `Enqueue` O(1), `Dequeue` O(1), `Peek` O(1).

```csharp
var queue = new Queue<string>();

// Enqueue (add to back)
queue.Enqueue("Alice");
queue.Enqueue("Bob");
queue.Enqueue("Charlie");

// Peek (view front without removing)
Console.WriteLine(queue.Peek()); // Alice

// Dequeue (remove from front)
Console.WriteLine(queue.Dequeue()); // Alice
Console.WriteLine(queue.Dequeue()); // Bob

Console.WriteLine(queue.Count); // 1

// Safe dequeue
if (queue.TryDequeue(out string item))
    Console.WriteLine(item); // Charlie
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is a Priority Queue and how do you use it in .NET 6+?

`PriorityQueue<TElement, TPriority>` (added in .NET 6) dequeues the element with the **lowest priority value** first (min-heap by default).

```csharp
// Use case: task scheduling, Dijkstra\'s algorithm, A* pathfinding
var pq = new PriorityQueue<string, int>();

pq.Enqueue("Low priority task",    10);
pq.Enqueue("High priority task",   1);
pq.Enqueue("Medium priority task", 5);
pq.Enqueue("Critical task",        0);

while (pq.TryDequeue(out string task, out int priority))
    Console.WriteLine($"[{priority}] {task}");

// Output:
// [0] Critical task
// [1] High priority task
// [5] Medium priority task
// [10] Low priority task
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a Circular Queue.

```csharp
public class CircularQueue<T>
{
    private readonly T[] _buffer;
    private int _front, _rear, _count;

    public CircularQueue(int capacity)
    {
        _buffer = new T[capacity];
    }

    public bool IsFull  => _count == _buffer.Length;
    public bool IsEmpty => _count == 0;
    public int  Count   => _count;

    public void Enqueue(T item)
    {
        if (IsFull) throw new InvalidOperationException("Queue is full");
        _buffer[_rear] = item;
        _rear          = (_rear + 1) % _buffer.Length;
        _count++;
    }

    public T Dequeue()
    {
        if (IsEmpty) throw new InvalidOperationException("Queue is empty");
        var item = _buffer[_front];
        _buffer[_front] = default!;
        _front          = (_front + 1) % _buffer.Length;
        _count--;
        return item;
    }

    public T Peek() => IsEmpty
        ? throw new InvalidOperationException("Queue is empty")
        : _buffer[_front];
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a Double-Ended Queue (Deque) using LinkedList.

```csharp
// .NET\'s LinkedList<T> is a doubly linked list — perfect for a Deque
public class Deque<T>
{
    private readonly LinkedList<T> _list = new();

    public void AddFront(T item) => _list.AddFirst(item);
    public void AddBack(T item)  => _list.AddLast(item);

    public T RemoveFront()
    {
        if (_list.Count == 0) throw new InvalidOperationException("Deque is empty");
        var val = _list.First!.Value;
        _list.RemoveFirst();
        return val;
    }

    public T RemoveBack()
    {
        if (_list.Count == 0) throw new InvalidOperationException("Deque is empty");
        var val = _list.Last!.Value;
        _list.RemoveLast();
        return val;
    }

    public T PeekFront() => _list.First!.Value;
    public T PeekBack()  => _list.Last!.Value;
    public int Count     => _list.Count;
    public bool IsEmpty  => _list.Count == 0;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 6. LINKED LISTS

<br/>

## Q. What is a Linked List? Compare types.

A **Linked List** is a sequence of nodes where each node holds a value and a pointer to the next (and optionally previous) node. It allows O(1) insertion/deletion at head but O(n) random access.

| Type                | Pointers per node  | Head | Tail | .NET class      |
|---------------------|--------------------|------|------|-----------------|
| Singly Linked List  | Next only          | Yes  | No   | (custom)        |
| Doubly Linked List  | Next + Previous    | Yes  | Yes  | `LinkedList<T>` |
| Circular Linked List| Next (wraps back)  | Yes  | —    | (custom)        |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a Singly Linked List with core operations.

```csharp
public class SinglyNode<T>
{
    public T Value;
    public SinglyNode<T>? Next;
    public SinglyNode(T value) { Value = value; }
}

public class SinglyLinkedList<T>
{
    private SinglyNode<T>? _head;
    public int Count { get; private set; }

    // Insert at head — O(1)
    public void AddFirst(T value)
    {
        var node = new SinglyNode<T>(value) { Next = _head };
        _head = node;
        Count++;
    }

    // Insert at tail — O(n)
    public void AddLast(T value)
    {
        var node = new SinglyNode<T>(value);
        if (_head is null) { _head = node; Count++; return; }

        var current = _head;
        while (current.Next is not null) current = current.Next;
        current.Next = node;
        Count++;
    }

    // Delete by value — O(n)
    public bool Remove(T value)
    {
        if (_head is null) return false;

        if (EqualityComparer<T>.Default.Equals(_head.Value, value))
        {
            _head = _head.Next;
            Count--;
            return true;
        }

        var current = _head;
        while (current.Next is not null)
        {
            if (EqualityComparer<T>.Default.Equals(current.Next.Value, value))
            {
                current.Next = current.Next.Next;
                Count--;
                return true;
            }
            current = current.Next;
        }
        return false;
    }

    // Reverse in-place — O(n)
    public void Reverse()
    {
        SinglyNode<T>? prev = null, current = _head;
        while (current is not null)
        {
            var next    = current.Next;
            current.Next = prev;
            prev         = current;
            current      = next;
        }
        _head = prev;
    }

    // Detect cycle — Floyd\'s algorithm O(n)
    public bool HasCycle()
    {
        var slow = _head;
        var fast = _head;
        while (fast?.Next is not null)
        {
            slow = slow!.Next;
            fast = fast.Next.Next;
            if (slow == fast) return true;
        }
        return false;
    }

    public void Display()
    {
        var current = _head;
        while (current is not null)
        {
            Console.Write($"{current.Value} → ");
            current = current.Next;
        }
        Console.WriteLine("null");
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you use .NET\'s built-in `LinkedList<T>`?

```csharp
var list = new LinkedList<int>();

// Add operations
list.AddFirst(1);
list.AddLast(2);
list.AddLast(3);
var node = list.Find(2)!;
list.AddAfter(node, 99);   // 1 → 2 → 99 → 3
list.AddBefore(node, 0);   // 1 → 0 → 2 → 99 → 3

// Traversal
foreach (var item in list)
    Console.Write($"{item} ");

// Remove
list.Remove(99);
list.RemoveFirst();
list.RemoveLast();

// Access ends
Console.WriteLine(list.First?.Value); // head
Console.WriteLine(list.Last?.Value);  // tail
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Find the middle node of a Linked List in one pass.

```csharp
// Floyd\'s slow/fast pointer — O(n) time, O(1) space
public static SinglyNode<T>? FindMiddle<T>(SinglyNode<T>? head)
{
    var slow = head;
    var fast = head;

    while (fast?.Next is not null)
    {
        slow = slow!.Next;
        fast = fast.Next.Next;
    }
    return slow; // When fast reaches end, slow is at middle
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Merge two sorted linked lists.

```csharp
public static SinglyNode<int>? MergeSorted(SinglyNode<int>? l1, SinglyNode<int>? l2)
{
    // Dummy head simplifies edge cases
    var dummy   = new SinglyNode<int>(0);
    var current = dummy;

    while (l1 is not null && l2 is not null)
    {
        if (l1.Value <= l2.Value)
        {
            current.Next = l1;
            l1           = l1.Next;
        }
        else
        {
            current.Next = l2;
            l2           = l2.Next;
        }
        current = current.Next;
    }

    current.Next = l1 ?? l2; // Attach remaining nodes
    return dummy.Next;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 7. TREES

<br/>

## Q. What is a Binary Search Tree (BST)? Implement core operations.

A **BST** is a binary tree where for each node: all left descendants are smaller and all right descendants are larger.

**Operations:** Search O(log n) avg / O(n) worst, Insert O(log n) avg, Delete O(log n) avg.

```csharp
public class BstNode
{
    public int Value;
    public BstNode? Left, Right;
    public BstNode(int value) { Value = value; }
}

public class BinarySearchTree
{
    private BstNode? _root;

    // Insert — O(log n) average
    public void Insert(int value) => _root = InsertRec(_root, value);

    private BstNode InsertRec(BstNode? node, int value)
    {
        if (node is null) return new BstNode(value);
        if (value < node.Value) node.Left  = InsertRec(node.Left, value);
        else if (value > node.Value) node.Right = InsertRec(node.Right, value);
        return node;
    }

    // Search — O(log n) average
    public bool Contains(int value) => ContainsRec(_root, value);

    private bool ContainsRec(BstNode? node, int value)
    {
        if (node is null) return false;
        if (value == node.Value) return true;
        return value < node.Value
            ? ContainsRec(node.Left, value)
            : ContainsRec(node.Right, value);
    }

    // In-order traversal (Left → Root → Right) — gives sorted output
    public IEnumerable<int> InOrder()
    {
        var result = new List<int>();
        InOrderRec(_root, result);
        return result;
    }

    private void InOrderRec(BstNode? node, List<int> result)
    {
        if (node is null) return;
        InOrderRec(node.Left, result);
        result.Add(node.Value);
        InOrderRec(node.Right, result);
    }

    // Pre-order: Root → Left → Right (useful for tree serialization)
    public IEnumerable<int> PreOrder()
    {
        var result = new List<int>();
        PreOrderRec(_root, result);
        return result;
    }

    private void PreOrderRec(BstNode? node, List<int> result)
    {
        if (node is null) return;
        result.Add(node.Value);
        PreOrderRec(node.Left, result);
        PreOrderRec(node.Right, result);
    }

    // Post-order: Left → Right → Root (useful for tree deletion)
    public IEnumerable<int> PostOrder()
    {
        var result = new List<int>();
        PostOrderRec(_root, result);
        return result;
    }

    private void PostOrderRec(BstNode? node, List<int> result)
    {
        if (node is null) return;
        PostOrderRec(node.Left, result);
        PostOrderRec(node.Right, result);
        result.Add(node.Value);
    }

    // Find min/max
    public int FindMin() => FindMinNode(_root)?.Value ?? throw new InvalidOperationException("Empty tree");
    public int FindMax()
    {
        if (_root is null) throw new InvalidOperationException("Empty tree");
        var node = _root;
        while (node.Right is not null) node = node.Right;
        return node.Value;
    }

    private BstNode? FindMinNode(BstNode? node)
    {
        while (node?.Left is not null) node = node.Left;
        return node;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Level-Order (BFS) traversal of a Binary Tree.

```csharp
// Use case: find shortest path, print tree level by level
public static IEnumerable<IEnumerable<int>> LevelOrder(BstNode? root)
{
    var result = new List<List<int>>();
    if (root is null) return result;

    var queue = new Queue<BstNode>();
    queue.Enqueue(root);

    while (queue.Count > 0)
    {
        int levelSize  = queue.Count;
        var levelNodes = new List<int>();

        for (int i = 0; i < levelSize; i++)
        {
            var node = queue.Dequeue();
            levelNodes.Add(node.Value);
            if (node.Left  is not null) queue.Enqueue(node.Left);
            if (node.Right is not null) queue.Enqueue(node.Right);
        }
        result.Add(levelNodes);
    }
    return result;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is an AVL Tree? How does it self-balance?

An AVL Tree is a self-balancing BST where the height difference (balance factor) between left and right subtrees of any node is at most 1. Rotations restore balance after insertions/deletions.

```csharp
public class AvlNode
{
    public int Value, Height;
    public AvlNode? Left, Right;
    public AvlNode(int value) { Value = value; Height = 1; }
}

public class AvlTree
{
    private AvlNode? _root;

    private int Height(AvlNode? n) => n?.Height ?? 0;
    private int BalanceFactor(AvlNode? n) => n is null ? 0 : Height(n.Left) - Height(n.Right);

    private void UpdateHeight(AvlNode n)
        => n.Height = 1 + Math.Max(Height(n.Left), Height(n.Right));

    // Right rotation
    private AvlNode RotateRight(AvlNode y)
    {
        var x  = y.Left!;
        var T2 = x.Right;
        x.Right = y;
        y.Left  = T2;
        UpdateHeight(y);
        UpdateHeight(x);
        return x;
    }

    // Left rotation
    private AvlNode RotateLeft(AvlNode x)
    {
        var y  = x.Right!;
        var T2 = y.Left;
        y.Left  = x;
        x.Right = T2;
        UpdateHeight(x);
        UpdateHeight(y);
        return y;
    }

    public void Insert(int value) => _root = InsertRec(_root, value);

    private AvlNode InsertRec(AvlNode? node, int value)
    {
        if (node is null) return new AvlNode(value);

        if (value < node.Value)      node.Left  = InsertRec(node.Left,  value);
        else if (value > node.Value) node.Right = InsertRec(node.Right, value);
        else return node; // duplicate

        UpdateHeight(node);
        int balance = BalanceFactor(node);

        // Left-Left case
        if (balance > 1 && value < node.Left!.Value)
            return RotateRight(node);

        // Right-Right case
        if (balance < -1 && value > node.Right!.Value)
            return RotateLeft(node);

        // Left-Right case
        if (balance > 1 && value > node.Left!.Value)
        {
            node.Left = RotateLeft(node.Left);
            return RotateRight(node);
        }

        // Right-Left case
        if (balance < -1 && value < node.Right!.Value)
        {
            node.Right = RotateRight(node.Right);
            return RotateLeft(node);
        }

        return node;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 8. GRAPH

<br/>

## Q. What is a Graph? How do you represent it in .NET Core?

A **Graph** G = (V, E) consists of vertices (nodes) and edges (connections). Edges can be directed/undirected and weighted/unweighted.

**Representations:**

```csharp
// Adjacency List (space-efficient for sparse graphs)
public class Graph
{
    private readonly int _vertices;
    private readonly List<int>[] _adjacencyList;

    public Graph(int vertices)
    {
        _vertices      = vertices;
        _adjacencyList = new List<int>[vertices];
        for (int i = 0; i < vertices; i++)
            _adjacencyList[i] = new List<int>();
    }

    public void AddEdge(int u, int v)
    {
        _adjacencyList[u].Add(v);
        _adjacencyList[v].Add(u); // For undirected graph
    }

    // Adjacency Matrix alternative (space-intensive but O(1) edge lookup)
    // bool[,] matrix = new bool[vertices, vertices];
    // matrix[u, v] = matrix[v, u] = true;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Breadth-First Search (BFS) on a Graph.

```csharp
// Use case: shortest path in unweighted graph, level-order traversal, web crawler
public List<int> BFS(int start)
{
    var visited = new bool[_vertices];
    var queue   = new Queue<int>();
    var result  = new List<int>();

    visited[start] = true;
    queue.Enqueue(start);

    while (queue.Count > 0)
    {
        int vertex = queue.Dequeue();
        result.Add(vertex);

        foreach (int neighbor in _adjacencyList[vertex])
        {
            if (!visited[neighbor])
            {
                visited[neighbor] = true;
                queue.Enqueue(neighbor);
            }
        }
    }
    return result;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Depth-First Search (DFS) on a Graph.

```csharp
// Use case: cycle detection, topological sort, maze solving, connected components
public List<int> DFS(int start)
{
    var visited = new bool[_vertices];
    var result  = new List<int>();
    DFSRec(start, visited, result);
    return result;
}

private void DFSRec(int vertex, bool[] visited, List<int> result)
{
    visited[vertex] = true;
    result.Add(vertex);

    foreach (int neighbor in _adjacencyList[vertex])
        if (!visited[neighbor])
            DFSRec(neighbor, visited, result);
}

// Iterative DFS using Stack
public List<int> DFSIterative(int start)
{
    var visited = new bool[_vertices];
    var stack   = new Stack<int>();
    var result  = new List<int>();

    stack.Push(start);

    while (stack.Count > 0)
    {
        int vertex = stack.Pop();
        if (visited[vertex]) continue;
        visited[vertex] = true;
        result.Add(vertex);

        foreach (int neighbor in _adjacencyList[vertex])
            if (!visited[neighbor])
                stack.Push(neighbor);
    }
    return result;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Dijkstra\'s Shortest Path Algorithm.

```csharp
// Use case: GPS navigation, network routing, game pathfinding
public static int[] Dijkstra(int[,] graph, int source)
{
    int n        = graph.GetLength(0);
    int[] dist   = Enumerable.Repeat(int.MaxValue, n).ToArray();
    bool[] visited = new bool[n];
    dist[source] = 0;

    // Use PriorityQueue for O((V+E) log V)
    var pq = new PriorityQueue<int, int>();
    pq.Enqueue(source, 0);

    while (pq.Count > 0)
    {
        int u = pq.Dequeue();
        if (visited[u]) continue;
        visited[u] = true;

        for (int v = 0; v < n; v++)
        {
            if (graph[u, v] > 0 && !visited[v])
            {
                int newDist = dist[u] + graph[u, v];
                if (newDist < dist[v])
                {
                    dist[v] = newDist;
                    pq.Enqueue(v, newDist);
                }
            }
        }
    }
    return dist;
}

// Usage
int[,] adjacencyMatrix = {
    { 0, 4, 0, 0, 8 },
    { 4, 0, 8, 0, 0 },
    { 0, 8, 0, 7, 0 },
    { 0, 0, 7, 0, 9 },
    { 8, 0, 0, 9, 0 }
};

int[] distances = Dijkstra(adjacencyMatrix, 0);
// distances[i] = shortest distance from vertex 0 to vertex i
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Kruskal\'s Minimum Spanning Tree algorithm.

```csharp
// Use case: network design (minimum cable to connect all nodes), clustering
public static List<(int, int, int)> Kruskal(int vertices, List<(int u, int v, int weight)> edges)
{
    // Sort edges by weight
    edges.Sort((a, b) => a.weight.CompareTo(b.weight));

    int[] parent = Enumerable.Range(0, vertices).ToArray();
    int[] rank   = new int[vertices];

    int Find(int x) => parent[x] == x ? x : parent[x] = Find(parent[x]); // path compression

    bool Union(int x, int y)
    {
        int px = Find(x), py = Find(y);
        if (px == py) return false; // cycle detected
        if (rank[px] < rank[py]) parent[px] = py;
        else if (rank[px] > rank[py]) parent[py] = px;
        else { parent[py] = px; rank[px]++; }
        return true;
    }

    var mst = new List<(int, int, int)>();

    foreach (var (u, v, weight) in edges)
    {
        if (Union(u, v))
        {
            mst.Add((u, v, weight));
            if (mst.Count == vertices - 1) break;
        }
    }
    return mst;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 9. SORTING ALGORITHMS

<br/>

## Q. Compare common sorting algorithms and their complexities.

| Algorithm      | Best       | Average    | Worst      | Space  | Stable |
|----------------|------------|------------|------------|--------|--------|
| Bubble Sort    | O(n)       | O(n²)      | O(n²)      | O(1)   | Yes    |
| Insertion Sort | O(n)       | O(n²)      | O(n²)      | O(1)   | Yes    |
| Selection Sort | O(n²)      | O(n²)      | O(n²)      | O(1)   | No     |
| Merge Sort     | O(n log n) | O(n log n) | O(n log n) | O(n)   | Yes    |
| Quick Sort     | O(n log n) | O(n log n) | O(n²)      | O(log n)| No   |
| Heap Sort      | O(n log n) | O(n log n) | O(n log n) | O(1)   | No     |
| Radix Sort     | O(nk)      | O(nk)      | O(nk)      | O(n+k) | Yes    |
| Bucket Sort    | O(n+k)     | O(n+k)     | O(n²)      | O(n+k) | Yes    |

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Bubble Sort.

```csharp
// Use case: educational, nearly sorted small datasets, simple embedded systems
public static void BubbleSort(int[] arr)
{
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++)
    {
        bool swapped = false;
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                (arr[j], arr[j + 1]) = (arr[j + 1], arr[j]);
                swapped = true;
            }
        }
        if (!swapped) break; // Early exit if already sorted — O(n) best case
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Merge Sort.

```csharp
// Use case: sorting linked lists, external sorting, stable sort requirement
public static void MergeSort(int[] arr, int left, int right)
{
    if (left >= right) return;

    int mid = left + (right - left) / 2;
    MergeSort(arr, left, mid);
    MergeSort(arr, mid + 1, right);
    Merge(arr, left, mid, right);
}

private static void Merge(int[] arr, int left, int mid, int right)
{
    int n1 = mid - left + 1;
    int n2 = right - mid;

    int[] L = new int[n1];
    int[] R = new int[n2];

    Array.Copy(arr, left, L, 0, n1);
    Array.Copy(arr, mid + 1, R, 0, n2);

    int i = 0, j = 0, k = left;
    while (i < n1 && j < n2)
        arr[k++] = L[i] <= R[j] ? L[i++] : R[j++];

    while (i < n1) arr[k++] = L[i++];
    while (j < n2) arr[k++] = R[j++];
}

// Usage
int[] arr = { 64, 34, 25, 12, 22, 11, 90 };
MergeSort(arr, 0, arr.Length - 1);
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Quick Sort.

```csharp
// Use case: general-purpose sorting, in-memory datasets, fastest average case
public static void QuickSort(int[] arr, int low, int high)
{
    if (low < high)
    {
        int pivotIndex = Partition(arr, low, high);
        QuickSort(arr, low, pivotIndex - 1);
        QuickSort(arr, pivotIndex + 1, high);
    }
}

private static int Partition(int[] arr, int low, int high)
{
    // Median-of-three pivot to avoid O(n²) on sorted arrays
    int mid = low + (high - low) / 2;
    if (arr[mid] < arr[low])  (arr[mid], arr[low])  = (arr[low], arr[mid]);
    if (arr[high] < arr[low]) (arr[high], arr[low])  = (arr[low], arr[high]);
    if (arr[mid] < arr[high]) (arr[mid], arr[high])  = (arr[high], arr[mid]);

    int pivot = arr[high];
    int i     = low - 1;

    for (int j = low; j < high; j++)
    {
        if (arr[j] <= pivot)
        {
            i++;
            (arr[i], arr[j]) = (arr[j], arr[i]);
        }
    }
    (arr[i + 1], arr[high]) = (arr[high], arr[i + 1]);
    return i + 1;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Heap Sort.

```csharp
// Use case: guaranteed O(n log n), in-place sort, no extra memory
public static void HeapSort(int[] arr)
{
    int n = arr.Length;

    // Build max-heap
    for (int i = n / 2 - 1; i >= 0; i--)
        Heapify(arr, n, i);

    // Extract elements from heap one by one
    for (int i = n - 1; i > 0; i--)
    {
        (arr[0], arr[i]) = (arr[i], arr[0]); // Move current root to end
        Heapify(arr, i, 0);
    }
}

private static void Heapify(int[] arr, int n, int i)
{
    int largest = i;
    int left    = 2 * i + 1;
    int right   = 2 * i + 2;

    if (left  < n && arr[left]  > arr[largest]) largest = left;
    if (right < n && arr[right] > arr[largest]) largest = right;

    if (largest != i)
    {
        (arr[i], arr[largest]) = (arr[largest], arr[i]);
        Heapify(arr, n, largest);
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Insertion Sort.

```csharp
// Use case: small arrays, nearly sorted data, online sorting (elements arrive one at a time)
public static void InsertionSort(int[] arr)
{
    for (int i = 1; i < arr.Length; i++)
    {
        int key = arr[i];
        int j   = i - 1;
        while (j >= 0 && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key;
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Radix Sort.

```csharp
// Use case: sorting integers/strings with bounded range, O(nk) time
public static void RadixSort(int[] arr)
{
    int max = arr.Max();

    for (int exp = 1; max / exp > 0; exp *= 10)
        CountingSortByDigit(arr, exp);
}

private static void CountingSortByDigit(int[] arr, int exp)
{
    int n       = arr.Length;
    int[] output = new int[n];
    int[] count  = new int[10];

    foreach (int num in arr)
        count[(num / exp) % 10]++;

    for (int i = 1; i < 10; i++)
        count[i] += count[i - 1];

    for (int i = n - 1; i >= 0; i--)
    {
        int digit = (arr[i] / exp) % 10;
        output[--count[digit]] = arr[i];
    }

    Array.Copy(output, arr, n);
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 10. SEARCHING ALGORITHMS

<br/>

## Q. Implement Linear Search.

```csharp
// O(n) time — works on unsorted data
public static int LinearSearch<T>(T[] arr, T target) where T : IEquatable<T>
{
    for (int i = 0; i < arr.Length; i++)
        if (arr[i].Equals(target)) return i;
    return -1; // Not found
}

// Use case: small/unsorted datasets, searching linked lists, finding first occurrence
int idx = LinearSearch(new[] { 5, 3, 8, 1, 9 }, 8); // returns 2
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Binary Search (iterative and recursive).

```csharp
// Prerequisite: array must be sorted
// O(log n) time, O(1) space

// Iterative
public static int BinarySearchIterative(int[] arr, int target)
{
    int left = 0, right = arr.Length - 1;

    while (left <= right)
    {
        int mid = left + (right - left) / 2; // avoids integer overflow vs (left+right)/2

        if (arr[mid] == target) return mid;
        if (arr[mid] < target)  left  = mid + 1;
        else                    right = mid - 1;
    }
    return -1;
}

// Recursive
public static int BinarySearchRecursive(int[] arr, int target, int left, int right)
{
    if (left > right) return -1;

    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    if (arr[mid] < target)  return BinarySearchRecursive(arr, target, mid + 1, right);
    return BinarySearchRecursive(arr, target, left, mid - 1);
}

// .NET built-in
int[] sorted = { 1, 3, 5, 7, 9, 11 };
int index = Array.BinarySearch(sorted, 7); // returns 3

// Use case: phone books, dictionary lookups, finding insertion points
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement Binary Search variants — find first/last occurrence.

```csharp
// Find FIRST occurrence of target in sorted array with duplicates
public static int FindFirst(int[] arr, int target)
{
    int left = 0, right = arr.Length - 1, result = -1;

    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
        {
            result = mid;
            right  = mid - 1; // keep searching left
        }
        else if (arr[mid] < target) left  = mid + 1;
        else                        right = mid - 1;
    }
    return result;
}

// Find LAST occurrence
public static int FindLast(int[] arr, int target)
{
    int left = 0, right = arr.Length - 1, result = -1;

    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target)
        {
            result = mid;
            left   = mid + 1; // keep searching right
        }
        else if (arr[mid] < target) left  = mid + 1;
        else                        right = mid - 1;
    }
    return result;
}

// Count occurrences using both
public static int CountOccurrences(int[] arr, int target)
{
    int first = FindFirst(arr, target);
    if (first == -1) return 0;
    return FindLast(arr, target) - first + 1;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 11. HASH TABLE

<br/>

## Q. What is a Hash Table? How does `Dictionary<TKey, TValue>` work in .NET?

A **Hash Table** maps keys to values using a hash function. Average O(1) for insert, lookup, and delete. Worst case O(n) due to collisions.

**Collision resolution strategies:**
- **Chaining** — each bucket holds a linked list (used by .NET\'s `Dictionary<TKey, TValue>`)
- **Open Addressing** — probe for next empty slot (linear, quadratic, double hashing)

```csharp
// Dictionary<TKey, TValue> — hash table implementation
var dict = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase);

// Add
dict["apple"]  = 5;
dict["banana"] = 3;
dict.Add("cherry", 8);

// Safe get
if (dict.TryGetValue("apple", out int count))
    Console.WriteLine($"apple: {count}");

// Get or add
dict["grape"] = dict.GetValueOrDefault("grape", 0) + 1;

// Remove
dict.Remove("banana");

// Iterate
foreach (var (key, value) in dict)
    Console.WriteLine($"{key}: {value}");

// ContainsKey — O(1)
bool hasApple = dict.ContainsKey("apple");
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Find the first non-repeating character using a Hash Table.

```csharp
// O(n) time, O(1) space (bounded by 26 letters)
public static char FirstNonRepeating(string s)
{
    var freq = new Dictionary<char, int>();

    foreach (char c in s)
        freq[c] = freq.GetValueOrDefault(c, 0) + 1;

    foreach (char c in s)
        if (freq[c] == 1) return c;

    return '\0'; // all characters repeat
}

// Usage
Console.WriteLine(FirstNonRepeating("leetcode")); // 'l'
Console.WriteLine(FirstNonRepeating("aabb"));     // '\0'
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Group anagrams together using a Hash Table.

```csharp
// O(n * k log k) where k = max word length
public static IEnumerable<IGrouping<string, string>> GroupAnagrams(string[] words)
{
    return words.GroupBy(word =>
        new string(word.OrderBy(c => c).ToArray())); // sort each word as key
}

// Manual approach with Dictionary
public static List<List<string>> GroupAnagramsManual(string[] strs)
{
    var map = new Dictionary<string, List<string>>();

    foreach (var s in strs)
    {
        var key = new string(s.OrderBy(c => c).ToArray());
        if (!map.ContainsKey(key)) map[key] = new List<string>();
        map[key].Add(s);
    }

    return map.Values.ToList();
}

// Usage
var groups = GroupAnagramsManual(new[] { "eat", "tea", "tan", "ate", "nat", "bat" });
// [["eat","tea","ate"], ["tan","nat"], ["bat"]]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a HashSet and its common use cases.

```csharp
var set = new HashSet<int>();

// Add — O(1) average
set.Add(1); set.Add(2); set.Add(3); set.Add(2); // duplicates ignored

// Contains — O(1) average
bool has2 = set.Contains(2); // true

// Set operations
var setA = new HashSet<int> { 1, 2, 3, 4 };
var setB = new HashSet<int> { 3, 4, 5, 6 };

var union        = new HashSet<int>(setA); union.UnionWith(setB);        // {1,2,3,4,5,6}
var intersection = new HashSet<int>(setA); intersection.IntersectWith(setB); // {3,4}
var difference   = new HashSet<int>(setA); difference.ExceptWith(setB);  // {1,2}

// Use case: find duplicates in array — O(n)
public static bool ContainsDuplicate(int[] nums)
{
    var seen = new HashSet<int>();
    foreach (int n in nums)
        if (!seen.Add(n)) return true; // Add returns false if already present
    return false;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 12. HEAP

<br/>

## Q. What is a Heap? How do you use it in .NET Core?

A **Heap** is a complete binary tree satisfying the heap property:
- **Max-Heap**: parent ≥ children (root is maximum)
- **Min-Heap**: parent ≤ children (root is minimum)

.NET 6+ provides `PriorityQueue<TElement, TPriority>` (min-heap by default).

```csharp
// Min-Heap via PriorityQueue
var minHeap = new PriorityQueue<int, int>();
minHeap.Enqueue(5,  5);
minHeap.Enqueue(3,  3);
minHeap.Enqueue(10, 10);
minHeap.Enqueue(1,  1);

while (minHeap.Count > 0)
    Console.Write($"{minHeap.Dequeue()} "); // 1 3 5 10

// Max-Heap — negate priority
var maxHeap = new PriorityQueue<int, int>();
maxHeap.Enqueue(5,  -5);
maxHeap.Enqueue(3,  -3);
maxHeap.Enqueue(10, -10);

while (maxHeap.Count > 0)
    Console.Write($"{maxHeap.Dequeue()} "); // 10 5 3
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Find the K largest elements in an array using a Min-Heap.

```csharp
// O(n log k) time — efficient for large n, small k
public static int[] KLargestElements(int[] nums, int k)
{
    var minHeap = new PriorityQueue<int, int>();

    foreach (int num in nums)
    {
        minHeap.Enqueue(num, num);
        if (minHeap.Count > k)
            minHeap.Dequeue(); // remove smallest — maintains k largest
    }

    return Enumerable.Range(0, k)
        .Select(_ => minHeap.Dequeue())
        .OrderByDescending(x => x)
        .ToArray();
}

// Usage
int[] result = KLargestElements(new[] { 3, 2, 1, 5, 6, 4 }, k: 2); // [6, 5]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Find the Kth largest element in an array.

```csharp
// Approach 1: Min-heap of size k — O(n log k)
public static int KthLargest(int[] nums, int k)
{
    var minHeap = new PriorityQueue<int, int>();

    foreach (int num in nums)
    {
        minHeap.Enqueue(num, num);
        if (minHeap.Count > k)
            minHeap.Dequeue();
    }

    return minHeap.Peek(); // root is the Kth largest
}

// Approach 2: Quickselect — O(n) average, O(n²) worst
public static int KthLargestQuickSelect(int[] nums, int k)
{
    int targetIndex = nums.Length - k;
    return QuickSelect(nums, 0, nums.Length - 1, targetIndex);
}

private static int QuickSelect(int[] nums, int low, int high, int k)
{
    int pivotIndex = Partition(nums, low, high);
    if (pivotIndex == k)  return nums[pivotIndex];
    if (pivotIndex < k)   return QuickSelect(nums, pivotIndex + 1, high, k);
    return QuickSelect(nums, low, pivotIndex - 1, k);
}

private static int Partition(int[] nums, int low, int high)
{
    int pivot = nums[high], i = low - 1;
    for (int j = low; j < high; j++)
        if (nums[j] <= pivot) (nums[++i], nums[j]) = (nums[j], nums[i]);
    (nums[i + 1], nums[high]) = (nums[high], nums[i + 1]);
    return i + 1;
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Implement a custom Min-Heap from scratch.

```csharp
public class MinHeap
{
    private readonly List<int> _heap = new();

    public int Count => _heap.Count;
    public int Peek  => _heap.Count > 0 ? _heap[0] : throw new InvalidOperationException("Heap is empty");

    public void Insert(int value)
    {
        _heap.Add(value);
        HeapifyUp(_heap.Count - 1);
    }

    public int ExtractMin()
    {
        if (_heap.Count == 0) throw new InvalidOperationException("Heap is empty");
        int min = _heap[0];
        int last = _heap[_heap.Count - 1];
        _heap.RemoveAt(_heap.Count - 1);

        if (_heap.Count > 0)
        {
            _heap[0] = last;
            HeapifyDown(0);
        }
        return min;
    }

    private void HeapifyUp(int i)
    {
        while (i > 0)
        {
            int parent = (i - 1) / 2;
            if (_heap[parent] <= _heap[i]) break;
            (_heap[parent], _heap[i]) = (_heap[i], _heap[parent]);
            i = parent;
        }
    }

    private void HeapifyDown(int i)
    {
        int n = _heap.Count;
        while (true)
        {
            int smallest = i;
            int left     = 2 * i + 1;
            int right    = 2 * i + 2;

            if (left  < n && _heap[left]  < _heap[smallest]) smallest = left;
            if (right < n && _heap[right] < _heap[smallest]) smallest = right;

            if (smallest == i) break;
            (_heap[i], _heap[smallest]) = (_heap[smallest], _heap[i]);
            i = smallest;
        }
    }
}
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## # 13. MISCELLANEOUS

<br/>

## Q. What is a Trie (Prefix Tree)? Implement insert and search.

A **Trie** is a tree data structure for storing strings where each node represents a character. Efficient for prefix-based lookups.

**Use Cases:** Autocomplete, spell checkers, IP routing, word games.

```csharp
public class TrieNode
{
    public Dictionary<char, TrieNode> Children { get; } = new();
    public bool IsEndOfWord { get; set; }
}

public class Trie
{
    private readonly TrieNode _root = new();

    // Insert — O(m) where m = word length
    public void Insert(string word)
    {
        var node = _root;
        foreach (char c in word)
        {
            if (!node.Children.ContainsKey(c))
                node.Children[c] = new TrieNode();
            node = node.Children[c];
        }
        node.IsEndOfWord = true;
    }

    // Search exact word — O(m)
    public bool Search(string word)
    {
        var node = _root;
        foreach (char c in word)
        {
            if (!node.Children.TryGetValue(c, out node!))
                return false;
        }
        return node.IsEndOfWord;
    }

    // Search by prefix — O(m)
    public bool StartsWith(string prefix)
    {
        var node = _root;
        foreach (char c in prefix)
        {
            if (!node.Children.TryGetValue(c, out node!))
                return false;
        }
        return true;
    }

    // Autocomplete — get all words with prefix
    public IEnumerable<string> GetWordsWithPrefix(string prefix)
    {
        var node = _root;
        foreach (char c in prefix)
        {
            if (!node.Children.TryGetValue(c, out node!))
                return Enumerable.Empty<string>();
        }
        return CollectWords(node, prefix);
    }

    private IEnumerable<string> CollectWords(TrieNode node, string current)
    {
        if (node.IsEndOfWord) yield return current;
        foreach (var (c, child) in node.Children)
            foreach (var word in CollectWords(child, current + c))
                yield return word;
    }
}

// Usage
var trie = new Trie();
trie.Insert("apple");
trie.Insert("app");
trie.Insert("application");
trie.Insert("banana");

Console.WriteLine(trie.Search("app"));          // True
Console.WriteLine(trie.StartsWith("appl"));     // True
var suggestions = trie.GetWordsWithPrefix("app");
// ["app", "apple", "application"]
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What is Dynamic Programming? Solve Fibonacci and 0/1 Knapsack.

**Dynamic Programming (DP)** breaks a problem into overlapping subproblems, solves each once, and stores results (memoization or tabulation).

```csharp
// Fibonacci — top-down with memoization O(n) time, O(n) space
public static long FibMemo(int n, Dictionary<int, long>? memo = null)
{
    memo ??= new Dictionary<int, long>();
    if (n <= 1) return n;
    if (memo.TryGetValue(n, out long cached)) return cached;
    return memo[n] = FibMemo(n - 1, memo) + FibMemo(n - 2, memo);
}

// Fibonacci — bottom-up tabulation O(n) time, O(1) space
public static long FibDP(int n)
{
    if (n <= 1) return n;
    long prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++)
        (prev2, prev1) = (prev1, prev2 + prev1);
    return prev1;
}

// 0/1 Knapsack — O(n*W) time and space
public static int Knapsack(int[] weights, int[] values, int capacity)
{
    int n    = weights.Length;
    int[,] dp = new int[n + 1, capacity + 1];

    for (int i = 1; i <= n; i++)
    {
        for (int w = 0; w <= capacity; w++)
        {
            dp[i, w] = dp[i - 1, w]; // exclude item i
            if (weights[i - 1] <= w)
                dp[i, w] = Math.Max(dp[i, w], dp[i - 1, w - weights[i - 1]] + values[i - 1]);
        }
    }
    return dp[n, capacity];
}

// Usage
int[] weights  = { 1, 3, 4, 5 };
int[] values   = { 1, 4, 5, 7 };
int maxValue   = Knapsack(weights, values, capacity: 7); // 9
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. Solve the Longest Common Subsequence (LCS) problem.

```csharp
// Dynamic Programming — O(m*n) time and space
public static int LCS(string s1, string s2)
{
    int m     = s1.Length, n = s2.Length;
    int[,] dp = new int[m + 1, n + 1];

    for (int i = 1; i <= m; i++)
    {
        for (int j = 1; j <= n; j++)
        {
            if (s1[i - 1] == s2[j - 1])
                dp[i, j] = dp[i - 1, j - 1] + 1;
            else
                dp[i, j] = Math.Max(dp[i - 1, j], dp[i, j - 1]);
        }
    }
    return dp[m, n];
}

// Reconstruct the actual LCS string
public static string GetLCS(string s1, string s2)
{
    int m     = s1.Length, n = s2.Length;
    int[,] dp = new int[m + 1, n + 1];

    for (int i = 1; i <= m; i++)
        for (int j = 1; j <= n; j++)
            dp[i, j] = s1[i - 1] == s2[j - 1]
                ? dp[i - 1, j - 1] + 1
                : Math.Max(dp[i - 1, j], dp[i, j - 1]);

    // Backtrack to find the LCS
    var sb = new StringBuilder();
    int r  = m, c = n;
    while (r > 0 && c > 0)
    {
        if (s1[r - 1] == s2[c - 1]) { sb.Insert(0, s1[r - 1]); r--; c--; }
        else if (dp[r - 1, c] > dp[r, c - 1]) r--;
        else c--;
    }
    return sb.ToString();
}

// Usage
Console.WriteLine(LCS("ABCBDAB", "BDCABA")); // 4
Console.WriteLine(GetLCS("ABCBDAB", "BDCABA")); // "BCBA" or "BDAB"
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. How do you implement a sliding window algorithm?

```csharp
// Use case: max sum subarray of size k, longest substring without repeating chars

// Fixed window — maximum sum of k consecutive elements O(n)
public static int MaxSumSubarray(int[] arr, int k)
{
    if (arr.Length < k) throw new ArgumentException("Array smaller than window");

    int windowSum = arr[..k].Sum();
    int maxSum    = windowSum;

    for (int i = k; i < arr.Length; i++)
    {
        windowSum += arr[i] - arr[i - k]; // slide: add new, remove old
        maxSum     = Math.Max(maxSum, windowSum);
    }
    return maxSum;
}

// Variable window — longest substring without repeating characters O(n)
public static int LengthOfLongestSubstring(string s)
{
    var seen    = new Dictionary<char, int>(); // char → last seen index
    int maxLen  = 0;
    int left    = 0;

    for (int right = 0; right < s.Length; right++)
    {
        if (seen.TryGetValue(s[right], out int lastIdx) && lastIdx >= left)
            left = lastIdx + 1; // shrink window

        seen[s[right]] = right;
        maxLen = Math.Max(maxLen, right - left + 1);
    }
    return maxLen;
}

// Usage
Console.WriteLine(MaxSumSubarray(new[] { 1, 4, 2, 9, 7, 3, 8 }, 3)); // 19 (9+7+3)
Console.WriteLine(LengthOfLongestSubstring("abcabcbb")); // 3 ("abc")
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>

## Q. What are common concurrency-safe data structures in .NET Core?

```csharp
using System.Collections.Concurrent;

// Thread-safe dictionary
var concurrentDict = new ConcurrentDictionary<string, int>();
concurrentDict.AddOrUpdate("key", 1, (k, v) => v + 1);
concurrentDict.GetOrAdd("key2", k => ExpensiveComputation(k));

// Thread-safe queue (producer-consumer)
var concurrentQueue = new ConcurrentQueue<WorkItem>();
concurrentQueue.Enqueue(new WorkItem());
if (concurrentQueue.TryDequeue(out var item))
    Process(item);

// Thread-safe stack
var concurrentStack = new ConcurrentStack<int>();
concurrentStack.Push(42);
if (concurrentStack.TryPop(out int val))
    Console.WriteLine(val);

// BlockingCollection — bounded producer-consumer with blocking
var blockingCollection = new BlockingCollection<int>(boundedCapacity: 100);

// Producer
Task.Run(() =>
{
    for (int i = 0; i < 1000; i++)
        blockingCollection.Add(i); // blocks if full
    blockingCollection.CompleteAdding();
});

// Consumer
foreach (int n in blockingCollection.GetConsumingEnumerable())
    Console.WriteLine(n); // blocks until item available

static int ExpensiveComputation(string k) => k.Length;
static void Process(WorkItem item) { }
public record WorkItem;
```

<div align="right">
    <b><a href="#table-of-contents">↥ back to top</a></b>
</div>
