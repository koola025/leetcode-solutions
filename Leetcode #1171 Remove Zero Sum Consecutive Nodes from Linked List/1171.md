# 1171. Remove Zero Sum Consecutive Nodes from Linked List
### Approach #1: Prefix Sum with Map
**Intuition**

If a consecutive sequence of nodes n<sub>i</sub>, ..., n<sub>i+k</sub> sums to `0` (n<sub>i</sub> stands for the i<sup>th</sup> node), then the prefix sum of n<sub>i-1</sub> equals to the prefix sum of n<sub>i+k</sub>. (Prefix sum of `i` means the sum from n<sub>1</sub> to n<sub>i</sub>.)

Therefore, if two nodes have the same prefix sum, then there must be a consecutive sequence of nodes that sum up to `0` between them.

**Algorithm**

In order to keep track of the prefix sums that occured and their positions, we use a hashmap that maps each prefix sum to the node with that sum.
1. Create a `map` that maps a integer to a `ListNode`.
2. Add a dummy node in front of the `head` of the linked list. (The first node might be deleted.)
3. Map `0` to the dummy node `dum`. (Prefix sum of dummy node is `0`.)
4. Set current pointer `cur` to `head`.
5. While `cur` is not `NULL`, 
    - Calculate its prefix sum and check if the prefix sum occurred before.
        - If yes, get the node with that prefix sum from the `map` and connect it to the next node of the current node.
        - If no, save the prefix sum and the current node into the `map`.
    - Move the `cur` to the next node.
6. Return the pointer to the next node of `dum`.

```cpp
class Solution {
public:
    ListNode* removeZeroSumSublists(ListNode* head) {
        int sum = 0;
        unordered_map<int, ListNode*> m;

        ListNode* dum = new ListNode(0);
        dum->next = head;
        m[0] = dum;
        
        ListNode* cur = head;
        while(cur) {
            sum += cur->val;
            if (m.find(sum) != m.end()) {
                m[sum]->next = cur->next;
            }
            else {
                m[sum] = cur;
            }
            cur = cur->next;
        }

        return dum->next;
    }
};
```
**Note**
1. In the `C++` code, instead of mapping to `ListNode`, we can map to the pointer of `ListNode`, which will be more space efficient.
2. This algorithm might link some nodes that are already "deleted" with the current node, but it will not affect the result.

**Complexity Analysis**
- Time Complexity: O(N), where N is the number of nodes. Each node is visited once, with operations that take constant time.
- Space Complexity: O(N). At most N prefix sum mappings in the map.
