# Heap Cheat Sheet

## What is a Heap?
A heap is a special tree-based data structure that satisfies the heap property. In a max heap, for any given node I, the value of I is greater than or equal to the values of its children. In a min heap, the value of I is less than or equal to the values of its children.

## Types of Heaps:
1. **Max Heap**
2. **Min Heap**

## Operations:
- **Insert**: Add a new element to the heap.
- **Extract**: Remove the root element from the heap.
- **Heapify**: Rearrange the heap to maintain the heap property.

## Java Examples:

### Max Heap Example
```java
import java.util.PriorityQueue;

public class MaxHeap {
    public static void main(String[] args) {
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);
        maxHeap.add(10);
        maxHeap.add(20);
        maxHeap.add(15);

        System.out.println("Max Heap: " + maxHeap);
        System.out.println("Removed Element: " + maxHeap.poll());
    }
}
```

### Min Heap Example
```java
import java.util.PriorityQueue;

public class MinHeap {
    public static void main(String[] args) {
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        minHeap.add(10);
        minHeap.add(20);
        minHeap.add(15);

        System.out.println("Min Heap: " + minHeap);
        System.out.println("Removed Element: " + minHeap.poll());
    }
}
```
