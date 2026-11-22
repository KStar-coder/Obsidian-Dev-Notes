lower_bound(arr.begin(), arr.end(), num)  -> Returns a pointer to the position of num if the container contains only **one occurrence** of num. Returns a pointer to the first position of num if the container contains **multiple occurrences** of num. Returns a pointer to the position of a number just higher than num, if the container **does not contain an occurrence** of num which is the position of the number when inserted in the already sorted array and sorted again.

upper_bound(arr.begin(), arr.end(), num) -> Returns a pointer to the position of the next higher number than num if the container contains **one occurrence** of num. Returns a pointer to the first position of the next higher number than the last occurrence of num if the container contains **multiple occurrences** of num. Returns a pointer to the position of the next higher number than num if the container **does not contain an occurrence** of num.

Safety case and mid formula :  int mid = low + (high-low) / 2

### 1) First and Last Position of an element

[First and Last Position of an element](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/description/) : Given an array of integers nums sorted in non-decreasing order, find the starting and ending position of a given target value.

If the target is not found return {-1, -1}.

-> [My Solution](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/submissions/1092886357/)
Perform binary search twice, one time for the first index and one time for the last index

The binary search will be a little modified
For first index :

```cpp
If (arr[mid] == target)
{
 Ans = mid;
 Right = mid-1;

 }
```

For last index :

```cpp
If (arr[mid] == target)
{
    Ans = mid;
    left = mid+1;
    }
```

### 2) Search in a rotated sorted array

[Search in a rotated sorted array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/) : Given the array nums **after** the possible rotation and an integer target, return *the index of* target *if it is in* nums*, or* -1 *if it is not in* nums

->[My solution](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1093416330/)

In this scenario,

Eg :     **Input:** nums = [4,5,6,7,0,1,2], target = 0
**Output:** 4 (index of target)

As observed, we have an array with 2 parts of increasing sequence. 1st part : 4,5,6,7 ; 2nd part : 0,1,2.

Our target can lie either in the first increasing sequence or second:

We can start with a binary search,
if nums[mid] \>= nums[0] then we are in the first part.
// if it is not present in the left portion
if (target \> nums[mid] \|\| target \<nums[0])
left = mid+1;

else // not present in the right section
right = mid-1;

This is the case for target in the first part;

If (nums[mid] \< nums[0] -> 2nd part ; below is the case for 2nd part to find the num

// second increasing sequence

// if it is not present in the right portion
if (target \< nums[mid] \|\| target \> nums[n-1])
right = mid-1;
else
left = mid+1;

### 3) Single element in a sorted array

[Single element in a sorted array](https://leetcode.com/problems/single-element-in-a-sorted-array/description/) : You are given a sorted array consisting of only integers where every element appears exactly twice, except for one element which appears exactly once.

Return *the single element that appears only once*.Your solution must run in O(log n) time and O(1) space.

-> [My Solution](https://leetcode.com/problems/single-element-in-a-sorted-array/submissions/) :

Eg : 1,1,2,2,3,4,4,8,8
Here, ans =  4th index

Observation : we have observed that the indices initially for a number x occur at an [even, odd] fashion. 1 ->0th and 1st index ; 2 -> 2nd and 3rd index.

But after the single element occurs, indices occur at an [odd,even] fashion for the remaining numbers eg 4-\> 5th and 6th; 8-\> 7th and 8th

In this binary search, the condition -> while (left\<right) ; here, \<= is not used
While (left \< right) and right = mid-1 reason:
In your code, the condition `while (left < right)` is designed to **shrink the search space** until only one element remains.

- Because you are using `right = mid` (and not `mid - 1`), the loop never "lands" on the answer and stops; it narrows the window until `left == right`.
- At that exact moment, the loop terminates, and `left` is pointing directly at your unique element.

At the end of the loop, our ans is in nums[left] as well as nums[right]
We can return any of that.

```cpp
while (left <right)
       {
           int mid = (left+right)/2;

           if ( (mid%2 == 1 && nums[mid] == nums[mid-1]) || (mid%2 == 0 && nums[mid]== nums[mid+1]) )
           {
               left = mid + 1;
           }
           else
           {
               // since the condition is while (left<right)
               right = mid;
           }

       }
```

return nums[left]; // or nums[left]

### 4) ZScaler OA Minimize the Max element

[ZScaler OA Minimize the Max element](https://docs.google.com/document/d/1Jx6Ily5pkZrjHnpbCppZOEYxiXwmnuyB4Thv9cU5wQ4/edit)

The code is self explanatory :

```cpp
/*
   Given an array of n positive integers the following operations can be performed n number of times. Use a
   1-based index for the array

   1) Choose any i such that 2<=i<=n
   2) Choose any x such that 1<=x<=arr[i]
   3) Set arr[i-1] to arr[i-1] + x
   4) Set arr[i] to arr[i] - x

   Minimize the maximum value of arr using these operations and return the value

   -> After a certain minimum value, all the numbers will be possible
   So, our case looks like
   F F F F F F T T T T T .......

   We need to find the first occurrence of True

*/

#include <bits/stdc++.h>

using namespace std;

bool check(int mid, vector<int> &arr, int n)
{
   vector<int> g = arr;
   int i = n;
   while (i >= 2)// using 1 based indexing
   {
       if (arr[i] >= mid)
       {
           int gg = abs(arr[i] - mid);
           arr[i - 1] += gg;
       }
       i--;
   }
   if (arr[1] <= mid)
   {
       arr = g;
       return true;
   }
   arr = g;
   return false;
}

int solve(vector<int> &arr, int n, int maxi)
{
   // binary search
   int left = 0;
   int right = maxi;
   int ans = -1;
   int k = 1;
   while (left <= right && k == 1)
   {
       int mid = (left + right) / 2;
       if (check(mid, arr, n) == true && check(mid - 1, arr, n) == false)
       {
           ans = mid;
           k = 0;
       }
       else if (check(mid, arr, n) == true)
       {
           right = mid;
       }
       else
       {
           left = mid + 1;
       }
   }
   return ans;
}

int main()
{
   int n;
   cin >> n;

   vector<int> arr(n + 1); // Given in question to use 1 based indexing
   // inserting the elements
   int maxi = 0;
   for (int i = 1; i <= n; i++)
   {
       cin >> arr[i];
       maxi = max(maxi, arr[i]);
   }
   int ans = solve(arr, n, maxi);
   cout << ans << endl;
   return 0;
}
```

### 5) Trapping Rain Water

[Trapping Rain Water](https://leetcode.com/problems/trapping-rain-water/) (not a binary search problem)

-> for each index, amt of water it can hold:
Min (max_left_wall , max_right_wall) - arr[i]

We can simply store max_left_wall and max_right_wall for each element instead of calculating it everytime.

Then we can calculate the total ans from the above for all the indices

### 6) Median in a Matrix

[Median in a Matrix](https://www.interviewbit.com/problems/matrix-median/)

-> [My Submission](https://www.interviewbit.com/problems/matrix-median/submissions/)

-> Here, we will use binary search since the matrix is sorted and then we will use the upper_bound function which will return us the iterator. This is done to find the number of elements less than or equal to the mid element of the binary search.
After this we will find if the iterator is in the 2nd half or the first half of the total number of elements in the matrix. We will update the mid accordingly and store the median at each iteration if the iterator it at the 2nd half of the matrix i.e more towards the right side as our loop condition is while (left \<= right)

### 7) Koko loves bananas

[Koko loves bananas](https://leetcode.com/problems/koko-eating-bananas/description/)

->  [My Solution](https://leetcode.com/problems/koko-eating-bananas/submissions/1128963066/) The code is self explanatory

### 8) Arranging coins

[Arranging coins](https://leetcode.com/problems/arranging-coins/description/)

-> [My Solution](https://leetcode.com/problems/arranging-coins/submissions/1129042381/) The code is self explanatory

### 9) Find smallest letter nearest

[Find smallest letter nearest](https://leetcode.com/problems/find-smallest-letter-greater-than-target/description/)

-> [My Solution](https://leetcode.com/problems/find-smallest-letter-greater-than-target/submissions/1129069996/) Binary search on char datatype… Must read

### 10) Search Insert Position

[Search Insert Position](https://leetcode.com/problems/search-insert-position/description/)

-> [My Solution](https://leetcode.com/problems/search-a-2d-matrix/submissions/1129476872/) Code is self explanatory.
**VERY IMPORTANT CORNER CASE *INSIDE* WHILE LOOP**
**IF (RIGHT == -1)**
**BREAK;**
**\# IT WILL GIVE RUNTIME ERROR IF THIS CASE IS NOT INCLUDED**

### 11) Search a 2D Matrix

[Search a 2D Matrix](https://leetcode.com/problems/search-a-2d-matrix/description/)

-> [My Solution](https://leetcode.com/problems/search-a-2d-matrix/submissions/1129476872/)

**VERY IMPORTANT CORNER CASE *OUTSIDE*  WHILE LOOP**
**IF (RIGHT == -1)**
**BREAK;**
**\# IT WILL GIVE RUNTIME ERROR IF THIS CASE IS NOT INCLUDED**

First, we will perform binary search on the 0th column of the matrix
we will find the element in the 0th column that is just greater than the
target element or is the target element,
If it is greater than the target, we will perform binary search on the row
above it
If it is the target element we will simply return it.

### 12) House Robber

[House Robber](https://leetcode.com/problems/house-robber-iv/description/)

-> [My Solution](https://leetcode.com/problems/house-robber-iv/submissions/1129603528/)

### 13) Aggressive cows

[Aggressive cows](https://practice.geeksforgeeks.org/problems/aggressive-cows/1) self explanatory

-> My Solution

```cpp
bool solve (int mid, int n, int k, vector<int>&stalls)
    {
        //mid = difference
        int cows = 1; // as the first cow will always be placed in a stall
        int lastPlaced = stalls[0]; // keeps a track of the last position

        for (int i = 1; i<n; i++)
        {
            if (stalls[i] - lastPlaced >= mid)
            {
                cows++;
                lastPlaced = stalls[i];
            }
            if (cows >= k)
                return true;
        }

        if (cows >= k)
            return true;
        else
            return false;

    }

    int solve(int n, int k, vector<int> &stalls) {

        // Write your code here

        // F F F F T T T T T T T T T ...
        // We need to find the first occurrence of true

        sort(stalls.begin(), stalls.end());

        int maxiDiff = INT_MIN;

        int left = 1;
        int right = stalls[n-1] - stalls[0];

        int ans = -1;

        while (left <= right)
        {
            int mid = (left+right)/2;

            if (solve(mid, n, k, stalls) == true )
            {
                left = mid+1;
            }
            else
            {
                right = mid-1;
            }

        }
        return right;
    }
```

### 14) Allocate minimum number of pages

[Allocate minimum number of pages](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article) Self Explanatory..

```cpp
bool check (int mid, int arr[], int n, int m )
   {
       int sum = 0;
       int students = 1; // assigning it to 1 as the first student will always be allocated a book
       for (int i = 0; i<n; i++)
       {
           if (arr[i]> mid)
               return false;

           sum += arr[i];
           if (sum > mid)
           {
               students++;

               sum = arr[i];
           }
           if (students > m)
           {
               return false;
           }

       }

       return true;
   }
   //Function to find minimum number of pages.
   int findPages(int arr[], int n, int m)
   {
       //base case
       if(n<m)
           return -1;
       // finding the sum of the elements
       int sum = 0;
       for (int i = 0; i<n; i++)
       {
           sum += arr[i];
       }
       // F F F F  T T T T T T T
       // We need to find the first occurrence of true
       int left = 1;
       int right = sum;
       int ans = INT_MAX;
       while (left <= right )
       {
           int mid = (left+right) / 2;

           if (check(mid,arr,n,m) == true )
           {
               ans = mid;
               right = mid - 1;
           }
           else
           {
               left = mid+1;
           }
           if (right == -1)
           {
               break;
           }

       }
       // now, we got the maximum  subarray sum so that we can divide it into 3 parts
       return ans;

   }
```

### 15) Minimize Max Difference of p pairs

[Minimize Max Difference of p pairs](https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs/description/)

-> [My Solution](https://leetcode.com/problems/minimize-the-maximum-difference-of-pairs/submissions/1131916224/)

### 16) DE Shaw Hard OA

```cpp
/*
In a fictional stock market, fund managers are required to create portfolios by including stocks from different
industries. Given an array of stocks, where stocks[i] represents the number of companies in the ith industry.
Distribute the stocks in such a way such that each portfolio has exactly k distinct types of stocks where k is a given
integer and each company is included only once in the portfolio. Find the number of portfolios possible

Eg:
   k = 2, stocks[] = {3,3,3}

   O/P : 4 (4 portfolios are possible when there are k distinct stocks in each portfolio)

*/

#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, int k, int n)
{
   int total = mid * k; // Total no of stocks for ‘mid’
   for (int i = 0; i < n; i++)
   {
       if (arr[i] > k)
       {
           total -= k;
       }
       else
       {
           total -= arr[i];
       }
   }

   if (total <= 0)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(vector<int> &arr, int k)
{
   int n = arr.size();
   // finding the sum value as it is the right most value of the binary search
   int sum = 0;
   for (auto it : arr)
   {
       sum += it;
   }

   sort(arr.begin(), arr.end(), greater<int>());

   int left = 1;
   int right = sum;

   // T T T T T F F F F F F F F F F F.....
   // We need to find the last occurrence of true
   int ans = 0;

   int dummy = 0;
   while (left <= right && dummy == 0)
   {

       int mid = (left + right) / 2;

       // cout << "Left" << left << endl;
       // cout << "Right" << right << endl;
       // cout << "mid" << mid << endl;

       if (check(mid, arr, k, n) == true && check(mid + 1, arr, k, n) == false)
       {
           // this is the ans
           ans = mid;
           dummy = 1;
       }
       else if (check(mid, arr, k, n) == true)
       {
           left = mid;
       }
       else
       {
           right = mid - 1;
       }
   }
   return ans;
}

int main()
{

   vector<int> arr = {3, 3, 3};
   cout << solve(arr, 2);
   return 0;
}
```

### 17) Decrement Operations (DE Shaw)

[Decrement Operations (DE Shaw)](https://www.desiqna.in/13333/de-shaw-sde-hard-oa-2023-binary-search)

In the solve function, if a number is divisible by mid; No of steps to make it \<= 0 is number / mid;
Else
It is (number/mid) + 1

```cpp
/*
Choose an integer x such that you make all the elements of arr <=0 in not more than maxMoves;

*/

#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, int n, int maxMoves)
{
   int moves = 0;

   for (int i = 0; i < n; i++)
   {
       if (arr[i] % mid != 0)
       {
           moves += (arr[i] / mid) + 1;
       }
       else
       {
           moves += arr[i] / mid;
       }
   }
   if (moves <= maxMoves)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(vector<int> &arr, int maxMoves)
{
   int n = arr.size();
   sort(arr.begin(), arr.end());
   int l = arr[0];
   int r = arr[n - 1];

   // F F F F T T T T T T .....
   // We need to find the first occurrence of T
   int k = 0;
   int ans = INT_MAX;
   while (l <= r && k == 0)
   {
       int mid = (l + r) / 2;

       if (check(mid, arr, n, maxMoves) == true && check(mid - 1, arr, n, maxMoves) == false)
       {
           ans = mid;
           k = 1;
       }
       else if (check(mid, arr, n, maxMoves) == true)
       {
           if (mid == 0)
           {
               ans = mid;
               k = 1;
           }
           else
           {
               r = mid - 1;
           }
       }
       else
       {
           l = mid + 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 2, 3};
   int maxMoves = 4;
   cout << solve(arr, maxMoves) << endl;
   int n = arr.size();

   return 0;
}
```

### 18) Salesforce OA Tom and Holes

[Salesforce OA Tom and Holes](https://docs.google.com/document/d/1DDwlUHu9rzxSXA6On-dCtFMpz_2n8Z9kFM2nlcTDDZI/edit)

-> Minimum capacity -> maximum element in the entire array i.e largest hole.

Maximum capacity -> Sum of all the depths of the holes.

*/\**
*Question link :*
*https://docs.google.com/document/d/1DDwlUHu9rzxSXA6On-dCtFMpz_2n8Z9kFM2nlcTDDZI/edit*
*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, int cats, int n)
{
   int cnt = 1;
   int sum = 0;
   int i = 0;
   while (i < n)
   {
       sum += arr[i];
       if (sum <= mid)
       {
           i++;
       }
       else // sum >= mid;
       {
           cnt++;
           sum = 0;
       }
   }
   if (cnt > cats)
   {
       return false;
   }
   else
   {
       return true;
   }
}

int solve(vector<int> &arr, int cats)
{
   int n = arr.size();

   // low : max element of the arr
   // high : sum of all the elements of the arr

   int maxi = INT_MIN;
   int sum = 0;
   for (auto it : arr)
   {
       maxi = max(maxi, it);
       sum += it;
   }
   // F F F F T T T T T T....
   // We need to find the first occurrence of true
   cout << "maxi: " << maxi << endl;
   cout << "Sum: " << sum << endl;
   int low = maxi;
   int high = sum;
   int ans = 0;
   int k = 0;
   while (low < high && k == 0)
   {
       int mid = low + ((high - low) / 2);
       if (check(mid, arr, cats, n) == true)
       {
           if (mid == 0)
           {
               ans = mid;
               k = 1;
           }
           else // mid is not 0;
           {
               if (check(mid - 1, arr, cats, n) == false)
               {
                   ans = mid;
                   k = 1;
               }
               else
               {
                   high = mid - 1;
                   ans = mid + 1;
               }
           }
       }

       else
       {
           low = mid + 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {5, 3, 20, 16, 18, 1, 10, 10, 9, 8};
   int cats = 3;
   cout << solve(arr, cats) << endl;
   return 0;
}
```

19) [Ultra Hard Amazon OA](https://www.desiqna.in/10528/amazon-sde-internship-coding-questions-and-solutions-2022) You are given an array and an integer k and an array of integers. You must find the length of the longest contiguous subarray where all integers are equal after removing at most k elements from the array.
Example
Input:
[1, -2, 1, 1, 3, 2, 1 ,-2] k = 3
Output: 4

Explanation:
Remove index 1 from original Array:
[1, 1, 1, 3, 2, 1, -2]
Remove index 4 from original Array:
[1, 1, 1, 2, 1, -2]
Remove index 5 from original Array:
[1, 1, 1, 1, -2]

-> ![[Binary Search Documentation-image1.png]]

Need to to 2 pointers first

20) Paypal OA : Need to see the entire video again
### 21) Optimal Point Solution Paypal

[Optimal Point Solution Paypal](https://docs.google.com/document/d/1T3j6PKHw6tbOIWWMsoICs58ffE_hMYiIdoD_EVDMsM8/edit)  Given a set of N distinct points on x-axis, choose k of them such that the minimum distance between any two chosen points is as large as possible. Find the maximum possible minimum distance.

We use the check function to check the minimum difference for each element with each other
Then we use binary search to find the ans on the check function

->

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, int k)
{
   int n = arr.size();
   int current = arr[0];
   int cnt = 0;
   for (int i = 1; i < n; i++)
   {
       int diff = abs(current - arr[i]);
       if (diff >= mid)
       {
           cnt++;
           current = arr[i];
       }
   }

   if (cnt + 1 >= k)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   sort(arr.begin(), arr.end());

   // T T T T F F F F F F
   // We need to find the last occurrence of true

   int low = 1;
   int high = arr[n - 1] - arr[0];
   int ans = 1;
   int dummy = 0;
   while (low <= high && dummy == 0)
   {
       int mid = (low + high) / 2;
       if (check(mid, arr, k) == true)
       {
           if (check(mid + 1, arr, k) == false)
           {
               ans = mid;
               dummy = 1;
           }
           else
           {
               low = mid + 1;
           }
       }
       else
       {
           high = mid - 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 4, 2, 9, 8};
   int k = 3;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 22) Min days to eat N chocolates

[Min days to eat N chocolates](https://docs.google.com/document/d/1hLi_sFb70XILSs-59La1NGZJ1yv8dfJ2VV5efnW-Xk0/edit?addon_store)

For G days ; what is the max amount of chocolates you can eat if G is even? -> G/2 \* (G/2 + 1)

What if G is odd-\> G1 + G2

G1 = V\*(V+1)/2 ; where V = G//2(rough division.)

G2 = Y\*(Y+1)/2 ; where Y = G//2 + 1

We use binary search on G from low to high where low = 1 because at least 1 chocolate is eaten when day = 1; and high = total number of chocolates

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int days, int chocolates)
{
   int ans = 0;
   if (days % 2 == 0)
   {
       int n = days / 2;
       ans = n * (n + 1);
   }
   else
   {
       int n = days / 2;
       int m = (days / 2) + 1;
       int d1 = n * (n + 1) / 2;
       int d2 = m * (m + 1) / 2;
       ans = d1 + d2;
   }

   if (ans >= chocolates)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(int chocolates)
{
   int low = 1;
   int high = chocolates;

   // F F F F T T T T T
   // We need to find the first occurrence of true

   int d = 0;
   int ans = 1;
   while (low <= high && d == 0)
   {
       int mid = (low + high) / 2;

       if (check(mid, chocolates) == true)
       {
           if (check(mid - 1, chocolates) == false)
           {
               ans = mid;
               d = 1;
           }
           else
           {
               high = mid - 1;
           }
       }
       else
       {
           low = mid + 1;
       }
   }
   return ans;
}

int main()
{
   int chocolates = 5;
   cout << "Minimum days to eat " << chocolates << " chocolates : " << solve(chocolates) << endl;

   return 0;
}
```

### 23) Blackrock OA Maximum Greatness

[Blackrock OA Maximum Greatness](https://docs.google.com/document/d/1tn-dP1ZznNLLghvsYPEQMwKO_uv15qG3Fs5toOdnpq4/edit)

You are having an array A of size “N” ; create a new array “B” such that maximum number elements of array B are larger than array “A” at fixed indices. Array B can only be created by rearranging array “A”.

-> Here, we are creating a comparison such that after sorting the given array, if we flip it, we will get the maximum number of indices where array B elements are greater than array A.
EG: 1, 1, 1, 2, 3, 5, 5 - - - -> Array A
->    2, 3, 5, 5, 1, 1, 1 - - - - \> Array B

Here we have 4 elements where array B[i] \> A[i] after rearranging the array A

We will implement something similar in the check function where we use the mid element from binary search as the number of indices we want to check. Eg: if mid = 3 then we check the last element with [3-1]nd element, 2nd last element with [3-2]nd element and so on. If at any case we get that
Eg: [n-2]th element is LESS THAN OR EQUAL TO [mid-2]th element, we return false, if we traverse the entire array will i==0 then we return true.

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr)
{
   if (mid == 0)
   {
       return true;
   }
   int n = arr.size();

   int j = n - 1;
   int i = mid - 1;

   while (i >= 0)
   {
       if (arr[j] <= arr[i])
       {
           return false;
       }
       i--;
       j--;
   }
   return true;
}

int solve(vector<int> &arr)
{
   int n = arr.size();

   // sorting the array to make out task easier for binary search
   sort(arr.begin(), arr.end());

   int low = 0;
   int high = n; //  highest possible indexes

   //  T T T T T  F F F F F F F
   // We need to find the last occurrence of true

   int d = 0;
   int ans = 0;
   while (low <= high && d ==  0)
   {
       int mid = (low + high) / 2;

       if (check(mid, arr) == true)
       {
           if (check(mid + 1, arr) == false)
           {
               ans = mid;
               d = 1;
           }
           else
           {
               low = mid + 1;
           }
       }
       else
       {
           high = mid - 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 3, 5, 2, 1, 5, 1};
   cout << solve(arr) << endl;
   return 0;
}
```

### 24) BNY Mellon Server Investment

[BNY Mellon Server Investment](https://docs.google.com/document/d/1aUZLNxW5Gnu0NeuBYeblxI7Vna8gXNa7J7ZS10-q538/edit)

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, int servers, int money, int sell, int upgrade)
{
   // checking to see if we sell 'mid' number of servers, will we be able to pay for the upgradation of rest of the servers
   int remaining = servers - mid;
   int cost = remaining * upgrade;
   int totalMoney = (mid * sell) + money;
   if (totalMoney >= cost)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(int servers, int money, int sell, int upgrade)
{
   int low = 0;
   int high = servers - 1;

   // minimum number of servers you can sell so
   // that rest of all of it you can upgrade.

   // F F F F T T T T T T T T T T T T
   // We need to find the first occurrence of true

   int ans = servers;
   int kk = 0;
   while (low <= high && kk == 0)
   {
       int mid = (high + low) / 2; // mid is the number of servers we will sell to upgrade max servers
       if (check(mid, servers, money, sell, upgrade) == true)
       {
           if (mid == 0)
           {
               ans = 0;
               kk = 1;
           }
           else
           {
               if (check(mid - 1, servers, money, sell, upgrade) == false)
               {
                   ans = mid;
                   kk = 1;
               }
               else
               {
                   high = mid - 1;
               }
           }
       }
       else
       {
           low = mid + 1;
       }
   }
   return ans;
}

int main()
{
   int n;
   cin >> n;
   vector<int> servers(n), money(n), sell(n), upgrade(n);
   for (int i = 0; i < n; i++)
   {
       cin >> servers[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> money[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> sell[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> upgrade[i];
   }

   cout << endl
        << endl;
   for (int i = 0; i < n; i++)
   {
       int g = solve(servers[i], money[i], sell[i], upgrade[i]);
       int ans = servers[i] - g;
       cout << ans << " ";
   }
   cout << endl;

   return 0;
}
```

### 25) Twilio OA

[Twilio OA](https://docs.google.com/document/d/1Smp8Xfo5zCvvj6VHYrvP6itSzmyG6lDOxivoTimN9JM/edit)

Understanding :-\> Given arrays B and C ; and budget D ; each element in the array B can be multiplied and increased to a new number ; but there is a cost of multiplying which is given by array C

-> Do as many operations as you want but total cost of operations \<=D;

-> Goal -> Minimum value of final array B should be as big as possible!

Analysis:-\> Let's say we want to make the minimum value of array B \>= G ;

-> all elements of array B\>=G

If I give you any array B -> Can you tell me the cost to make the minimum of array B\>=G

-> Make each element \>=G -> Number of operations needed:-\> G/b[i] -> if this is divisible G%b[i]==0 ; v = G/b[i] ; if not ->
v = G/b[i] + 1

-> add v-1 to the cost for each element

-> now your total cost \<= D; then yes!  It is possible to make minimum of array B\>=G

Code :

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, vector<int> &cost, int budget)
{
   int total = 0;
   int n = arr.size();
   for (int i = 0; i < n; i++)
   {

       if (arr[i] < mid)
       {
           int g = 0;
           if (mid % arr[i] == 0)
           {
               g = mid / arr[i];
           }
           else
           {
               g = (mid / arr[i]) + 1;
           }
           total += (g - 1) * cost[i]; // g-1 because one g is already there by default
       }
   }
   if (total <= budget)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(vector<int> &arr, vector<int> &cost, int budget)
{
   int n = arr.size();

   int low = 0;
   int high = budget;
   int ans = 0;
   int kk = 0;

   // T T T T T F F F F F F F F F.....
   // We need to find the last occurrence of mid

   while (low <= high && kk == 0)
   {
       int mid = (low + high) / 2;

       // trying the make all the elements >= mid

       if (check(mid, arr, cost, budget) == true)
       {
           if (mid == 0)
           {
               ans = 0;
               kk = 1;
           }
           else if (check(mid + 1, arr, cost, budget) == false)
           {
               ans = mid;
               kk = 1;
           }
           else
           {
               low = mid + 1;
           }
       }
       else
       {
           high = mid - 1;
       }
   }
   return ans;
}

int main()
{
   int n;
   cin >> n;
   vector<int> arr(n), cost(n);
   for (int i = 0; i < n; i++)
   {
       cin >> arr[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> cost[i];
   }

   int budget = 32;

   cout << solve(arr, cost, budget) << endl;

   return 0;
}
```

### 26) Infosys Super hard OA

[Infosys Super hard OA](https://docs.google.com/document/d/1WgbY8uZyjcgDVhsAWsIp_HC0B1CifCSWjpPss41A538/edit)

-> need to watch the video again. It is about double binary search

### 27) Amazon Hard OA

[Amazon Hard OA](https://docs.google.com/document/d/1c5iQN5uZ9Aj29u7dYKeWdW7nasrz_zkjUnO70CdIksM/edit)

-> need to do 2pointer and sliding window before this

### 28) Rubrick OA

[Rubrick OA](https://docs.google.com/document/d/1j5-TcwAwYRDqm82gaPbZGkSGaeO0fJ7ydz1AXnqfvXw/edit)

Understanding -> Given an array ; find p pairs; calculate the difference of all the pairs; let’s say g = max.difference;try to make g as small as possible! You have to select all the pairs in such a way that the difference becomes as small as possible.

Analysis:-\>

i) If you select p-pairs ; and all their differences are \<=5 ; it means the max difference will also be \<=5

How can you find p-pairs whose difference \<= l

-> sort the array -> now only adjacent differences matter
-> any other pair you take except adjacent will have big difference which you don't want

-> Hence greedy approach of starting from 1st index will work because this way you give maximum space and largest subarray space for other pairs to be born on the right hand side.

ii) Try to find p-pairs with difference\<=0 ; if you fail in it try for difference\<=1 ; if you again fail try for \<=2…..and so on;

iii) The day you find a number such that you are able to find p-pairs all with difference\<=u ; it means u is your answer

iv) u + 1 ; u + 2 ; u + 3 ;........infinity are also correct answer but we want minimum hence printed u

v)Use standard binary search to optimize it

// F F F F T T T T T T T T
We need to find the first occurrence of T. This will tell us the minimum difference to create ‘P’ pairs.

**We will sort the array first as it is not sorted !**

Code :

```cpp
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &arr, int p)
{
   int n = arr.size();
   int cnt = 0;
   int i = 1;
   while (i < n)
   {
       if (arr[i] - arr[i - 1] <= mid)
       {
           cnt++;
           i += 2;
       }
       else
       {
           i++;
       }
   }
   if (cnt >= p)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int solve(vector<int> &arr, int p)
{
   int n = arr.size();
   sort(arr.begin(), arr.end());

   // F F F F T T T T T T ....
   // We need to find the first occurrence of true (T)

   int low = 0;
   int high = arr[n - 1] - arr[0];

   int ans = 0;
   int trigger = 0;
   while (low <= high && trigger == 0)
   {
       int mid = (low + high) / 2;

       if (check(mid, arr, p) == true)
       {
           if (mid == 0) // to avoid check(mid-1) when mid = 0
           {
               ans = mid;
               trigger = 1;
           }
           else
           {
               if (check(mid - 1, arr, p) == false)
               {
                   ans = mid;
                   trigger = 1;
               }
               else
               {
                   high = mid - 1;
               }
           }
       }
       else
       {
           low = mid + 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> nums = {10, 1, 2, 7, 1, 3};
   int p = 2; // O/P : 1 as 2 pairs (1,2) and (2,3) have their min difference = 1;
   cout << solve(nums, p) << endl;
   return 0;
}
```

### 29) Google OA

[Google OA](https://docs.google.com/document/d/1wwYChSCPmQ2u0xaTyt-Sn3byRDmTSMkMt3T96WQNgv4/edit?addon_store)

-> We should sort the array of pairs in the ascending order of cost and use the cheapest costs first

```cpp
/*
We are given 3 arrays; A B C of size “N”;

For each index i ; A[i] >= B[i]

You have to do some operations such that you select (i,j) ; you do
A[i] = A[i] + 1 and A[j] = A[j] - 1
This operation can only be done when a[j] is currently > b[j]. The cost of this single operation is c[j].

Find the maximum possible minimum number present in a[j]. Also, find the minimum cost for the same

Eg:

Test case :
4
1 99 2 99
1 97 2 98
1 5 2 2

O/P:
3 12
Where 3 is the maximised minimum number and 12 is the cost

*/
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> &a, vector<int> &b)
{
   int n = a.size();
   int taker = 0;
   int giver = 0;
   int i = 0;
   while (i < n)
   {
       if (a[i] < mid)
       {
           taker += (mid - a[i]); // amount to be added to make it large
       }
       else
       {
           giver += a[i] - (max(mid, b[i])); //  amount to be safely deducted from giver
           // here, max(mid,b[i]) is used because a[i] can only be deducted if it is > b[i]
       }
       i++;
   }
   if (giver >= taker)
   {
       return true;
   }
   else
   {
       return false;
   }
}

void solve(vector<int> &a, vector<int> &b, vector<int> &c)
{
   int n = a.size();

   // finding the largest number in vector a as it will be the high in binary search

   int high = INT_MIN;
   for (auto it : a)
   {
       high = max(high, it);
   }

   int low = 1;
   int ans = 0;
   int trigger = 0;

   // finding out the minimum number that we can add to maximize the minimum element
// T T T T F F F F F . . .
// WE NEED TO FIND THE LAST OCCURRENCE OF TRUE
   while (low <= high && trigger == 0)
   {
       int mid = (low + high) / 2;

       if (check(mid, a, b) == true)
       {
           if (mid == 0)
           {
               ans = 0;
               trigger = 1;
           }
           else
           {
               if (check(mid + 1, a, b) == false)
               {
                   ans = mid;
                   trigger = 1;
               }
               else
               {
                   low = mid + 1;
               }
           }
       }
       else
       {
           high = mid - 1;
       }
   }
   // now, ans stores the minimum number that we can add to maximize the minimum value
   cout << ans << " ";
   // evaluating the cost

   // storing the cost and the giver in the pairs in a vector for easy calculation
   vector<pair<int, int>> g;
   int i = 0;
   int taker = 0;
   int giver = 0;

   // calculating the giver and taker again like did in the check function. this time to calculate cost
   // And fill the create the g vector for calculating the cost
   while (i < n)
   {
       if (a[i] < ans)
       {
           taker += (ans - a[i]); // amount to be added to make it large
       }
       else
       {
           giver = a[i] - (max(ans, b[i])); //  amount to be safely deducted from giver
           // here, max(mid,b[i]) is used because a[i] can only be deducted if it is > b[i]
           g.push_back({c[i], giver}); // g vector is filled with the givers and their cost
       }
       i++;
   }
   int finalAns = 0;
   sort(g.begin(), g.end());
   int sz = g.size();

   int j = 0;
   trigger = 0;

   while (j < sz && trigger == 0)
   {
       taker = taker - g[j].second; // taker is getting from giver
       if (taker <= 0)              // taken all it can
       {
           trigger = 1;
           taker = taker + g[j].second;
           // getting the last giver amount out to calculate the final cost of the last giver with the prev taker
           finalAns = finalAns + (g[j].first * taker);
       }
       else
       {
           finalAns = finalAns + (g[j].first * g[j].second);
       }
       j++;
   }
   cout << finalAns << endl;
}

int main()
{
   int n;
   cin >> n;
   vector<int> a(n), b(n), c(n);
   for (int i = 0; i < n; i++)
   {
       cin >> a[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> b[i];
   }

   for (int i = 0; i < n; i++)
   {
       cin >> c[i];
   }

   solve(a, b, c);

   return 0;
}
```

30) [Very hard leetcode contest on string](https://leetcode.com/contest/weekly-contest-378/problems/find-longest-special-substring-that-occurs-thrice-ii/)
### 31) Very Hard Infosys Problem Solved

```cpp
/*
Fastenal is the largest fastener distributor in North America. The trucks of the company are required to
deliver boxes of fasteners to their customers. The company has N trucks and each truck has a particular
speed S(1<=S<=10). A truck with a speed S can deliver x box of fasteners in a time given by the function :
F(x) = S * (x^2 - x + 1)
Where F(x) is the time taken in minutes.
The company manager has already taken the orders and wants to know the minimum time taken to complete the orders.
NOTE: All trucks are delivering simultaneously, all trucks are operational.

input 1: No of box of fasteners
input 2: No of trucks
input 3: array of integers denoting the speed of each truck

Sample input 1:
10
4
[1,2,3,4]

Sample output :
13

Explanation: First truck will deliver 4 boxes, 2nd,3rd and 4th will deliver 2 boxes;

UNDERSTANDING:
Given an array of speed of size n ; name of array is arr ; given an integer k : no. of boxes ;
divide no of boxes into each truck such that the maximum time of all trucks by the given
function is as minimum as possible. Minimize the maximum time taken!
*/

#include <bits/stdc++.h>
using namespace std;

int check(int mid, int speedi, int boxes, int n)
{
   // this function returns the max number of boxes possible in speed[i] for time mid
   int low = 0;
   int high = boxes;

   int bb = mid / speedi; // since S*(boxes^2 - boxes + 1) <= mid, therefore (boxes^2 - boxes + 1) <= mid/S;

   int box_for_each_speed = 0;
   while (low <= high)
   {
       int box = (low + high) / 2;

       int totbox = box * box - box + 1;

       if (totbox <= bb)
       {
           if (totbox == bb)
           {
               return box;
           }
           else
           { // the equation may not be equal so we need to keep a track of max boxes accomodated
               box_for_each_speed = max(box_for_each_speed, box);
               low = box + 1;
           }
       }
       else
       {
           high = box - 1;
       }
   }
   return box_for_each_speed;
}

// solve function where mid = time
bool solve(int mid, vector<int> &speed, int boxes, int n)
{
   // here, we need to find the maximum boxes possible for each speed[i] truck.
   // We then keep a track of the total boxes possible in all the trucks
   // if it is >= boxes assigned, it is true else false

   int totalBoxes = 0;
   // checking the maximum box possible for each index in time mid

   for (int i = 0; i < n; i++)
   {
       int box = check(mid, speed[i], boxes, n);
       totalBoxes += box;
   }

   if (totalBoxes >= boxes)
   {
       return true;
   }
   else
   {
       return false;
   }
}

int main()
{
   int boxes;
   cin >> boxes;
   int n;
   cin >> n;
   vector<int> speed(n);

   for (int i = 0; i < n; i++)
   {
       cin >> speed[i];
   }
   /*
   F(x) = S * (x^2 - x + 1)
   in this equation, the more speed the truck has, the more time it will take ! (Infosys logic)
   so, we can sort the speed array, and then the idea is to assign max no of boxes to the slowest
   truck to optimize.
   */

   sort(speed.begin(), speed.end());

   // lowest time possible = 1 min, highest let it be 1e5;
   int low = 1;
   int high = 1e5;

   // we have to binary search the right time in a solve function

   int ans = 0;

   int trigger = 0; // to stop the search when we find the ans trigger = 1
   while (low <= high && trigger == 0)
   {
       int mid = (high + low) / 2;

       if (solve(mid, speed, boxes, n) == true and solve(mid - 1, speed, boxes, n) == false)
       {
           ans = mid;
           trigger = 1;
       }
       else if (solve(mid, speed, boxes, n) == true)
       {
           high = mid - 1;
       }
       else
       {
           low = mid + 1;
       }
   }

   cout << ans << endl;

   return 0;
}

IMPORTANT: GO THROUGH THIS->

Analysis :-

A normal sorted array would look like this -

0 1 2 3 4 5 6 7

Its possible rotations can look like this :-

1 2 3 4 5 6 7 0
2 3 4 5 6 7 0 1
3 4 5 6 7 0 1 2
4 5 6 7 0 1 2 3
5 6 7 0 1 2 3 4
6 7 0 1 2 3 4 5
7 0 1 2 3 4 5 6
-> The original array itself is the next possible rotation in this row.

Can you see how the 0 is going from right to left in each rotation.

If we are able to locate the “0” minimum element of the array -> then array can be divided into two parts :- say -> 3 4 5 6 7 0 1 2 -> after division -> [3 4 5 6 7] + [0 1 2] -> [left-sorted-array] + [right-sorted-array]

-> [l1,r1] + [l2,r2] -> [0,4] + [5,7]

-> If we are able to find the position of the smallest element of the given array using binary search -> then we will be able to find [l1,r1] , [l2,r2] .

-> So focus on finding “u”  (position of smallest element in the array)

-> then , l1 = 0 , r1 = u-1 , l2 = u , r2 = n-1

-> Now once you are able to find u , l1 , r1 , l2 , r2 -> now you can find target element very easily

-> you have [left-sorted] + [right-sorted]

-> using a single if-else condition just check whether the target element comes under left-sorted part or right-sorted part -> say it comes under right-sorted part then just do a binary search algorithm on the right-sorted part to find target element which is a standard DSA problem :)

-> So in 1 binary search this problem :- [https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/) can be solved and in two binary searches this problem can be solved - [https://leetcode.com/problems/search-in-rotated-sorted-array/](https://leetcode.com/problems/search-in-rotated-sorted-array/)
C++ - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984474539/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984474539/)
Java - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984472448/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984472448/)
Py - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984473798/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984473798/)

Now think of how to solve this problem in just 1 binary search?
C++ - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984467741/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984467741/)
Java - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984469044/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984469044/)
Py - [https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984469644/](https://leetcode.com/problems/search-in-rotated-sorted-array/submissions/1984469644/)

-> Follow-up :-
(Were asked in Amazon SDE Internship Interview - source :- [https://leetcode.com/discuss/post/7913310/amazon-sde-intern-interview-experience-b-3pra/](https://leetcode.com/discuss/post/7913310/amazon-sde-intern-interview-experience-b-3pra/) )

i)How to find how many times the array was rotated?
ii)How to find the kth smallest element?
iii)What if there were duplicates? ([https://leetcode.com/problems/search-in-rotated-sorted-array-ii/](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/) , [https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/))

/*
There are m jobs to schedule on n processors. A schedule is balanced if the difference between the number of jobs scheduled on any two neighboring processors does not exceed 1.

The kth processor is the most efficient, and thus, the maximum number of jobs should be scheduled on that processor.

Find the maximum number of jobs that can be scheduled on the kth processor, such that the overall schedule remains balanced.

Note: Each processor must have at least one job scheduled.

Function Description

Complete the function getMaximumJobs in the editor below.

getMaximumJobs has the following parameters:
int n: the number of processors
int m: the number of jobs
int k: the 1-based index of the most efficient processor
Returns

int: the maximum number of jobs that can be scheduled on the kth processor while maintaining a balanced schedule.

Constraints
1 ≤ n ≤ 10^5
n ≤ m ≤ 10^9
1 ≤ k ≤ n
*/
#include <bits/stdc++.h>
using namespace std;

int check(int n, int m, int k, int mid)
{
    vector<int> arr(n + 1, 0);

    int low = 0;

    int maxPossible = mid * n;

    // constructing the array for the minimum possible
    arr[k] = mid;
    int minPossible = 0;
    int i = k + 1;

    while (i <= n)
    {
        arr[i] = arr[i - 1] - 1;
        if (arr[i] == 0)
        {
            arr[i] = 1;
        }
        minPossible += arr[i];
        i++;
    }

    i = k - 1;

    while (i > 0) // 1 based as mentioned in the question
    {
        arr[i] = arr[i + 1] - 1;
        if (arr[i] == 0)
        {
            arr[i] = 1;
        }
        minPossible += arr[i];
        i--;
    }
    minPossible += arr[k];
    // if m lies between max possible and min possible then it is valid
    if (m >= minPossible && m <= maxPossible)
    {
        return 1;
    }
    // if m > maxPossible, we are on the left F part
    if (m > maxPossible)
    {
        return 0;
    }
    // if m < minPossible, we are on the right F part
    if (m < minPossible)
    {
        return 2;
    }
    return -1;
}

int solve(int n, int m, int k)
{

    int left = 1;

    int right = m;
    int ans = 0;
    while (left <= right)
    {
        // F F F F F T T T T F F F F F F F F . . .
        // We need to find the last occurrence of true

        int mid = left + (right - left) / 2;

        int result = check(n, m, k, mid);
        if (result == 1)
        {
            if (check(n, m, k, mid + 1) == 2)
            {
                ans = mid;
                return ans;
            }
            else
            {
                left = mid + 1;
            }
        }
        else
        {
            if (result == 0) // we are on the left part of F
            {
                left = mid + 1;
            }
            else // we are on the right part of F
            {
                right = mid - 1;
            }
        }
    }
    return ans;
}

int main()
{
    int n = 5;  // no. of job processor
    int m = 11; // total jobs
    int k = 3;  // 1 based index for highest jobs
    cout << solve(n, m, k) << endl;
    return 0;
}

/*
Sequence Search

A sequence (X) is defined as:

(X_1 = 0);
(X_2 = A);
(X_3 = B);
(X_i = X_{i-1} + X_{i-2} - X_{i-3}) for all (i > 3).

Given an integer (K), find the (K^{th}) smallest element of sequence $X$.

Input Format
The first line of input will contain a single integer (T), denoting the number of test cases.
Each test case consists of three space-separated integers (A), (B), and (K), as mentioned in the statement.
Output Format

For each test case, output on a new line, the ${K}^{th}$ smallest element in the sequence (X).

Constraints
(1 leq T leq 10^5)
(1 leq A, B, K leq 10^9)
Sample 1

Input:

3
18 13 2
50 30 7
61 41 6

Output:

13
110
123
Explanation

Test case 1: The sequence is:
0, 18, 13, 31, ...

where the second smallest element is 13.

Test case 2: The sequence is:
0, 50, 30, 80, 60, 110, 90, 140, 120, ...

where the seventh smallest element is 110.
*/

/*************
If we observe closely:
0
A
B
(B+A-0) = B+A
(B+A +B -A) = 2B
2B + B+A -B = 2B + A
2B + A + 2B - B-A = 3B
3B + 2B+A -2B = 3B+A  . . .

SO THE SEQUENCE IS-> 0, A, B, A+B, 2B, A+2B, 3B, A+3B, 4B, A+4B . . .

*/
#define ll long long
#include <bits/stdc++.h>
using namespace std;

// No of  2B, 3B, 4B terms

ll f1(ll mid, ll b) // 2B, 3B, 4B terms
{
   return (mid / b);
}

ll f2(ll mid, ll a, ll b) // A + B, A + 2B, A + 3B TERMS
{
   if (mid - a < 0)
       return 0;

   return 1 + (mid - a) / b; // +1 for the A + 0*B th term which is not counted here in (mid-a)/b;
}

// if f(i) <= k, then i is the kth smallest element in the series.
ll f(ll mid, ll a, ll b)
{
   return f2(mid, a, b) + f1(mid, b) + 1; // +1 for the 0th term where x_1 = 0;
}

int main()
{
   // your code goes here

   ll t;
   cin >> t;
   while (t--)
   {
       ll a, b, k;
       cin >> a >> b >> k;

       ll left = 0;
       ll right = 2e18;
       ll ans = 0;
       bool trigger = false;
       while (left <= right && !trigger)
       {
           ll mid = left + (right - left) / 2;

           // if f(mid) == k  && f(mid-1) < k=> mid is the kth smallest element

           if (f(mid, a, b) < k)
           {
               left = mid + 1;
           }
           else
           {
               if (f(mid - 1, a, b) >= k)
               {
                   right = mid - 1;
               }
               else
               {
                   ans = mid;
                   trigger = true;
               }
           }
       }

       cout << ans << endl;
   }
   return 0;
}

TIK TOK OA
Observation:

Given an array of integers, you can only choose one number to modify each time. You can add 1 for odd-numbered operations, and add 2 for even-numbered operations or also skip. What is the minimum number of operations you can perform to make all the elements of the array the same?
Example: [3,2,2] = 3 operations
1.[3,3,2] Add 1 to index 1
2.Skip
3.[3,3,3] Add 1 to index 2

*/

Observation 0 - All the numbers of the array can only increase.

Observation 1 - You can make all numbers in the array equal to either max number in the array or max + 1 or max + 2 or ……….infinity

->[1 1 3 3 5 5]

 = 8

Operation 2 + 4 + 6 + 8 will make (1,1) -> (5,5)

Operation 1 + 3 + 5 + 7 will make (3,3) -> (5,5)

Observation :- You should always make all the numbers equal to the max number in the array because if you make it more than that it will cost you more operations. Also max + 1 is a viable candidate because of the +1/+2 nature of operations!

Try k = 1 seat

If fails

Try k = 2 seat

If fails

Try k = 3 seat

If fails

Try k = 4 seat

If fails

Try k = 5 seat
Success!

#include <bits/stdc++.h>
using namespace std;

bool solve(vector<int> arr, int maxElement, int mid) // mid is the number of operations
{
   int n = arr.size();
   int plusTwo = 0;
   int plusOne = 0;

   if (mid % 2 == 0)
   {
       plusTwo = mid / 2;
       plusOne = mid / 2;
   }
   else
   {
       plusOne = (mid / 2) + 1;
       plusTwo = mid / 2;
   }

   for (int i = 0; i < n; i++)
   {
       if (arr[i] < maxElement)
       {
           int d = maxElement - arr[i];

           if (d % 2 == 0)
           {
               int twoUsage = (d / 2);

               if (twoUsage <= plusTwo)
               {
                   plusTwo -= twoUsage;
               }
               else
               {
                   twoUsage -= plusTwo;
                   plusTwo = 0;

                   if (twoUsage * 2 <= plusOne)
                   {
                       plusOne -= twoUsage * 2;
                   }
                   else
                   {
                       return false;
                   }
               }
           }
           else
           {
               if (plusOne >= 1)
               {
                   plusOne--;
                   d--;

                   int twoUsage = (d / 2) - 1;

                   if (twoUsage <= plusTwo)
                   {
                       plusTwo -= twoUsage;
                   }
                   else
                   {
                       twoUsage -= plusTwo;
                       plusTwo = 0;

                       if (twoUsage * 2 <= plusOne)
                       {
                           plusOne -= twoUsage * 2;
                       }
                       else
                       {
                           return false;
                       }
                   }
               }
               else
               {
                   return false;
               }
           }
       }
   }
   return true;
}

int main()
{
   vector<int> arr = {3, 2, 2};
   // vector<int> arr = {1, 1, 3, 3, 5, 5};

   sort(arr.begin(), arr.end());

   int maxElement = *max_element(arr.begin(), arr.end());

   int left = 0; // when all elements are same
   int right = maxElement * 2;
   int ans = INT_MAX;
   while (left <= right)
   {
       int mid = left + (right - left) / 2;

       // f f f t t t t t t t t
       // after certain number of operations, all other nunber of operations are possible
       // find the first occurrence of true

       if (solve(arr, maxElement, mid) || solve(arr, maxElement + 1, mid))
       {
           ans = min(ans, mid);
           right = mid - 1;
       }
       else
       {
           left = mid + 1;
       }
   }
   cout << ans << endl;
   return 0;
}

Rippling OA

/*
There are n processes to be executed, each with a respective size given by processSize[i] for 0≤i<n. Additionally, there are m processors, each with a different size capacity given by capacity[j] for 0≤j<m.

A processor can execute a task in 1 second if the task size is less than or equal to its capacity; otherwise, it cannot execute the task. A processor can execute multiple processes one after the other, but needs to pause for 1 second after completing each one. Multiple processors can work on different tasks simultaneously.

Determine the minimum time required to execute all the processes, or return -1 if it is not possible to execute them all.

Example

It is given that:

n = 3

processSize = [2, 5, 3]

m = 3

capacity = [6, 2, 4]

The optimal way to assign processes is to give the first processor the second process, the second processor the first process, and the third processor the third process. All of them complete their processes in 1 second. Therefore, the minimum time required is 1 second.
*/
#include <bits/stdc++.h>
using namespace std;

bool check(int mid, vector<int> a, vector<int> b)
{
   int n = a.size();
   int m = b.size();

   if (mid * m < n) // not possible when there are too many processes and to less processors and the given time is less
   {
       return false;
   }

   for (int i = 0; i < n; i++)
   {
       int j = i / mid;

       if (a[i] > b[j])
           return false;
   }
   return true;
}

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   vector<int> a(n);
   vector<int> b(m);

   for (int i = 0; i < n; i++)
   {
       cin >> a[i];
   }

   for (int i = 0; i < m; i++)
   {
       cin >> b[i];
   }

   sort(a.rbegin(), a.rend());
   sort(b.rbegin(), b.rend());
   int ans = INT_MAX;
   if (b[0] >= a[0])
   {
       int left = 1;
       int right = n;

       while (left <= right)
       {
           int mid = left + (right - left) / 2;
           // F F F F T T T T T T T T
           if (check(mid, a, b) == true)
           {
               ans = min(ans, mid);
               right = mid - 1;
           }
           else
           {
               left = mid + 1;
           }
       }
   }
   else
   {
       cout << -1 << endl;
   }
   cout << (ans * 2) - 1 << endl; // *2 because we need to to calculate +1 second for the processor to rest and -1 because an extra +1 is calculated in the end
   return 0;
}

AMAZON OA

/*

Amazon operates a system of n warehouses, each represented by warehouse[i], where warehouse[i] indicates the maximum number of items that particular warehouse can hold. Additionally, there are q shipments to process, represented by a 2D array catalog[q][2]. Each shipment has specific requirements:

catalog[i][0] denotes the minimum capacity that the selected warehouse must have to accommodate the shipment.

catalog[i][1] denotes the minimum combined capacity required from the other warehouses to fulfill backup storage needs.

Amazon can increase the capacity of any warehouse by spending 1 token per unit of capacity.

The task is to determine the optimal strategy for allocating capacity for each shipment such that the fewest number of tokens are expended. This strategy must ensure that the selected warehouse meets the required capacity for the shipment and that the combined capacity of the other warehouses is sufficient for backup storage.

Additional Conditions

The tokens are spent independently for each shipment.

If warehouse i is selected to accommodate the shipment and there is some leftover capacity, i.e., warehouse[i] - catalog[i][0] > 0, then it cannot be used for backup storage.

Example

Input:

n = 4
warehouse = [2, 4, 1, 3]

q = 1
catalog = [[5, 7]]

Explanation:

If the first warehouse is expanded for the shipment, 3 tokens are needed for the first warehouse to accommodate the shipment, and no tokens are needed for the rest of the warehouses to maintain backup storage. Hence, the total tokens needed = 3 + 0 = 3.

If the second warehouse is expanded for the shipment, 1 token is needed for the second warehouse to accommodate the shipment, and 1 token is needed for the rest of the warehouses to maintain backup storage. Hence, the total tokens needed = 1 + 1 = 2.

If the third warehouse is expanded for the shipment, 4 tokens are needed for the third warehouse to accommodate the shipment, and no tokens are needed for the rest of the warehouses to maintain backup storage. Hence, the total tokens needed = 4 + 0 = 4.

If the fourth warehouse is expanded for the shipment, 2 tokens are needed for the fourth warehouse to accommodate the shipment, and no tokens are needed for the rest of the warehouses to maintain backup storage. Hence, the total tokens needed = 2 + 0 = 2.

Therefore, the most optimal solution is to expand either the second or fourth warehouse for the shipment. Hence, the answer is [2].

Case(1) :-> current > x1 -> c1 = 0 -> c2 = y1-remaining_sum; -> select the smallest number in array > x1 so that remaining sum can be increased and c2 can be decreased;

Case(2) :-> 2.1 -> current<=x1 && remaining_sum<=y1 -> cost = ((x1+y1)-sum) -> only when x1+y1>=sum;

            :-> 2.2 -> current<=x1 && remaining_sum>y1

            -> you decide a prefix of values in the sorted array which are eligible to be curr<=x1

            -> now using linear search or (optimal) binary search -> find the largest number in the prefix range [1….i] -> such that when it is selected remaining_sum still remains > y1

*/

#include <bits/stdc++.h>
using namespace std;

int upperBound(vector<int> a, int x)
{
   int n = a.size();
   int left = 0;
   int right = a[n - 1];
   int ans = -1;
   while (left <= right)
   {
       int mid = left + (right - left) / 2;
       if (a[mid] > x)
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

int lowerBound(vector<int> a, int x)
{
   int n = a.size();
   int left = 0;
   int right = a[n - 1];
   int ans = -1;
   while (left <= right)
   {
       int mid = left + (right - left) / 2;

       if (a[mid] <= x)
       {
           ans = mid;
           left = mid + 1;
       }
       else
       {
           right = mid - 1;
       }
   }
   return ans;
}

int main()
{
   int n;
   cin >> n;
   vector<int> warehouse(n);
   int sum = 0;
   for (int i = 0; i < n; i++)
   {
       cin >> warehouse[i];
       sum += warehouse[i];
   }
   sort(warehouse.begin(), warehouse.end());

   int q;
   cin >> q;
   int coins = INT_MAX;
   for (int i = 0; i < q; i++)
   {
       int x, y;
       cin >> x >> y;
       int c = 1e8;
       int ub = upperBound(warehouse, x);

       // case1:
       if (ub != -1) // there exists a number > x so no coins need to be used
       {
           int remainingSum = sum - warehouse[ub];

           if (remainingSum < y) // now coins will be required
           {
               c = min(c, abs(remainingSum - y));
           }
       }

       // case 2:
       int lb = lowerBound(warehouse, x);
       if (lb != -1)
       {
           int remainingSum = sum - warehouse[lb];

           if (remainingSum <= y && warehouse[lb] <= x)
           {
               c = min(c, abs(sum - (x + y)));
           }
       }

       if (lb != -1)
       {
           int left = 1, ans = -1, right = lb;
           while (left <= right)
           {
               int mid = (left + right) / 2;
               int remainingSum = sum - warehouse[mid];
               if (remainingSum > y)
               {
                   ans = mid;
                   left = mid + 1;
               }
               else
               {
                   right = mid - 1;
               }
           }
           if (ans != -1)
           {
               c = min(c, x - warehouse[ans]);
           }
       }
       cout << c << endl;
   }
   return 0;
}

LEETCODE HARD:

![[Binary Search Documentation-image2.png]]
[https://leetcode.com/problems/smallest-substring-with-identical-characters-ii/description/](https://leetcode.com/problems/smallest-substring-with-identical-characters-ii/description/)

CODECHEF High rated problem: Ranged based binary search:

Question Link: [https://www.codechef.com/problems/ALTUNI](https://www.codechef.com/problems/ALTUNI)

Here, we cannot simply use binary search because it is not a monotonic function. It gives something like:
F F F T T F F F F T F F F T T F F F F . . . . .
So, we use a range based check() function where check(i) says if there is any True event happening from 0 to i.
We keep a track of the highest value and the lowest value mid and attain using maxi and mini variables. In cases where the highest value of mid also reaches <0, it will be false.

#define ll long long
#include <bits/stdc++.h>
using namespace std;

bool check(ll mid, const vector<ll> &a, const vector<ll> &b)
{
   ll n = a.size();
   ll maxi = mid, mini = 0;

   for (int i = 0; i < n; i++)
   {
       if (maxi > a[i])
       {
           if (mini > a[i])
           {
               maxi--;
               mini--;
           }
           else
           {
               maxi = max(maxi, a[i] + b[i]); // Other than maxi, one possible solution is a[i]+b[i] in test cases where a = {5,0,0,0,0,0,0,0} and b={100,0,0,0,0,0,0,0}
               maxi--;
               mini--;
           }
       }
       else
       {
           maxi = maxi + b[i];
           maxi--;
           mini--;
       }

       if (maxi < 0)
       {
           return false;
       }
       else
       {
           if (mini < 0)
           {
               mini = 0; // because keeping mini < 0 is useless
           }
       }
   }
   return true;
}

int main()
{
   // your code goes here

   int t;
   cin >> t;
   while (t--)
   {
       ll n;
       cin >> n;

       vector<ll> a(n), b(n);

       for (int i = 0; i < n; i++)
       {
           cin >> a[i];
       }
       for (int i = 0; i < n; i++)
       {
           cin >> b[i];
       }

       ll low = 0;
       ll high = 1e18;
       ll ans = high;
       while (low <= high)
       {
           ll mid = low + (high - low) / 2;

           //  F F F T T T T T
           // We need to find the first occurrence of true

           if (check(mid, a, b) == true)
           {
               ans = min(ans, mid);
               high = mid - 1;
           }
           else
           {
               low = mid + 1;
           }
       }
       cout << ans << endl;
   }
}

Google OA

/*
Q) You are given the following:
- An array A of length N
- Two numbers M and L
You can perform the given queries on this array A. In each query:
- Pick any continuous subarray of array A of length at most L and decrement the value of all its elements by 1
- You can perform this operation at most M times.

Approach: Binary Search + Greedy + Difference Array
1. solve() function: Binary Search
We binary search for the minimum possible maximum element of the array.
- mid is our guessed maximum value.
- If we can achieve this value using at most M operations, we try a smaller value.
- Otherwise, we try a larger value.
2. check() function: Check if mid is possible
- For every element, calculate how much it exceeds mid.
- If A[i] is greater than mid, we need to decrease it by A[i] - mid. Otherwise, no reduction is needed.
- Store these required reductions in array b.
- Call countOperations() to calculate the minimum number of operations required.
- If the required operations are at most M, return true; otherwise, return false.
3. countOperations() function: Calculate the operations
- Traverse array b from left to right.
- Use a difference array c to track the reductions affecting each position without updating every element individually.
- If the current element still needs to be reduced, perform the required number of operations on subarrays starting at that position, each of length at most L.
- Update the difference array to track where these reductions begin and stop.
- Add the operations performed to the total and return that total.
*/
#include <bits/stdc++.h>
using namespace std;

// count the number of operations here
int count(vector<int> b, int length)
{
   int n = b.size();
   int operations = 0;
   vector<int> c(n * 2 + 1, 0);
   // taking care of the 0th index
   if (b[0] > c[0])
   {
       int diff = abs(b[0] - c[0]);
       c[0] += diff;
       c[length] -= diff;
       operations += diff;
   }

   // over here, updating the prefix sum using range update trick and also calculating and applying changes along with it in single for loop
   for (int i = 1; i < n; i++)
   {
       c[i] = c[i] + c[i - 1];
       if (b[i] > 0)
       {
           if (b[i] > c[i])
           {
               int diff = abs(c[i] - b[i]);
               operations += diff;
               c[i] += diff;
               c[i + length] -= diff;
           }
       }
   }
   return operations;
}

// check if mid can be the largest minimum element in the array for given moves and subarray length
bool check(int mid, vector<int> arr, int moves, int length)
{
   int n = arr.size();
   vector<int> b(n * 2 + 1, 0);
   for (int i = 0; i < n; i++)
   {
       if (arr[i] >= mid)
       {
           b[i] = arr[i] - mid;
       }
   }
   // count the number of operations needed
   int cnt = count(b, length);

   if (cnt <= moves)
       return true;
   return false;
}

int solve(const vector<int> &arr, int moves, int length)
{
   int n = arr.size();
   int maxi = *max_element(arr.begin(), arr.end());
   int mini = *min_element(arr.begin(), arr.end());

   int left = mini - moves;
   int right = maxi;
   int ans = INT_MAX;
   while (left <= right)
   {
       int mid = left + (right - left) / 2; // mid is the minimum value possible
       // F F F T T T T T . . .
       // find the first occurrence of true

       if (check(mid, arr, moves, length))
       {
           ans = min(ans, mid);
           right = mid - 1;
       }
       else
       {
           left = mid + 1;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {4, 4, 3, 4, 1};
   int moves = 4;
   int length = 2;

   cout << solve(arr, moves, length) << endl;

   return 0;
}
```
