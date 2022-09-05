# Leetcode-Solutions
## General Rule for Binary Search:  
To proceed in the binary search process, given a search range and the middle value, we decide which half of the range to discard and update the search range to the other half, based on the middle value. Consider $O(\log N)$ for complexity.  
## General Rule for Union-Find:
Initialize each elements with group id equal to itself. For each pair of connected elemtn, use the ```find``` function to trace the final group id for the two elements. If the gorup id are different, set one group id (indexed by that group id) to the other one. The group id should be indexed by the group id itself, instead of the element, the group id indexed by the element might not be the final group id of the group.  
  

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
  

$416$. Partition Equal Subset Sum  
https://leetcode.com/problems/partition-equal-subset-sum/  
A knapsack problem. The peoblem is asking whether we can add a subset of all elements to form the half of their total sum. Then the DP table should be formed as follows. $dp[i][j]$ is whether we can form a sum of $j$ using first $i$ elements. Then the problem comes to, for $i$ and $j$, whether we use the $i_{th}$ element in subset to form a sum of $j$. If we use it, then the value depends on whether we can form a sum of $j - nums[i]$ using the first $i-1$ elements. If we do not use it, the value of $dp[i][j]$ depends on whether we can form a sum of $j$ using the first $i-1$ elements. Hence, the dynammic programming relationship is $dp[i][j] = dp[i-1][j - nums[i]\  ||\ dp[i-1][j]$. 
  
  
$523$. Continuous Subarray Sum  
https://leetcode.com/problems/continuous-subarray-sum/  
Assume we have two numbers $x = n\cdot k + a$, $y = m\cdot k + a$, with the same remainder when divided by $k$, then $x-y=(n-m)\cdot k$, which is a multiple of $k$. This means that as we traverse through the prefix sums of the numbers and keep track of the remainder of current sum divided by $k$, if at any time the remainder has been encountered before, then the subarray between current and last occurrences of the remainder has elements summing up to be a multiple of $k$. Since we want a subarray with size larger than 1, we want to map each remainder to the index at which we run into it. We also need to add entry $(0, -1)$ to the map in case the subarray starts from the beginning of nums.
  
  
$678$. Valid Parenthesis String  
https://leetcode.com/problems/valid-parenthesis-string/  
Using classic two-pass way to check whether the string is valid. When going from left to right, $numRight \leq numLeft + numWild$ at any moment; when going from right to left, $numLeft \leq numRight + numWild$ at any moment. However, it might seem that there could be different assignment for a wildcard between when traversing from left to right and when from right to left. Here is a proof by contradiction to prove it is impossible. Suppose we have a wildcard that was assigned as a left parenthesis when going from left to right and assigned as a right parenthesis when going from right to left. This means that, when going from left to right, there is a right parenthesis at the right of the wildcard that cannot be paired with a left parenthesis. It also means that, when going from right to left, there is a left parenthesis at the left of the wildcard that cannot be paried with a right parenthesis. However, if it is the case, the right parenthesis at the right of the wildcard can be paired with the left parenthesis at the left, thus a contradiction.   
  
  
$713$. Subarray Product Less Than K  
https://leetcode.com/problems/subarray-product-less-than-k/  
A sliding window problem. To find the total number of such continuous subarrays, we find the number of such subarrays for each of the element as the right pointer of the window, so that the sum of these numbers would be the final answer. Everytime we move the right pointer to the right, we update the left pointer so that the product of elements in the window is less than k. After the left pointer is updated, $right - left + 1$ is the number of such continuous subarrays that end with current right pointer, since it is the number of possible starting elements of a subarray within the range [left, right]. Note that 0 should be directly returned when $k\leq1$, otherwise the left pointer could be larger than the right pointer, and extra cases need to be handled. 
  
  
$729$. My Calendar I  
https://leetcode.com/problems/my-calendar-i/  
It appears that we should use binary search to search for insert position (leetcode $35$) of a booking, but the actual insertion is separated from search and requires $O(n)$. Hence, it is better if we just store the bookings in a binary search tree, for which we can insert during search.  
  
  
$739$. Daily Temperatures  
https://leetcode.com/problems/daily-temperatures/  
There are two ways to solve the problem. The first way is to use monotonic stack, and the implementation is straightforward. The second way is to search directly. One thing to realize is that, as we traverse through the array from right to left, marking current index as $i$, then for any $j > i$ and temperature[$j$] <= temperature[$i$], assuming the number of days for temperature[$j$] to wait for a warmer day is $x$, we can just compare if temperature[$j+x$] to temperature[$i$], since the $x$ temperatures with index between $j$ and $j+x$ will be less than or equal to temperature[$i$] and can be ignored. Therefore, we traverse through the temperatures from right to left, and for each element, we compare it to its right neighbor. If its right neighbor is less or equal, the next searching element will be the right neighbor's next warmer temperture, and the number of days between them is counted toward current element's result. However, we also need to remember current highest temperature and update it as we traverse through the array, so that we can avoid the searching if current temperature is higher than the current highest. Also, we can avoid searching all the way to the end of the array.  
The time complexity of the second method is still $O(N)$, because no element will appear in a searching path twice. Assume we have elements x, y, z from left to right, such that $y < x < z$, z is x's next warmer temperature, and $y$ is an element in $x$'s searching path. Suppose that there is an element $t$ at the left of $x$ and assume that $y$ also appears in $t$'s searching path. That means $y$ is the next warmer temperature of an element between $t$ and $x$. However, since $x$ > $y$, if the element is less than $y$ then it is less than $x$, which means $x$ will be its next warmers temperature. Thus, we proved by contradiction that each element will be searched once. If any future search hits $x$, it will then be directed to $z$ and skip all elements between $x$ and $z$, including $y$.  
  
  
$767$. Reorganize String  
https://leetcode.com/problems/reorganize-string/  
First thing to realize is that the string can be rearranged if and only if the maximum number of occurences of a number is less than or equal to half of the length of the string (or half of the length + 1 if the length is odd). Then, to rearrange the string, we insert characters with the order 0, 2, 4, ..., n and then 1, 3, 5, .... However, if the length of the string is odd, and the maximum number of a character is $\frac{length + 1}{2}$, this character has to be inserted from 0. Therefore, the first step of the rearrangement is to insert the maximum character to the position2 0, 2, 4, .... Then we follow the order and insert the remaining characters.  
  
  
$853$. Car Fleet  
https://leetcode.com/problems/car-fleet/  
We need to sort the cars first so that their positions are in descending order, since whether we can combine cars into a fleet depends on their relative positions, speed, and the distance to target. After sorting, one important thing to realize is that, assuming the time it takes for the head of current fleet to reach target is $t$, if a car after it needs less than or equal to $t$ to get to the target, the car will be merged into a fleet with the head. However, the first car after the head that needs more than $t$ to get to the target will never catch the head, and therefore will be the head of a new fleet, and all cars behind the car can never be merged into the former fleet. Hence, as we traverse through the sorted array, we just need to keep track of current head's time to get to target. If a car's time is larger, it will be the head of a new fleet. Note that if we sort the array in descending order, a straighforward stack method might not work. Since we process cars from closest to the target to the furtherest, this is the reverse order through which cars are merged into fleet, and the merging of current cars might stop future cars that should have been merged from being close enough to merge.  
  
  
$856$. Score of Parentheses  
https://leetcode.com/problems/score-of-parentheses/  
The first way to solve it is through stack. We first define two relationships. The first is the value of current pair of parenthesis, $curVal$, and the second is the sum of all parallel pairs directly embraced in a pair of parenthesis, $levelSum$. We can see that the two relationships are transitive - $levelSum$ of current level is the $curVal$ of the parenthesis embracing the pairs. Hence, we can use a stack to store the relationships. The top of the stack is current $curVal$, while the element right below it is current $levelSum$. For every left parenthesis we push a 0, while for every right parenthesis we pop the $curVal$ out and double it (set it to 1 if it is 0), and then we added $curVal$ to $levelSum$.  
The second way to solve it is to realize that only a clean pair of parenthesis (one that does not embrace anything) contributes to the total sum, and they contribute to the total sum by $2^d$, where $d$ is the depth of this clean pair. Hence, we just need to keep track of current depth and only adds to total sum when we run into a clean pair. This method transforms $2\cdot(1 + 2\cdot(1+1+2\cdot2\cdot(1+2\cdot(1+1))+1))$ to $2+2^2+2^2+2^4+2^5+2^5+2^2$.  
  
  
$1353$. Maximum Number of Events That Can Be Attended  
https://leetcode.com/problems/maximum-number-of-events-that-can-be-attended/  
This is a greedy problem. To maximize the number of events, for all the events we can attend in a day, we should always attend the one that ends the earliest. Then, for each of the possible days, we maintain a collection of events that we can attend, by clearing the events that already ended and adding events that start at the day. By using a min-heap to store the end time of all the events, we always attend the one at the top of the heap, if there is one. The heap also faciliate the removal 
of ended events.  
  
  
$1648$. Sell Diminishing-Valued Colored Balls  
https://leetcode.com/problems/sell-diminishing-valued-colored-balls/  
This is a greedy problem. The idea is simple, in that we always sell the ball with the highest inventory. The most straightforward way would be using a heap, but for each of the order it incurs $O(\log N)$ operations. One important thing to notice is that, assuming the highest inventory is $a$ and the next one is $b$, the profit we make for each sale would be $a$, $a-1$, $a-2$,...,$b+1$, $b$, $b$, $b-1$, $b-1$, from which point there are two balls with the same inventory. In fact, as the inventory $b$ becomes equal to the next inventory $c$, there will be three balls with the same inventories. Therefore, we can reduce the computation by keeping track of a coefficient $k$, representing there are $k$ balls now with the same inventories. Assume now the current three highest inventories are $a$, $b$, and $c$ in order, and there are $k$ balls whose inventory is $a$. Suppose the remaining $orders$ is large enough, then in the first round of computation we can process $k\cdot(a - b)$ orders and add $k\cdot\frac{(a + b + 1)\cdot(a - b)}{2}$ to the total sum. In the next round, we will then process $(k+1)\cdot(b-c)$ orders and add $(k+1)\cdot\frac{(b + c + 1)\cdot(b - c)}{2}$ to the total sum. However, if the remaining number of $orders$ is less than a full round, we need to compute how many of the remaining inventories can be sold. Assuming the current highest inventory is $d$, then the basic idea is that we process $k$ orders at a time and add $k\cdot d$, $k\cdot(d-1)$, $k\cdot(d-2)$, ... to the sum until we have processed all orders. Let $q$ be the quotient and $r$ be the remainder of $\frac{orders}{k}$. Then in the final round we can process $k\cdot\frac{(d+d-q+1)\cdot q)}{2}+r\cdot(d-q)$. Since the prerequiste is that $orders$ will be larger than the sum of all inventories, we do not need to consider the case when there are no inventories left.  
  
  
$1776$. Car Fleet II  
https://leetcode.com/problems/car-fleet-ii/  
This problem only asks the time for a car to bump into the first car (or fleet) ahead. One important thing to notice is that, assuming there are three cars $x$, $y$, and $z$ in order from left to right, if $y$ cannot bump into $z$, then $x$ can never bump into $z$. We traverse through the cars from right to left and make use of a stack. Each car will be pushed into stack. However, before it is pushed into stack, check whether the top car, $t$, in the stack can be bumped into by current car, $c$. If $c$ cannot bump into the $t$, or the $t$ will already have bumped into the other car (so its speed will change) before it is bumped into by current car, it should just be discarded. This is because, first, if $t$ will bump into another car, $a$, first, then the collision time of $c$ will be that for it to bump into $a$, and second, any future cars will never bump into $t$. Also, this is a monotonic stack since the collision time is strictly decreasing.  

  
  
$1980$. Find Unique Binary String  
https://leetcode.com/problems/find-unique-binary-string/  
The first solution is to sort the array, traverse it in order, and find the first one that is missing. However, this method requires $O(N\log N)$. The second solution only requires $O(N)$. Notice that there are $N$ strings, each of which has $N$ bits, and the missing binary string is the one that is different from all strings in the array. Therefore, we can simply construct the missing binary string such that its $i_{th}$ bit is different from the $i_{th}$ bit of the $i_{th}$ string. 
  
  
$2104$. Sum of Subarray Ranges  
https://leetcode.com/problems/sum-of-subarray-ranges/  
This problem needs us to compute the sum of differences between each subarray's largest element and smallest element. The sum can be transformed into the difference between the sum of the largest elements of in each subarrays and the sum of the smallest elements in each subarrays. Therefore, we need to sum up all the largest (smallest) elements of all subarrays. If we want to compute the largest (smallest) sum, for each of the element, we want to know in how many subarrays it is the largest (smallest) element. Therefore, we will use a descending (ascending) monotonic stack. Suppose the top two elements in the stak are $j$ and $k$, and current element we try to push into the stack is $i$. If $nums[i] > nums[k]$ ($nums[i] > nums[k]$), then the number of subarrays in which $nums[k]$ is the largest (smallest) is $(i-k)\cdot(k-j)$, since the subarrays can start with any element with index between $j$ and $k$ and end with any element with index between $k$ and $i$. Then the $k_{th}$ element contributes $nums[k]\cdot(i-k)\cdot(k-j)$ to the largest (smallest) sum. After computing the two sums, we subtract the samllest sum from the largest sum to get the final result.  
  
   
$2289$. Steps to Make Array Non-decreasing  
https://leetcode.com/problems/steps-to-make-array-non-decreasing/  
A dynamic programming problem solved with a monotonic stack. Suppose that we have a $sequence_1$ of number starting with $a$, which is also the largest number, and it takes $x$ steps to make $sequence_1$ non-decreasing (remaining only $a$ itself). Now another $sequence_2$ that starts with a number $b$, for which $b$ is the largest number in $sequence_2$, $b > a$, and all other numbers in $sequence_2$ are less than $a$, is prepended to $sequence_1$, and it took $y$ steps for $sequence_2$ to be non-decreasing (remaining $b$ itself). The question now is that how many steps it takes to make the combined $sequence_3$ non-decreasing (remaining $b$ only). After $y$ steps, the first part of $sequence_3$ has only $b$ itself, and $a$ is to be removed in the next step, which means it needs at least $y+1$ steps to finish. However, if $x>y+1$, then when $a$ is removed, there are some number belonging to $sequence_1$ that are not removed yet; but since $b$ > $a$, nothing is different for the remaining numbers, and they will still be removed in $x$ steps. Hence, the total number of steps that $sequence_3$ needs is $max(y+1, x)$, and for a number at index $i$ being $b$ and a number at index $j$ being $a$, the dynamic programming relationship is $max(dp[i] + 1, dp[j])$. However, how do we map each number to $a$ or $b$? We use a monotonic stack. Traversing from back to front and keeping the stack in an ascending order, then the current traversed number is $b$, while the number whose index is at the top of the stack is $a$, if it is smaller. If the current traversed number is smaller, we can push it to the top of the stack, since we have calculated the $x$, and it can be $a$ for another number.
