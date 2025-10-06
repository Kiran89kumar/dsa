# Heap Data Structure Cheat Sheet

## What is a Heap?

- Complete binary tree with a heap property
- Min-heap: parent ≤ children. Max-heap: parent ≥ children
- Great for getting current min or max fast

---

## Heap Properties

### Structural Property
- **Complete Binary Tree**: Every level except the last is completely filled
- **Left-to-Right Filling**: Last level is filled from left to right
- **Height**: Always O(log n) where n is the number of elements

### Ordering Property
- **Min Heap**: `parent ≤ children` (root is minimum)
- **Max Heap**: `parent ≥ children` (root is maximum)

### Array Representation
For a node at index `i`:
- **Parent**: `(i-1)/2`
- **Left Child**: `2*i + 1`
- **Right Child**: `2*i + 2`

---

## Tree Structure

### Visual Representation (Max Heap)
```
        50
       /  \
     30    40
    / \   / \
   20 10 35 25
  /
 15
```

### Array layouts

**Max-heap array (1-based):**
```
Index:  0  1  2  3  4  5  6  7  8
Value:  - 50 30 40 20 10 35 25 15
Parent=i//2, Left=2i, Right=2i+1
```

**Min-heap array (1-based):**
```
Index:  0  1  2  3  4  5  6  7
Value:  -  5 10  8 20 15 12  9
Parent=i//2, Left=2i, Right=2i+1

### Navigation Formulas (Java)
```java
int parent(int i) {
    return (i - 1) / 2;
}

int leftChild(int i) {
    return 2 * i + 1;
}

int rightChild(int i) {
    return 2 * i + 2;
}
```

---

## Heapify Algorithm

**Heapify** is the process of maintaining the heap property after an element is added or removed.

### Types of Heapify:
1. **Heapify Up (Bubble Up)**: Used after insertion
2. **Heapify Down (Bubble Down)**: Used after deletion

### Heapify Down Implementation (Java)
```java
void heapifyDown(int[] arr, int n, int i) {
    int largest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;

    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    if (largest != i) {
        int temp = arr[i];
        arr[i] = arr[largest];
        arr[largest] = temp;
        heapifyDown(arr, n, largest);
    }
}

void heapifyUp(int[] arr, int i) {
    int parent = (i - 1) / 2;
    if (i > 0 && arr[i] > arr[parent]) {
        int temp = arr[i];
        arr[i] = arr[parent];
        arr[parent] = temp;
        heapifyUp(arr, parent);
    }
}
```

### Building a Heap from Array (Java)
```java
void buildHeap(int[] arr) {
    int n = arr.length;
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapifyDown(arr, n, i);
    }
}
```

---

## Common Heap Problems

### Kth Largest Element

**Problem**: Find the kth largest element in an array.

**Approach**: Use a min heap of size k to maintain the k largest elements.

```java
import java.util.PriorityQueue;

int findKthLargest(int[] nums, int k) {
    PriorityQueue<Integer> minHeap = new PriorityQueue<>();
    for (int num : nums) {
        minHeap.offer(num);
        if (minHeap.size() > k) {
            minHeap.poll();
        }
    }
    return minHeap.peek();
}
```

### Kth Closest Points to Origin

**Problem**: Find k closest points to the origin (0,0) in 2D plane.

**Approach**: Use max heap to maintain k closest points.

```java
import java.util.*;

class Point {
    int x, y;
    Point(int x, int y) { this.x = x; this.y = y; }
}

List<Point> kClosestPoints(Point[] points, int k) {
    PriorityQueue<Point> maxHeap = new PriorityQueue<>(
        (a, b) -> Integer.compare(b.x * b.x + b.y * b.y, a.x * a.x + a.y * a.y)
    );
    for (Point point : points) {
        maxHeap.offer(point);
        if (maxHeap.size() > k) {
            maxHeap.poll();
        }
    }
    List<Point> result = new ArrayList<>(maxHeap);
    return result;
}
```

### Last Stone Weight

**Problem**: We have a collection of stones with positive weights. Each turn, we choose the two heaviest stones and smash them together. If they're equal, both are destroyed. Otherwise, the lighter stone is destroyed and the heavier stone's weight is reduced by the lighter stone's weight. Find the weight of the last remaining stone.

**Approach**: Use max heap to always get the two heaviest stones.

```java
import java.util.*;

int lastStoneWeight(int[] stones) {
    PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
    for (int stone : stones) maxHeap.offer(stone);
    while (maxHeap.size() > 1) {
        int stone1 = maxHeap.poll();
        int stone2 = maxHeap.poll();
        if (stone1 != stone2) {
            maxHeap.offer(stone1 - stone2);
        }
    }
    return maxHeap.isEmpty() ? 0 : maxHeap.poll();
}
```

---

## Heap Operations Summary

| Operation      | Time Complexity | Description                         |
|----------------|----------------|-------------------------------------|
| Insert         | O(log n)       | Add element and heapify up          |
| Extract Max/Min| O(log n)       | Remove root and heapify down        |
| Peek           | O(1)           | View root without removing          |
| Build Heap     | O(n)           | Convert array to heap               |
| Heapify        | O(log n)       | Maintain heap property              |

## Java PriorityQueue Quick Reference

```java
import java.util.PriorityQueue;

PriorityQueue<Integer> minHeap = new PriorityQueue<>(); // Min Heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder()); // Max Heap

minHeap.offer(item); // Insert
int min = minHeap.peek(); // Peek minimum
int minRemoved = minHeap.poll(); // Remove minimum

maxHeap.offer(item); // Insert
int max = maxHeap.peek(); // Peek maximum
int maxRemoved = maxHeap.poll(); // Remove maximum
```

---

## Key Takeaways

1. **Heap is a complete binary tree** with heap property
2. **Array representation** is space-efficient and cache-friendly
3. **Heapify maintains** the heap property after modifications
4. **Min heap** for finding kth largest (maintain k elements)
5. **Max heap** for finding kth smallest or closest elements
6. **Java PriorityQueue** is a heap implementation
7. **Time complexity** for most operations is O(log n)
8. **Space complexity** is O(n) for storage, O(k) for kth problems
