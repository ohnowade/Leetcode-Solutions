# Leetcode-Solutions
## General Rule for Binary Search:  
To proceed in the binary search process, given a search range and the middle value, we decide which half of the range to discard and update the search range to the other half, based on the middle value. Consider $O(\log N)$ for complexity.
  

$33$. Search in Rotated Sorted Array  
https://leetcode.com/problems/search-in-rotated-sorted-array/  
An interesting binary search problem. Referring to the general rule for binary search, to decide the proceeding rule, we consider the relationship between the middle value and the lowest value of the range and that between the middle value and the target. If the middle value is larger than the lowest value, the rotation point is not within the first half of the range, and the first half includes one range: [lowest value, middle value]. Thus, if the target is larger than the middle value or less than the lowest value, current half should be discarded. When the middle value is less than the lowest lavue, it means the rotation point is within the range, indicating that current half includes two ranges: [lowest value, maximum value] and [minimum value, middle value]. Hence, we should only keep current half if the target is larger than the lowest value or less than the middle value.


$116$. Populating Next Right Pointers in Each Node  
https://leetcode.com/problems/populating-next-right-pointers-in-each-node/  
Two ways to solve the problem:
* DFS: use a list to store all lastly visited node at each level. As we DFS through the tree, if it is the first time we enter current level, the size of the list will be one less than the height of current level, indicating we should add current node to the list; otherwise, we simply retrieve the node stored in the list corresponding to current level, and set its next pointer to current node. The key is that for nodes at the same level, DFS visits them in order.
* While Loop: assume that all nodes at current level are connected. Then we can use a while loop to go through all nodes at current level and connect their children accordingly. Hence, as we enter the next level, all nodes will be connected. Since the first level only has the root node, it has all nodes connected, so the assumption is true (Mathematical Induction).  

Complexity: in both ways we visit each node exactly one, so $O(N)$.


$153$. Find Minimum in Rotated Sorted Array  
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/  
Similar to question $33$. Referring to the general rule for binary search, to decide the proceeding rule, we consider the relationship between the middle value and the lowest value of current search range. If the middle value is larger, the rotation point is not in the first half, which includes one range: [lowest value, middle value]. This leads to two situation. First, if a rotation point exists, it is also the minimum value and resides in the second half; Else, the lowest value is the minimum value. Then to decide whether there is a rotation, we compare the highest value to the lowest value. If the highest value is larger, there cannot be a rotation; while if it is less, there must be a rotation. Now if the middle value is less than the lowest value, current half includes two ranges: [lowest value, maximum value] and [minimum value, middle value], meaning that the minimum value has to be in current half.
  
  
$209$. Minimum Size Subarray Sum  
https://leetcode.com/problems/minimum-size-subarray-sum/  
A sliding window problem. The window maintains the sum of its elements to be $\geq$ the target, and it slides from left to right. Every time an new element, pointed to by the right pointer, is included in the window, we try to shrink the window, by moving the left pointer, as much as possible, to achieve the minimum size for current right pointer. Since we will traverse through the minimum window size for each of the elements as the right pointer, the minimum one among them would be the answer.


$253$. Meeting Rooms II  
https://leetcode.com/problems/meeting-rooms-ii/  
The core of this problem is to keep track of the earliest end time of currently in-use conference rooms, since if a meeting starts before it, then an additional conference room is necessarily needed. Therefore, we just need to go through all intervals in the ascending order of their start times. If a start time is less than current earliest end time, it means we need an additional room; if the start time is larger, the meeting in the room with earliest end time is already over, current meeting can occupy the room, and we update the earliest end time accordingly. Either a priority queue or an array with a pointer can update the earliest end time correctly. Since sorting is involved, we need $O(N\log N)$ time; since we need to remember end times to update the earliest one, we need $O(N)$.
  
  
$291$. Word Pattern II  
https://leetcode.com/problems/word-pattern-ii/  
This is a straightforward backtracking problem. In each recursion, if current character does not match to any string, match it to every prefix of remaining s and try if such match works, and stop when one works; if current character already matches to a string, check if the string is the following string in s and backtrack if it is not. Suppose the length of pattern is $m$ and that of s is $n$. The search process resembles placing m sticks among n objects, so the time and space complexity is $n\choose m$.
  
  
$394$. Decode String  
https://leetcode.com/problems/decode-string/  
The setting of the problem resembles the stack in memory and function calls, which means it could be solved through a ```Stack```. Each pair of square brackets and what it includes correspond to a stack frame, while the number preceding corresponds to the argument to be fetched. We use a ```StringBuilder``` to store the string formed in current state (representing current stack frame). Every time we encounter a number, it is like that we call a function from current stack frame, which means we need to push current state (what the ```StringBuilder``` has stored) as well as the argument (the number preceding the pait of square brackets) to the ```Stack```, and then we enter a new stack frame (re-initializing the ```StringBuilder```). As we encounter a right square bracket, it represents the end of current function, and we need to return the result, which is the repetition of current string by preceding-number times, back to the last stack frame. After we return from the function call, we need to restore current state, by appending the returned string to the previously stored string and setting the appended string as the data in a new ```StringBuilder```. 
  
  
$402$. Remove K Digits  
https://leetcode.com/problems/remove-k-digits/  
One important thing to realize is that we want the smaller digit to be as left as possible. If a digit is smaller than a digit on its left, the left one should be removed to make the resulting number smaller. This means the ideal resulting number should have ascending digits from left to right. As we are processing each digit and try to maintain an ascending order by removing certain digits, a monotonic stack is the best option. As we pushing each digit into the monotonic stack, we only remove at most $k$ elements. After processing all digits, if we need to remove more digits, we should remove from the right, since the digits at the right side of the ascending sequence are the larger ones.  
  
  
$713$. Subarray Product Less Than K  
https://leetcode.com/problems/subarray-product-less-than-k/  
A sliding window problem. To find the total number of such continuous subarrays, we find the number of such subarrays for each of the element as the right pointer of the window, so that the sum of these numbers would be the final answer. Everytime we move the right pointer to the right, we update the left pointer so that the product of elements in the window is less than k. After the left pointer is updated, $right - left + 1$ is the number of such continuous subarrays that end with current right pointer, since it is the number of possible starting elements of a subarray within the range [left, right]. Note that 0 should be directly returned when $k\leq1$, otherwise the left pointer could be larger than the right pointer, and extra cases need to be handled. 
  
  
$2289$. Steps to Make Array Non-decreasing  
https://leetcode.com/problems/steps-to-make-array-non-decreasing/ 
A dynamic programming problem solved with monotonic stack. Suppose that we have a sequence_1 of number starting with $a$, which is also the largest number, and it takes $x$ steps to make sequence_1 non-decreasing (remaining only $a$ itself). Now another sequence_2 that starts with a number $b$, for which $b$ is the largest number in sequence_2, $b > a$, and all other numbers in sequence_2 are less than $a$, is prepended to sequence_1, and it took $y$ steps for sequence_2 to be non-decreasing (remaining $b$ itself). The question now is that how many steps it takes to make the combined sequence_3 non-decreasing (remaining $b$ only). After $y$ steps, the first part of sequence_3 has only $b$ itself, and $a$ is to be removed in the next step, which means it needs at least $y+1$ steps to finish. However, if $x>y+1$, then when $a$ is removed, there are some number belonging to sequence_1 that are not removed yet; but since $b$ > $a$, nothing is different for the remaining numbers, and they will still be removed in $x$ steps. Hence, the total number of steps that sequence_3 needs is $max(y+1, x)$, and for a number at index $i$ being $b$ and a number at index $j$ being $a$, the dynamic programming relationship is $max(dp[i] + 1, dp[j])$. However, how do we map each number to $a$ or $b$? We use a monotonic stack. Traversing from back to front and keeping the stack in an ascending order, then the current traversed number is $b$, while the number whose index is at the top of the stack is $a$, if it is smaller. If the current traversed number is smaller, we can push it to the top of the stack, since we have calculated the "$x$", and it can be $a$ for another number.
