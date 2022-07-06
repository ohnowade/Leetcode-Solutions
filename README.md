# Leetcode-Solutions
## General Rule for Binary Search:  
To proceed in the binary search process, given a search range and the middle value, we decide which half of the range to discard and update the search range to the other half, based on the middle value.

$33$. Search in Rotated Sorted Array  
https://leetcode.com/problems/search-in-rotated-sorted-array/  
An interesting binary search problem. Referring to the general rule for binary search, to decide the proceeding rule, we consider the relationship between the middle value and the lowest value of the range and that between the middle value and the target. If the middle value is larger than the lowest value, the rotation point is not within the first half of the range, and the first half includes one range: [lowest value, middle value]. Thus, if the target is larger than the middle value or less than the lowest value, current half should be discarded. When the middle value is less than the lowest lavue, it means the rotation point is within the range, indicating that current half includes two ranges: [lowest value, maximum value] and [minimum value, middle value]. Hence, we should only keep current half if the target is larger than the lowest value or less than the middle value.


$153$. Find Minimum in Rotated Sorted Array  
https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/  
Similar to question $33$. Referring to the general rule for binary search, to decide the proceeding rule, we consider the relationship between the middle value and the lowest value of current search range. If the middle value is larger, the rotation point is not in the first half, which includes one range: [lowest value, middle value]. This leads to two situation. First, if a rotation point exists, it is also the minimum value and resides in the second half; Else, the lowest value is the minimum value. Then to decide whether there is a rotation, we compare the highest value to the lowest value. If the highest value is larger, there cannot be a rotation; while if it is less, there must be a rotation. Now if the middle value is less than the lowest value, current half includes two ranges: [lowest value, maximum value] and [minimum value, middle value], meaning that the minimum value has to be in current half.


$253$. Meeting Rooms II  
https://leetcode.com/problems/meeting-rooms-ii/  
The core of this problem is to keep track of the earliest end time of currently in-use conference rooms, since if a meeting starts before it, then an additional conference room is necessarily needed. Therefore, we just need to go through all intervals in the ascending order of their start times. If a start time is less than current earliest end time, it means we need an additional room; if the start time is larger, the meeting in the room with earliest end time is already over, current meeting can occupy the room, and we update the earliest end time accordingly. Either a priority queue or an array with a pointer can update the earliest end time correctly. Since sorting is involved, we need $O(N\log N)$ time; since we need to remember end times to update the earliest one, we need $O(N)$.
  
  
$291$. Word Pattern II  
https://leetcode.com/problems/word-pattern-ii/  
This is a straightforward backtracking problem. In each recursion, if current character does not match to any string, match it to every prefix of remaining s and try if such match works, and stop when one works; if current character already matches to a string, check if the string is the following string in s and backtrack if it is not. Suppose the length of pattern is $m$ and that of s is $n$. The search process resembles placing m sticks among n objects, so the time and space complexity is $n\choose m$.
