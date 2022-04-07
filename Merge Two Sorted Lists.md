# Merge Two Sorted Lists

- Problem: https://leetcode.com/problems/merge-two-sorted-lists/
- Level: Easy

### Explanation

There is nothing special about the solution, we check heads of two lists to get the smaller head into the final list consecutively.

Also we can consider using a dummy `ListNode` as a head to make the solution implementation easier.

### Code

There are two versions of the implementation, one is verbose and another is short by using dummy node.

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        # Return list2 if list1 is empty
        if not list1:
            return list2
        
        # Return list1 if list2 is empty
        if not list2:
            return list1
        
        # Find node of the head
        head = None
        if list1.val < list2.val:
            head = list1
            list1 = list1.next
        else:
            head = list2
            list2 = list2.next
        
        # Append to the list in order until at least one of two list is empty
        # Use `node_iter` to keep track of the tail, keep `head` variable to return at the end
        node_iter = head
        while list1 and list2:
            if list1.val < list2.val:
                node_iter.next = list1
                list1 = list1.next
            else:
                node_iter.next = list2
                list2 = list2.next
            node_iter = node_iter.next
        
        # Append remaining nodes of either list1 or list2
        if list1:
            node_iter.next = list1
        if list2:
            node_iter.next = list2
        
        return head
```

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
        head = ListNode()

        # Append to the list in order until at least one of two list is empty
        # Use `node_iter` to keep track of the tail, keep `head` variable to return at the end
        node_iter = head
        while list1 and list2:
            if list1.val < list2.val:
                node_iter.next = list1
                list1 = list1.next
            else:
                node_iter.next = list2
                list2 = list2.next
            node_iter = node_iter.next
        
        # Append remaining nodes of either list1 or list2
        if list1:
            node_iter.next = list1
        if list2:
            node_iter.next = list2
        
        return head.next
```

### Complexity

- Time: O(max(n,m))
- Space: O(1)
