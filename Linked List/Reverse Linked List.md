# 206. Reverse Linked List

## Problem
Reverse a singly linked list.

Example:

Input: 1->2->3->4->5->NULL
Output: 5->4->3->2->1->NULL

## Methods
### 1. Recursive -- T = O(n), S = O(1)
Intuition: 
* Recursively divide the linked list into 1st node and the rest nodes
* Reverse a single link by: node.next.next = node

### Code
```JavaScript
// recursive, elegent
var reverseList = function(head) {
    // basecase
    if (head === null || head.next === null) return head
    let rest = new reverseList(head.next)
    head.next.next = head   // where reverse take place
    head.next = null        // delete the original link
    
    return rest
};
```

### 2. Iterative -- T = O(n), S = O(1)
![Image of Yaktocat](https://github.com/frostace/LeetCode)

Loop until curr points at NULL

  1. 3 pointers: prev, curr, next
    * prev maintains already reversed linked list
    * curr points at current list node
    * next points at next list node
  2. init prev = null, curr = head, next = null
  3. increment next
  4. reverse current link by appending prev to curr
  5. increment prev
  6. increment curr

### Code
```JavaScript
// iterative, elegent
var reverseList = function(curr) {
    let prev = null
    while (curr) {
        const next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    return prev
};
```

### Reference
https://www.geeksforgeeks.org/reverse-a-linked-list/