### 1) Subarray sum \< x

```cpp
/*
Given an array arr and an integer x, find the largest subarray whose sum < x

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   int i = 0;
   int j = 0;
   int start = 0;
   int end = 0;
   int sum = arr[0];
   int ans = 0;
   while (i < n && j < n)
   {
       if (i == j)
       {
           if (arr[i] > k)
           {
               i++;
               j++;
               sum = arr[i];
           }
           else
           {
               int len = 1; // since i and j are on the same
               ans = max(ans, len);
               j++;
               if (j < n)
               {
                   sum += arr[j];
               }
           }
       }
       else // i!=j
       {
           if (sum > k)
           {
               // it means arr[i...j-1] was the valid pair
               start = i;
               end = j - 1;
               int len = end - start + 1;
               ans = max(ans, len);
               sum -= arr[i];
               i++;
               sum -= arr[j];
               j--;
               // handling a corner case where i might be > j
               if (i > j)
               {
                   j = i;
               }
           }
           else
           {
               int len = abs(i - j) + 1;
               ans = max(ans, len);
               j++;
               if (j < n)
               {
                   sum += arr[j];
               }
           }
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 2, 1, 1, 0, 0, 1};
   int k = 4;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 2) Pair sum \< k

```cpp
/*
You are given two sorted arrays ; both arrays were of size N ;
you have to find a pair of numbers such that sum of those numbers <=x and as close as possible to x

N = 4
A = 1 4 5 7
B = 10 20 30 40
X = 32

Output : 31 (1+30)
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b, int x)
{
   int n = a.size();

   // two variables first and second to store 2 numbers
   int first = 0;
   int second = 0;

   // two pointers i and j for 2 arrays
   int i = 0;
   int j = n - 1;
   int v = 0; // stores numbers <=target nearest to them
   int vv = INT_MAX; // stores numbers >= target nearest to the target
   while (i < n && j >= 0)
   {
       if (a[i] + b[j] < x)
       {
           int sum = a[i] + b[j];
           if (sum > v)
           {
               v = sum;
               first = a[i];
               second = b[j];
           }
           i = i + 1;
       }
       else
       {
           if (a[i] + b[j] > x)
           {
               int sum = a[i] + b[j];
               if (sum < vv)
               {
                   vv = sum;
               }
           }

           j = j - 1;
       }
   }
   int d1 = vv - x;
   int d2 = x - v;
   if (d1 > d2)
   {
       return v;
   }
   else
   {
       return vv;
   }
}

int main()
{
   vector<int> a = {1, 4, 5, 7};
   vector<int> b = {10, 20, 30, 40};
   int x = 32;
   cout << solve(a, b, x) << endl;
   return 0;
}
```

### 3) Google Angle

```cpp
/*
You are standing at the top of the skyscraper in NYC with camera,
find the direction you should face to take the photo with most buildings

Input array sorted based on angles
2 inputs -> array with each entry representing 1 building at arr[i] degree
and field of view is given as 30 degree, so output the angle at which you will stand that maximum buildings are covered in 30 degree range

arr = [5, 9, 20, 22, 28, 35, 60, 350, 358, 359, 360]
field of view = 30

output : angle with maximum buildings

understanding:  find the largest length of subarray whos difference <= angle

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int angle)
{
   int n = arr.size();
   int i = 0;
   int j = 0;
   int d = 0;
   int ans = 0;
   while (i < n && j < n)
   {
       if (i == j)
       {
           if (arr[i] > angle)
           {
               i++;
               j++;
               d = arr[i];
           }
           else
           {
               int len = 1;
               ans = max(ans, len);
               j++;
               d = arr[j] - arr[i];
           }
       }
       else // i!= j
       {
           if (d > angle)
           {
               // this means [i...j] > angle so [i...j-1] is the longest length so far which is already calculated

               i++;
               d = abs(arr[j] - arr[i]); //  51, 40, 38, 32, 25
               if (i > j)
               {
                   j = i;
               }
           }
           else
           {
               int len = abs(i - j) + 1;
               ans = max(ans, len); // 2,3,4,5,6
               j++;
               if (j < n)
               {
                   d = arr[j] - arr[i]; // 15,17,22,30,65
               }
           }
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {5, 9, 20, 22, 28, 35, 60, 350, 358, 359, 360};
   int angle = 30;
   cout << solve(arr, angle) << endl;

   return 0;
}
```

### 4) Count pairs in sorted array whose sum \< x

```cpp
/*
Count pairs in a sorted array whose sum is less than x
Input  : arr[] = {1, 2, 3, 4, 5, 6, 7, 8}
        x = 7
Output : 6
Pairs are (1, 2), (1, 3), (1, 4), (1, 5)
         (2, 3) and (2, 4)
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int x)
{
   int n = arr.size();

   // We will use 2 pointers, i will start from the 0th position and j from n-1th position
   int i = 0;
   int j = n - 1;
   int cnt = 0;
   while (i < j)
   {
       if (arr[i] + arr[j] >= x)
       {
           j--;
       }
       else
       {
           cnt += j - i; // all possible for that ith value
           i++;
       }
   }
   return cnt;
}

int main()
{
   vector<int> arr = {1, 2, 3, 4, 5, 6, 7, 8};
   int x = 7;
   cout << solve(arr, x) << endl;
   return 0;
}
```

### 5) Move Zeroes Leetcode

[Move Zeroes Leetcode](https://leetcode.com/problems/move-zeroes/description/)

-> [My Submission](https://leetcode.com/problems/move-zeroes/submissions/1156190124/) The code is self explanatory

### 6) Amazon SDE 2 no of unique subarrays with length

[Amazon SDE 2 no of unique subarrays with length](https://docs.google.com/document/d/1c5iQN5uZ9Aj29u7dYKeWdW7nasrz_zkjUnO70CdIksM/edit) \> You are given an array of size “N”. For each and every subarray -> find the count of distinct elements in it -> put all those counts in new array -> sort it -> find the middle element

Solution Description: We will use binary search combined with 2 pointer in our check function of binary search
Here, we use the count() function as our check function of binary search code where we count the number of distinct elements for each subarray. Here we count the distinct elements that are either \< or = mid. Then, in our solve() function we run binary search to find the mid value that satisfies our count() function and return it in the end
-> My Solution:

The output **2** is actually correct for the array {5, 5, 2, 2, 1}.

To understand why, we need to look at the "uniqueness" (number of distinct elements) of every possible subarray. Let's break it down:

### 1. Generate Subarray Uniqueness

There are $\frac{5 \times 6}{2} = 15$ subarrays. Let's list their uniqueness values:

- **Size 1:** [5], [5], [2], [2], [1] $\rightarrow$ **1, 1, 1, 1, 1**
- **Size 2:** [5,5\](1), [5,2\](2), [2,2\](1), [2,1\](2) $\rightarrow$ **1, 2, 1, 2**
- **Size 3:** [5,5,2\](2), [5,2,2\](2), [2,2,1\](2) $\rightarrow$ **2, 2, 2**
- **Size 4:** [5,5,2,2\](2), [5,2,2,1\](3) $\rightarrow$ **2, 3**
- **Size 5:** [5,5,2,2,1\](3) $\rightarrow$ **3**

### 2. Find the Median

If we collect all 15 uniqueness values and sort them:

1, 1, 1, 1, 1, 1, 1, 2, 2, 2, 2, 2, 2, 3, 3

Total elements = 15. The median is the **8th element**.

The 8th element in that sorted list is **2**.

Code :

```cpp
#include <bits/stdc++.h>
using namespace std;

// Function to count subarrays with at most 'target' distinct elements
long long getSubarraysWithTargetRank(vector<int> &arr, int target)
{
    int n = arr.size();
    if (target == 0) return 0;

    unordered_map<int, int> mp; // to keep track of frequence of each element and measure distinct element
    int distinct_count = 0;     // Number of distinct subarray
    long long count = 0;        // length of each subarray
    int left = 0;

    // 2 pointer sliding window
    for (int right = 0; right < n; right++)
    {
        if (mp[arr[right]] == 0) // not present in the current subarray so it is distinct
        {
            distinct_count++;
        }
        mp[arr[right]]++;

        // sliding the left part of the window ahead to the appropriate place
        while (distinct_count > target)
        {
            mp[arr[left]]--;
            if (mp[arr[left]] == 0)
            {
                distinct_count--; // since arr[left] does not exist in the current subarray
            }
            left++;
        }

        count += (long long)right - left + 1; // total count of subarrays with target number of distinct elements
    }
    return count;
}

// Binary search to find the uniqueness value at a specific rank
int binSearchRank(vector<int> &arr, long long rank)
{
    int n = arr.size();
    int left = 1;
    int right = n;
    int ans = -1;

    while (left <= right)
    {
        int mid = left + (right - left) / 2;

        // we should find the largest possible
        // This function counts how many subarrays have at most mid no. of distinct elements
        if (getSubarraysWithTargetRank(arr, mid) >= rank)
        {
            ans = mid;
            right = mid - 1;
        }
        else
        {
            left = mid + 1;
        }
    }
    return ans;
}

// Main solve function to handle median logic
int solve(vector<int> &arr)
{
    int n = arr.size();
    long long totalSubarrays = (long long)n * (n + 1) / 2;
    int ans = 0;

    if (totalSubarrays % 2 != 0) // odd number of subarrays so one middle element
    {
        long long rank = (totalSubarrays + 1) / 2;
        ans = binSearchRank(arr, rank); // BINARY SEARCH to find the largest possible median close to totalSubarrays/2. Max value: rank. We need to find the closest to them
    }
    else // even number of subarrays so 2 medians. We need to take their average
    {
        long long rank1 = totalSubarrays / 2;
        long long rank2 = rank1 + 1;

        // BINARY SEARCH to find the largest possible median. Max value: rank1 & rank2. We need to find the closest to them
        int ans1 = binSearchRank(arr, rank1);
        int ans2 = binSearchRank(arr, rank2);

        ans = (ans1 + ans2) / 2; // average of the 2 will be our median in case of even number of subarrays
    }
    return ans;
}

int main()
{
    vector<int> arr = {5, 5, 2, 2, 1};
    cout << "Median: " << solve(arr) << endl;

    return 0;
}
```

### 7) Rubrik OA

[Rubrik OA](https://docs.google.com/document/d/1qJm45nHXryDn2FPldVQUvFcUtP0c152ubx7CtXeYycw/edit?addon_store)

Solution description: Here, the time complexity is amortized O(n). Observation:-\> First element of Array A = First element of Array B if it is not happening then for sure you need to bring some element in Array B at the start position which will be equal to Array A element -> you can do this operation right now or after some operations but it is guaranteed that you will have to do it

-> Now the problem has changed to find the largest subarray in Array A which is coming as a subset in Array B

We will use a hashmap to store the {element, index} of array B.
So that we can immediately find if the subsequent elements in B are present after the current element or before it with the condition
If (mp[a[j\]\] \< mp[a[j+1\]\]) -> this means that in the Bth array the the j+1th element is present after the jth element

*/\**
*We are given two arrays A and B of size “N”; which have integers strictly from 1 to N coming single times.*
*Our task is to convert Array B -> Array A in minimum operations.*

*Operation:*
*-> Pick a number from Array B and put it at the start of Array B*
*OR*
*-> Pick up number from Array B and put it at the end of Array B*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();

   // hashmap to store the element and index of B array
   unordered_map<int, int> mp;
   for (int i = 0; i < n; i++)
   {
       mp[b[i]] = i;
   }

   //  2 loops to find the largest subarray in array A which is present as a subset in array B
   // TC: amortised O(n)

   int i = 0;
   int ans = 0;

   while (i < n)
   {

       int count = 0;
       int j = i;
       while (j < n - 1)
       {
           if (mp[a[j]] < mp[a[j + 1]]) // A[j] appears before A[j+1] in the B array hence can be a subset
           {
               count++;
           }
           else
           {
               break;
           }
           j++;
       }
       i = uu + count + 1; // i is getting incremented by a large number each time so TC = amortized O(n)
       ans = max(ans, count + 1);
   }
   return n - ans;
}

int main(void)
{
   vector<int> a = {4, 2, 3, 1, 5, 6};
   vector<int> b = {3, 1, 4, 6, 5, 2};
   cout << solve(a, b) << endl;
   return 0;
}
```

# Two Pointers + Sliding Window

### 1) DataStream Leetcode

[DataStream Leetcode](https://leetcode.com/problems/find-consecutive-integers-from-a-data-stream/description/)

-> [My Submission](https://leetcode.com/problems/find-consecutive-integers-from-a-data-stream/submissions/1157486145/) The code is self explanatory

### 2) Amazon OA : Given list of processes where each value representing memory…

Amazon OA : Given list of processes where each value representing memory consumption by the process

and given one variable m representing the number of processes to be removed.
We need to delete m number of processes from the list in contiguous manner and return minimum amount of main memory
used by all the processes running after deleting contiguous segments of processes.

Example - processes = {10,4,8,13,20}, m = 2;
output = 22 [removing 13 and 20 as its consuming large memory]

Code : (The code is self explanatory)

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int m)
{
   int n = arr.size();

   // we need to use a sliding window of size m and calculate the sum for each instance

   int i = 0;
   int j = 0;

   int maxi = 0;
   int sum = 0;
   while (j < n && i < n)
   {

       if (j - i + 1 <= m)
       {
           sum += arr[j];
           j++;
           maxi = max(maxi, sum);
       }
       else
       {
           sum -= arr[i];
           i++;
       }
   }
   // now, maxi contains the max memory consumed by the m consecutive process
   // to find the remaining amount of memory consumption, we need to subtract the total consumption with maxi

   // finding the total consumption
   int tt = 0;
   for (auto it : arr)
   {
       tt += it;
   }

   return tt - maxi;
}

int main()
{ //     i  j
   vector<int> arr = {10, 4, 8, 13, 20};
   int m = 2;
   cout << solve(arr, m) << endl;
   return 0;
}
```

### 3) No of Subarrays with sum between L and R

[No of Subarrays with sum between L and R](https://www.geeksforgeeks.org/problems/count-the-number-of-subarrays/1?utm_source=geeksforgeeks&utm_medium=ml_article_practice_tab&utm_campaign=article_practice_tab)

-> Here, we are using the trick : No of Subarrays with sum in range (L,R)= Number of subarrays with sum in range (0, R) - No of subarrays with sum in range (0, L-1)

### 4) Given a string array of characters and an integer K, find the number of…

Given a string array of characters and an integer K, find the number of subarrays with least distinct characters K

**REMEMBER THESE STEPS:**

-> My Solution :

```cpp
/*
Given a string array of characters and an integer K, find the number of subarrays with least distinct characters K
*/

#include <bits/stdc++.h>
using namespace std;

int solve(string &s, int k)
{
   int n = s.size();

   // creating a map to store distinct characters and their frequency
   unordered_map<char, int> mp;

   int ans = 0;
   // traversing the string
   int j = 0;
   for (int i = 0; i < n; i++)
   {
       // calc
       mp[s[i]]++;

       while (j <= i && mp.size() >= k) // no of distinct characters have reached atleast k
       {
           mp[s[j]]--; // removing the left most element in the window

           if (mp[s[j]] == 0)
           {
               mp.erase(s[j]);
           }
           ans += n - i + 1;

           j++; // sliding the window
       }
   }

   return ans;
}

int main()
{
   string s = "abccd";
   int k = 3;
   cout << solve(s, k) << endl;
   return 0;
}
```

### 5) Given an array and an integer k, find the maximum sum of k consecutive…

Given an array and an integer k, find the maximum sum of k consecutive subarrays such that no two elements are same in

one set of k elements. the sets can overlap
eg: 1,2,7,7,4,3,6 , k = 3;
observation: {1+2+7}, {7+4+3}, {4+3+6}
10, 14, 13
ans = max(10,14,13) = 14
ans : 14

Solution description:  Here, we use a map to store the k set of elements and also to check if the set does not contain any duplicate elements in it. We use two pointers with sliding window to iterate through the array checking the sum and keeping a track of the maximum sum at at each iteration. The rest of the code is self explanatory.

```cpp
/*
Given an array and an integer k, find the maximum sum of k consecutive subarrays such that no two elements are same in
one set of k elements. the sets can overlap

eg: 1,2,7,7,4,3,6 , k = 3;
observation: {1+2+7}, {7+4+3}, {4+3+6}
           10, 14, 13
           ans = max(10,14,13) = 14
ans : 14
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   int i = 0;
   int j = 0;

   int maxi = 0;
   int sum = 0;

   unordered_map<int, int> mp; // {element, frequency} to keep a track of visited
   while (j < n)
   {
       sum += arr[j];
       mp[arr[j]]++;

       if (j - i + 1 < k)
       {
           j++;
       }
       else if (j - i + 1 == k)
       {
           // we need to check if this set of elements are unique or not to be valid
           if (mp.size() == k)
           {
               maxi = max(maxi, sum);
           }

           sum -= arr[i]; // removing the left most element from the sum as it is no longer required
           mp[arr[i]]--;
           if (mp[arr[i]] == 0)
               mp.erase(arr[i]);

           // checking for the next set of elements
           i++;
           j++;
       }

       // the case of (j-i+1 > k) will not arise at all in this code!
   }
   return maxi;
}

int main()
{
   vector<int> arr = {1, 2, 7, 7, 4, 3, 6};
   int k = 3;
   cout << solve(arr, k) << endl;

   return 0;
}
```

### 6)

```cpp
/*
Given an array arr containing both positive and negative integers, find the maximum subarray product
#include <bits/stdc++.h>
using namespace std;

int solve (vector<int>&arr)
{

    int n = arr.size();
    if (n == 0)
        return 0;

    int currMax = arr[0];
    int currMin = arr[0];
    int prevMax = arr[0];
    int overallMax = arr[0];
    for (int i = 1; i<n; i++)
    {
        prevMax = currMax; // because we need to calculate the currMin and currMax with the prevMax value so we store it before updating the currMax;

        currMax = max (arr[i], max(arr[i]*prevMax, arr[i]*currMin));

        currMin = min (arr[i], min(arr[i]*prevMax, arr[i]*currMin));

        overallMax = max(overallMax, currMax);
    }
    return overallMax;
}

int main() {

    vector<int>arr = {2,3,-2,4};

    cout<<solve(arr)<<endl;

    return 0;
}
```

### 7) Longest Substring without repeating characters

[Longest Substring without repeating characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/description/)

Given a string s, find the length of the longest
substring
without repeating characters.

Example 1:

Input: s = "abcabcbb"
Output: 3
Explanation: The answer is "abc", with the length of 3.
Example 2:

Input: s = "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: s = "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3.
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

-> Here, we are using 2 pointer method along with a set. We loop through the string ones and in each iteration we keep a track of the left and right part of the substring to keep a track of it’s length (left-right+1). If duplicate is found we remove the duplicate letter and keep moving forward

[My Solution](https://leetcode.com/problems/longest-substring-without-repeating-characters/submissions/1515680123/)

```cpp
/************************************************************/
```

### 8) Two pointers with deque variation

<https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit/submissions/2170940792/>

Here, 2 deques are used to keep a track of the largest and smallest number in the range [start, end]

### 9) Rippling OA :

```cpp
/*
HackerLand Sports Club wants to send a team for a relay race. There are n racers indexed from 0 to n-1, each with a speed of speed[i] units.
The coach wants to send a contiguous segment of racers with the same speed to ensure smooth baton transfer. To maximize the team size, the coach may remove at most k racers from the group.
Given the array of racer speeds and an integer k, find the maximum possible number of racers in a contiguous segment with the same speed after removing at most k racers.
Example
n = 6
k = 2
speed = [1, 4, 4, 2, 2, 4]

The optimal strategy is to remove the two racers with speed 2, resulting in [1, 4, 4, 4]. Now there are 3 contiguous racers with speed 4, which is the maximum possible.
Therefore, the answer is 3.

SOLUTION DESCRIPTION:
We will store every index of each element in an adjacency list.
we can easily calculate the ABSENT FACTOR for each element
ABSENT FACTOR: count of elements in a subarray which is not the current element we are searching

eg: {8,8,4,4,4,8,8,8,8}
Here, if we are searching for 8, then the absent factor of 8 for the entire array is 3 as there are 3 elements in the range which != 8

*/
#include <bits/stdc++.h>
using namespace std;

int solve(const vector<int> &arr, int k)
{
   int n = arr.size();

   int maxi = INT_MIN;
   for (auto it : arr)
   {
       maxi = max(maxi, it);
   }

   vector<int> adj[maxi]; // because we need to have adj[largestElement].push_back(positions)

   for (int i = 0; i < n; i++)
   {
       int element = arr[i];
       adj[element].push_back(i);
   }
   int ans = 0;
   // iterating the adjacency list
   for (int i = 1; i <= maxi; i++)
   {
       // here i is the current element
       vector<int> pos = adj[i]; // indices of the current element

       int absentFactor = 0;
       int len = 0;
       // iterating the positions using two pointers
       for (int i1 = 0, j1 = 0; j1 < pos.size(); j1++)
       {

           if (j1 > 0)
           {
               absentFactor += pos[j1] - pos[j1 - 1] - 1;
           }

           while (absentFactor > k)
           {
               if (i > 0)
               {
                   absentFactor -= pos[i1] - pos[i1 - 1] - 1;
               }

               i1++;
           }
           len = max(len, j1 - i1 + 1);
       }
       ans = max(ans, len);
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 4, 4, 2, 2, 4};
   int k = 2;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 10) SalesForce OA

```cpp
/*
Given two array A and B :- Find the number of good indices in array B

Good Index meaning - When you remove this index :- array B becomes a subsequence of array A

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();
   int m = b.size();

   vector<int> prefB(m, n);
   vector<int> suffB(m, -1);

   int i = 0;
   int j = 0;

   while (j < n && i < m)
   {
       if (a[j] == b[i])
       {
           prefB[i] = j;
           i++;
           j++;
       }
       else
       {
           j++;
       }
   }

   i = m - 1;
   j = n - 1;

   while (i < m && j >= 0)
   {
       if (a[j] == b[i])
       {
           suffB[i] = j;
           i--;
           j--;
       }
       else
       {
           j--;
       }
   }

   int good = 0;
   for (int i = 0; i < m; i++)
   {
       int left = (i > 0) ? prefB[i - 1] : -1;
       int right = (i < m - 1) ? suffB[i + 1] : n;

       if (left < right)
       {
           good++;
       }
   }
   return good;
}

int main()
{
   vector<int> a = {1, 2, 5, 8, 8, 6, 5, 4, 3};
   vector<int> b = {5, 5};
   cout << solve(a, b) << endl;
   return 0;
}
```

### 11) Microsoft OA

```cpp
/*
There is an array A, consisting of N integers, and two integers, L and R. Your task is to find the shortest fragment of consecutive elements of A that contains every integer from L to R inclusive. Given A, L and R, return the length of the shortest fragment of A that contains all numbers from L to R. If no such fragment exists, the function should return -1.

We will use a hashmap to store the current elements which fall within the range l and r. After this, we will count the size of the hashmap in each iteration. If it is equal to l-r+1 then we have our range. The length of that range is j-i+1

also, when we move i forward, we need to decrement the frequency of arr[i] from the hashmap
*/
#include <bits/stdc++.h>
using namespace std;

int solve(const vector<int> &arr, int l, int r)
{
   int n = arr.size();

   unordered_map<int, int> mp;

   int i = 0;
   int j = 0;
   int ans = INT_MAX;
   while (j < n)
   {
       if (arr[j] >= l && arr[j] <= r)
       {
           mp[arr[j]]++;
       }

       if (mp.size() == r - l + 1)
       {
           int length = abs(j - i) + 1;

           if (arr[i] >= l && arr[i] <= r)
           {
               mp[arr[i]]--;
               if (mp[arr[i]] == 0)
               {
                   mp.erase(arr[i]);
               }
           }
           ans = min(ans, length);

           i++;

           if (arr[j] >= l && arr[j] <= r) // removing the jth element [i+1... j-1] because jth element is added again at the top so to avoid duplicate frequency of j elements in the map
           {
               mp[arr[j]]--;
           }
       }
       else
       {
           j++;
       }
   }
   if (ans == INT_MAX)
       return -1;
   return ans;
}

int main()
{
   vector<int> arr = {2, 1, 4, 3, 2, 1, 1, 4};
   int l = 2;
   int r = 4;
   cout << solve(arr, l, r) << endl;
   return 0;
}
```

### 12) Leetcode Binary string

Question: <https://leetcode.com/problems/count-the-number-of-substrings-with-dominant-ones/description/>

Code:
Here, we use 2 loops i and j but the time complexity is not N^2 because we use certain conditions to check how far we can move jth pointer in one go. So it decreases time complexity a lot

```cpp
int numberOfSubstrings(string s) {
       int n = s.size();
       vector<int>prefix(n,0);
       prefix[0] = s[0]-'0' == 1?1:0;
       for(int i = 1; i<n; i++)
       {
           prefix[i] = prefix[i-1] + (s[i]-'0' == 1?1:0);
       }
       int ans = 0;
       for(int i = 0; i<n; i++)
       {
           int zero = 0, one = 0;
           for(int j = i; j<n; j++)
           {
               int one = prefix[j] - (i==0?0:prefix[i-1]);
               int zero = (j-i+1)-one;

               // Case 1: non Dominant
               if(zero*zero > one)
               {
                   int requiredOne = (zero*zero)-one -1; // -1 because j++ will already move the loop by one place
                   j += requiredOne; // move j this many times forward to increase chance of having some more 1s
               }

               // Case 2: Exactly Dominant
               else if(zero*zero == one)
               {
                   ans++;
               }
               // Case 3
               else if(zero*zero < one)
               {
                   ans++;
                   //how far we can safely skip forward since we have extra ones
                   int skip = (int)floor(sqrt(one))-zero;

                   int nextJ = j+skip;
                   if(nextJ >=n)
                   {
                       ans += (n-j-1);
                   }
                   else
                   {
                       ans += skip; // because this many subarrays are also valid along with ans++
                   }
                   j = nextJ;
               }
           }
       }
       return ans;
   }
```

### 13) Amazon OA MEX

Brute force:

```cpp
/*
MEX: Minimum Excluded Value. It is non negative in nature
Eg: for the array {0,1,2,3,4,6,8,12} the MEX value is 5. It is the smallest non negative excluded value in the array

Lexiographically larger:
a = {1,2,3}, b = {1,2,4} then b is lexiographically larger than a

for a given array-> {0,2,1,1,3,1,2,4}
MEX prefix ->       {1,1,3,3,4,4,4,5}

From here, we can see that MEX prefix is always an increasing function
*/

#include <bits/stdc++.h>
using namespace std;

vector<int> solve(vector<int> arr)
{
   vector<int> highestMexLexiographically;
   int n = arr.size();
   while (arr.size())
   {
       unordered_map<int, int> mp;
       int mex = 0;
       vector<int> prefMex(n, 0);
       for (int i = 0; i < n; i++)
       {
           mp[arr[i]]++;
           while (mp[mex] != 0)
           {
               mex++;
           }
           prefMex[i] = mex;
       }

       // prefMex is an increasing prefix. Mex value increases as we travel the array forward
       // prefMex[n-1] will always contain the largest mex value. But the largest value may start to appear even before n-1th index.
       // finding the first index of prefMex where the largest value has occurred;
       // we can remove the array from 0th till that index and than calculate again for the remaining indices.
       // This way, we can maximise the lexiographically largest answer which is asked in the question
       int p = prefMex[n - 1];
       int i1 = -1;
       for (int i = 0; i < n; i++)
       {
           if (prefMex[i] == p)
           {
               i1 = i;
               break;
           }
       }
       highestMexLexiographically.push_back(p);
       // now, we need to delete the array till i1
       if (i1 != n - 1)
           i1++; // because the erase function removes till one index before i1 so we need to do i1++ before erasing so that we remove correct elements
       arr.erase(arr.begin(), arr.begin() + i1);
   }
   return highestMexLexiographically;
}

int main()
{
   vector<int> arr = {0, 1, 0, 1};
   vector<int> ans = solve(arr);
   for (auto it : ans)
       cout << it << " ";
   cout << endl;

   return 0;
}

Optimal solution :

/*
Summary: Lexicographically Largest MEX Partition

Core idea: Greedy. At every step, grab the largest MEX you can, using the shortest block that achieves it.

Why it works

The remaining array is always a suffix a[i..n-1], so there are only n possible states.
The largest MEX you can get from a suffix is the MEX of the whole suffix, so the first element of result must be suffix[i].
Among all blocks reaching that MEX, the shortest leaves the longest remainder. That gives better (or equal) later MEX values, and a longer result wins ties.

Two phases

Build suffixMex[] (right to left). Add a[i] to a seen-array and push mex upward. It never decreases, so this is O(n).
Walk left to right.
target = suffixMex[i], append it to result.
If target == 0, take k = 1 (i++).
Otherwise scan forward from i until all of 0..target-1 have been seen, then jump i to just past that point.

Tricks to remember

Round stamping: store the current round number in visited[v] instead of clearing the array. Initialize it to -1, not 0.
a[j] < target check: ignores useless values, and short-circuits so visited[a[j]] is always in bounds (since target <= n).
Values > n can be ignored when building suffixMex, because MEX never exceeds n.

Mini example: a = [1, 2, 0, 3, 0, 1]

suffixMex = [4, 4, 2, 2, 2, 0]
i=0: target 4, the block [1,2,0,3] completes {0,1,2,3}, jump to i=4
i=4: target 2, the block [0,1] completes {0,1}, jump to i=6
Result: [4, 2]

Complexity: O(n) time, O(n) space, because every index is scanned once in each phase.

One-line memory hook: “Suffix MEX tells you what to aim for; shortest block that hits it tells you where to cut.”
*/
#include <bits/stdc++.h>
using namespace std;

vector<int> solve(vector<int> arr)
{
   int n = arr.size();

   // suffix Mex is basically reverse of prefix mex
   vector<int> suffixMex(n, 0);
   unordered_map<int, int> mp;
   int mex = 0;
   for (int i = n - 1; i >= 0; i--)
   {
       mp[arr[i]]++;
       while (mp[mex] != 0)
       {
           mex++;
       }
       suffixMex[i] = mex;
   }

   vector<int> highestMexLexiographically;

   int i = 0;
   int round = 0;
   vector<int> visitedRound(n + 1, -1); // tracks which element has visited in which round
   while (i < n)
   {
       int target = suffixMex[i];
       highestMexLexiographically.push_back(target);
       if (target == 0) // 0 is absent from the rest of the array, so k = 1 suffices
       {
           i++;
           round++;
       }
       else // determines how much i to skip
       {
           int cnt = 0;
           int j = i;

           while (cnt < target)
           {
               if (arr[j] < target && visitedRound[arr[j]] != round)
               {
                   visitedRound[arr[j]] = round;
                   cnt++;
               }
               j++;
           }
           i = j;
           round++;
       }
   }
   return highestMexLexiographically;
}

int main()
{
   vector<int> arr = {0, 1, 0, 1};
   vector<int> ans = solve(arr);
   for (auto it : ans)
   {
       cout << it << " ";
   }
   cout << endl;
   return 0;
}
```

### 14) Meesho OA

```cpp
/*
Given a string which consists of characters from “0” to “9”; find the number of substrings which contain each character
->  “k” number of times.
Example:-> “00888005”
K = 1
Output :

*/

#include <bits/stdc++.h>
using namespace std;

int solve(string &s, int k)
{
   int n = s.size();
   int count = 0;
   for (int p = 1; p <= 10; p++) // window size
   {
       int length = p * k; // if number of distinct digits is p and number of times each char should come is k than required length = p*k
       if (length > n)
       {
           break;
       }
       vector<int> freq(10, 0);    // frequency of each character
       for (int i = 0; i < n; i++) // iterating the string
       {
           // updating the current char in the frequency
           freq[s[i] - '0']++;

           if (i >= length) // current index exceeds length so we need to reduce the frequency window
           {
               freq[s[i - length] - '0']--;
           }

           if (i >= length - 1) // current length is full so we can check if it is a valid substring
           {
               bool trigger = true;
               for (int num = 0; num <= 9; num++)
               {
                   if (freq[num] != k && freq[num] != 0)
                   {
                       trigger = false;
                       break;
                   }
               }
               if (trigger)
               {
                   count++;
               }
           }
       }
   }
   return count;
}

int main()
{
   string arr = "00888005";
   int k = 1;
   cout << solve(arr, k) << endl;

   return 0;
}
```

### 15) JP Morgan

![[Two Pointers-image1.png]]

```cpp
/*
Given an array, find the count of subarrays of size k that are strictly increasing.

Observation:
   The stricty increasing subarrays will never overlap each other
   If all the n element are strictly increasing in the array, then the no of subarrays of size k that are strictly increasing : n-k+1
*/

// ***THIS IS A WRONG SOLUTION
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   int ans = 0;
   int currentLength = 0;
   for (int i = 0, j = 1; j < n; j++)
   {
       if (arr[j] > arr[i])
       {
           currentLength = abs(j - i) + 1;
       }
       else
       {
           i = j;
       }

       if (currentLength == k) // [i..j] is valid
       {
           ans++;
           i++;
       }
   }

   return ans;
}

int main()
{
   vector<int> arr{5, 3, 5, 7, 8};
   int k = 3;
   cout << solve(arr, k) << endl;

   return 0;
}
```

16)
