# 2402. Meeting Rooms III  
🔗 [Leetcode Link](https://leetcode.com/problems/meeting-rooms-iii/)

---

## ❓ Problem Statement

You are given an integer `n`. There are `n` rooms numbered from 0 to n - 1.  
You are given a 2D integer array `meetings` where meetings[i] = [starti, endi] means  
that a meeting will be held during the half-closed time interval [starti, endi).  
All the values of starti are unique.

Meetings are allocated to rooms as follows:
- Use the **lowest-numbered unused room**.
- If none are free, **delay the meeting** until the earliest one becomes available.
- Meetings must be scheduled in the order of their **original start times**.

Return the number of the room that held the **most meetings**.

---

## 🧪 Examples

**Example 1:**
Input: n = 2, meetings = [[0,10],[1,5],[2,7],[3,4]]
Output: 0


---

## ✅ Code (Python)

```python
import heapq
from typing import List

class Solution:
    def mostBooked(self, n: int, mt: List[List[int]]) -> int:
        mt.sort()
        cnt = [0]*n
        free = list(range(n))
        heapq.heapify(free)
        busy = []
        for s, e in mt:
            while busy and busy[0][0] <= s:
                t, r = heapq.heappop(busy)
                heapq.heappush(free, r)
            d = e - s
            if free:
                r = heapq.heappop(free)
                heapq.heappush(busy, (e, r))
            else:
                t, r = heapq.heappop(busy)
                heapq.heappush(busy, (t + d, r))
            cnt[r] += 1
        mx = max(cnt)
        for i in range(n):
            if cnt[i] == mx:
                return i
