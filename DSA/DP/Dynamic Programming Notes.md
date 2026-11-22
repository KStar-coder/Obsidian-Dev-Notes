<https://leetcode.com/problems/burst-balloons/submissions/1170332059/>
We learn DP using arrays and loops first instead of jumping to recursion

- Kumar K

**Doubt: Q11, Q12,  Q13, Q15, Q19,**
**Revisit: Q19, Q24**

### 1) Select maximum sum subset from the two arrays such that no two elements are…

Select maximum sum subset from the two arrays such that no two elements are consecutive

Note : for each step, suppose you select a[i], you cannot select a[i-1], as well as b[i-1]

Solution Description: We first precompute the dp[0] and dp[1]

Then we run a loop from 2 to n where

dp[i] = max(dp[i - 1], max(a[i] + dp[i - 2], b[i] + dp[i - 2]));

Our final ans is stored in dp[n-1]

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();

   vector<int> dp(n);

   // precomputing the first 1-2 required values
   dp[0] = max(a[0], b[0]);
   dp[1] = max(dp[0], max(a[1], b[1]));

   for (int i = 2; i < n; i++)
   {
       dp[i] = max(dp[i - 1], max(a[i] + dp[i - 2], b[i] + dp[i - 2]));
   }
   return dp[n - 1];
}

int main()
{
   vector<int> a = {2, 3, 4, -8, 2};
   vector<int> b = {-5, 8, 3, 1, -4};
   cout << solve(a, b) << endl;
   return 0;
}
```

### 2) Given an array arr, arr[0] -> source and arr[n-1] -> destination

You can make 1 jump to arr[i+1] or 3 jumps to arr[i+3];
Cost of a jump = abs(arr[i]-arr[wherever u jumped]). Find the minimum cost from src to destination

```cpp
#include <bits/stdc++.h>
   using namespace std;

   int solve(vector<int> &arr)
   {
      int n = arr.size();
      vector<int> dp(n);
      dp[0] = 0;                            // source
      dp[1] = abs(arr[0] - arr[1]);         // 1st place so can't make a jump
      dp[2] = abs(arr[1] - arr[2]) + dp[1]; // 2nd place so can't make a jump

      // now we can make a jump
      for (int i = 3; i < n; i++)
      {
          int x = abs(arr[i] - arr[i - 1]) + dp[i - 1];
          int y = abs(arr[i] - arr[i - 3]) + dp[i - 3];
          dp[i] = min(x, y);
      }
      return dp[n - 1];
   }

   int main()
   {
      vector<int> arr = {4, 12, 13, 18, 10, 12};
      cout << solve(arr) << endl;
      return 0;
   }
```

### 3) AtCoder Vacation

[AtCoder Vacation](https://atcoder.jp/contests/dp/tasks/dp_c)

-> My Solution::

```cpp
/*
Problem Statement
Taro's summer vacation starts tomorrow, and he has decided to make plans for it now.
The vacation consists of
N days. For each
i (
1≤i≤N), Taro will choose one of the following activities and do it on the
i-th day:
A: Swim in the sea. Gain ai points of happiness.
B: Catch bugs in the mountains. Gain bi points of happiness.
C: Do homework at home. Gain ci points of happiness.
As Taro gets bored easily, he cannot do the same activities for two or more consecutive days.
Find the maximum possible total points of happiness that Taro gains.

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b, vector<int> &c)
{
   int n = a.size();
   vector<int> dpa(n, 0);
   vector<int> dpb(n, 0);
   vector<int> dpc(n, 0);

   // initializing the values
   dpa[0] = a[0];
   dpb[0] = b[0];
   dpc[0] = c[0];

   for (int i = 1; i < n; i++)
   {
       dpa[i] = a[i] + max(dpb[i - 1], dpc[i - 1]);
       dpb[i] = b[i] + max(dpa[i - 1], dpc[i - 1]);
       dpc[i] = c[i] + max(dpa[i - 1], dpb[i - 1]);
   }
   return max(dpa[n - 1], max(dpb[n - 1], dpc[n - 1]));
}

int main()
{
   vector<int> a = {10, 20, 30};
   vector<int> b = {40, 50, 60};
   vector<int> c = {70, 80, 90};

   cout << solve(a, b, c) << endl;

   return 0;
}
```

### 4) Given an integer “N” ; find the minimum cost to reduce it to 1

The operations you can do are as follows.-\>

i) Reduce the number by 1 in “y” dollars;

ii) Reduce the number by /7 in “x” dollars.(if number is divisible by 7)

iii) Reduce the number by /3 in “z” dollars;(if number is divisible by 3)

iv) Reduce the number by /5 in “b” dollars;(if number is divisible by 5)

-> Simple DP approach with 1d DP array
We start with 1 and build up till n, finding minimum cost to reduce each element i (1\<= i \<= n)
After each iteration,
dp[i] = min(x1, min(x2, min(x3, x4)));

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(int n, int y, int x, int z, int b)
{
   vector<int> dp(n + 1, 0);

   dp[1] = 0; // Min steps to reduce 1->1 is 0
   for (int i = 2; i <= n; i++)
   {
       int x1 = dp[i - 1] + y;
       int x2 = 1e8;
       int x3 = 1e8;
       int x4 = 1e8;
       if (i % 7 == 0)
       {
           x2 = dp[i / 7] + x;
       }

       if (i % 3 == 0)
       {
           x3 = dp[i / 3] + z;
       }

       if (i % 5 == 0)
       {
           x4 = dp[i / 5] + b;
       }

       dp[i] = min(x1, min(x2, min(x3, x4)));
   }
   return dp[n];
}

int main()
{
   int n;
   cin >> n;
   int y, x, z, b;
   cin >> y >> x >> z >> b;
   cout << solve(n, y, x, z, b) << endl;
   return 0;
}
```

### 5) Given an array , find the number of odd sum journeys and even sum journeys.

\#You are allowed to make jumps of size 1 and 2

Solution description: // We create a 2d DP where dp[i][0]-\>even journey and dp[i][1] = odd journey

```cpp
    vector<vector<int>> dp(n, vector<int>(2, 0));

Then, we add dp[0] and dp[1] as base cases as we can jump 2 steps at once. After that we iterate through the rest of the array and use the following logic:

if (arr[i] % 2 == 0) // arr[i] is even
        {
            // even journey, current element is even so the rest of the elements must be even
            dp[i][0] = dp[i - 1][0] + dp[i - 2][0];

            // odd journey, current element is even so the rest of the elements must be odd (even+odd = odd)
            dp[i][1] = dp[i - 1][1] + dp[i - 2][1];
        }
        else // arr[i] is odd
        {
            // Even journey, current element is odd so rest of the elements must be odd (odd+odd = even)
            dp[i][0] = dp[i - 1][1] + dp[i - 2][1];

            // Odd journey, current element is odd so rest of the elements must be even(odd+even = odd)
            dp[i][1] = dp[i - 1][0] + dp[i - 2][0];
        }

Solution :

/*
Given an array , find the number of odd sum journeys and even sum journeys.
#You are allowed to make jumps of size 1 and 2
*/

#include <bits/stdc++.h>
using namespace std;

void solve(vector<int> &arr)
{
   int n = arr.size();

   // We create a 2d DP where dp[i][0]->even journey and dp[i][1] = odd journey

   vector<vector<int>> dp(n, vector<int>(2, 0));

   // base cases
   // filling up the 0th index
   if (arr[0] % 2 == 0)
   {
       dp[0][0] = 1;
   }
   else
   {
       dp[0][1] = 1;
   }

   // Filling up the 1st index

   if (arr[1] % 2 == 0)
   {
       dp[1][0] = dp[0][0];
       dp[1][1] = dp[0][1];
   }
   else
   {
       dp[1][0] = dp[0][1];
       dp[1][1] = dp[0][0];
   }

   for (int i = 2; i < n; i++)
   {
       if (arr[i] % 2 == 0) // arr[i] is even
       {
           // even journey, current element is even so the rest of the elements must be even
           dp[i][0] = dp[i - 1][0] + dp[i - 2][0];

           // odd journey, current element is even so the rest of the elements must be odd (even+odd = odd)
           dp[i][1] = dp[i - 1][1] + dp[i - 2][1];
       }
       else // arr[i] is odd
       {
           // Even journey, current element is odd so rest of the elements must be odd (odd+odd = even)
           dp[i][0] = dp[i - 1][1] + dp[i - 2][1];

           // Odd journey, current element is odd so rest of the elements must be even(odd+even = odd)
           dp[i][1] = dp[i - 1][0] + dp[i - 2][0];
       }
   }
   cout << "Even Journeys:  " << dp[n - 1][0] << " And Odd journeys: " << dp[n - 1][1] << endl;
}

int main()
{
   vector<int> arr = {2, 3, 5, 8, 10};
   solve(arr);
   return 0;
}
```

### 6) Atlassian OA

Understanding of the problem:-\> You are given “B” queries;
you have to perform some of them in order ; and make your initial integer 1 as large as possible!

-> + x
-> - x
-> \* x
->/ x
->N ( turning the number to negative )

-> Solution description:  Here, we keep a tab of both maximum as well as minimum values using dpMax and dpMin arrays. We need to keep a track of maximum values as well because there might arise N in the query and that -ve value will turn into a positive value. This value might then become the largest positive value.

Code:

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(int queries)
{
   vector<int> dpMax(queries);
   vector<int> dpMin(queries);

   for (int i = 0; i < queries; i++)
   {
       char ch;
       cin >> ch;
       int x;
       cin >> x;
       if (ch == '+')
       {
           dpMax[i] = max(dpMax[i - 1] + x, dpMax[i - 1]);
           dpMin[i] = min(dpMin[i - 1] + x, dpMin[i - 1]);
       }
       else if (ch == '-')
       {
           dpMax[i] = max(dpMax[i - 1], dpMax[i - 1] - x);
           dpMin[i] = min(dpMin[i - 1], dpMin[i - 1] - x);
       }

       else if (ch == '*')
       {
           dpMax[i] = max(dpMax[i - 1] * x, dpMax[i - 1]);
           dpMin[i] = min(dpMin[i - 1] * x, dpMin[i - 1]);
       }

       else if (ch == '/')
       {
           dpMax[i] = max(dpMax[i - 1] / x, dpMax[i - 1]);
           dpMin[i] = min(dpMin[i - 1] / x, dpMin[i - 1]);
       }
       else
       {
           dpMax[i] = max(-1 * dpMax[i - 1], max(dpMax[i - 1], -1 * dpMin[i - 1]));
           dpMin[i] = min(-1 * dpMax[i - 1], max(dpMax[i - 1], -1 * dpMin[i - 1]));
       }
   }
   return dpMax[queries - 1];
}

int main()
{
   int queries; // no of queries
   cin >> queries;
   cout << solve(queries) << endl;

   return 0;
}
```

### 7) Google OA 3 cities

```cpp
/*
Understanding :-> You are given 3 arrays ; travel from start to end; maximize maximum dollars ;
Whenever you try to jump from Array “A” to Array “B” you make no money :)
*/
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b, vector<int> &c)
{
   int n = a.size();

   vector<vector<int>> dp(n, vector<int>(3, 0));

   // base case for 0th and 1st index as we need [i-2] in the loop
   dp[0][0] = a[0];
   dp[0][1] = b[0];
   dp[0][2] = c[0];

   dp[1][0] = a[1] + (dp[0][0]);
   dp[1][1] = b[1] + (dp[0][1]);
   dp[1][2] = c[1] + (dp[0][2]);

   for (int i = 2; i < n; i++)
   {
       dp[i][0] = a[i] + max(dp[i - 1][0], max(dp[i - 2][1], dp[i - 2][2]));

       dp[i][1] = b[i] + max(dp[i - 1][1], max(dp[i - 2][0], dp[i - 2][2]));

       dp[i][2] = c[i] + max(dp[i - 1][2], max(dp[i - 2][0], dp[i - 2][1]));
   }
   return max(dp[n - 1][0], max(dp[n - 2][1], dp[n - 2][2]));
}

int main()
{
   vector<int> a = {5, 1, 1000, 1, 100};
   vector<int> b = {1, 100, 2, 3, 1};
   vector<int> c = {21, 1, 10, 100, 56};
   cout << solve(a, b, c) << endl;
   return 0;
}
```

### 8) Amazon MAX Goodness

```cpp
/*
Given an array ; pick any subset of numbers(minimum size 1) ;
such that their goodness is maximized. (Numbers can be positive as well as -ve;)
To go from x—>y costs :-  y - x; whole cost of journey is goodness calculated.

Goodness :- Sum of adjacent differences of the numbers in set

-> [5 10 1 5 100]

-> Output : 99.

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();
   vector<int> dp(n, 0);
   vector<int> maxx(n, 0);
   int maxi = maxx[0];
   dp[0] = 0; // as we cannot subtract the first element from anything
   maxx[0] = dp[0] - arr[0];
   for (int i = 1; i < n; i++)
   {
       dp[i] = max(dp[i], arr[i] + maxi);
       maxx[i] = dp[i] - arr[i];
       maxi = max(maxi, maxx[i]);
   }

   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       ans = max(ans, dp[i]);
   }

   return ans;
}

int main()
{
   vector<int> arr = {5, 10, 1, 5, 100};
   cout << solve(arr) << endl;

   return 0;
}
```

### 9) No of palindromic substrings

[No of palindromic substrings](https://leetcode.com/problems/palindromic-substrings/description/)

Dp[i][j] = 1 if it is a palindrome from [i…j]
Else , it is 0;

We first calculate the no of palindromes for length = 1 and length = 2 manually,
Then we run 2 loops to calculate the no of palindromes for length 3 to n

We do this as :

```cpp
int length = 3;
 while (length <= n)
    {
        int i = 0;
        while (i < n - length + 1)
        {
            int j = i + length - 1; // checking substring of size = length

       // inner part of a palindrome is also a palindrome, so, we use that property to calculate the outer palindromes from length 3 -> n
            if (s[i] == s[j] && dp[i + 1][j - 1] == 1)
            {
                dp[i][j] = 1;
                cntPal++;
            }
            i++;
        }
        length++;
    }
```

-> [My Solution](https://leetcode.com/problems/palindromic-substrings/submissions/1166499919/)

### 10) Longest

[Longest](https://www.geeksforgeeks.org/problems/longest-palindrome-in-a-string1956/1?itm_source=geeksforgeeks&itm_medium=article&itm_campaign=bottom_sticky_on_article) Palindromic Substring

-> Same like previous, Here, ans stores first longest palindrome occurred with size 1 or 2, ans 2 store from length = 3 onwards.
We use a map {largest length, substring with largest length} to keep a track of the largest palindromic string from length 3 to n

```cpp
/*
Given a string s, find the longest palindromic subsequence and print it
*/

#include <bits/stdc++.h>
using namespace std;

string longestPalindrome(string s)
{
   // code here
   int n = s.size();
   vector<vector<int>> dp(n, vector<int>(n, 0));
   int maxi = 0;

   int trigger = 0;

   string ans;
   string ans2;
   // for string with length 1
   for (int i = 0; i < n; i++)
   {
       dp[i][i] = 1;
       maxi = 1;
       if (trigger == 0)
       {
           ans = s[i];
           trigger = 1;
       }
   }
   // for string with length 2
   for (int i = 0; i < n; i++)
   {
       if (s[i] == s[i + 1])
       {
           dp[i][i + 1] = 1;
           maxi = 2;
           if (trigger == 1)
           {
               ans = s.substr(i, 2);
               trigger = 0;
           }
       }
   }

   // length 3 to n
   unordered_map<int, string> mp; // {length of palindrome, string}
   for (int length = 3; length <= n; length++)
   {

       for (int i = 0; i < n - length + 1; i++)
       {
           int j = i + length - 1;
           if (s[i] == s[j] && dp[i + 1][j - 1] == 1)
           {
               dp[i][j] = 1;
               maxi = max(maxi, length);
               if (mp.find(maxi) == mp.end())
               {
                   ans2 = s.substr(i, length);
                   mp[length] = ans2;
               }
           }
       }
   }
   if (maxi <= 2)
   {
       ans2 = ans;
   }
   else
   {
       ans2 = mp[maxi];
   }
   return ans2;
}

int main()
{
   string s;
   cin >> s;
   string ans = longestPalindrome(s);
   cout << ans << endl;
   return 0;
}
```

### 11) Burst Balloons

[Burst Balloons](https://leetcode.com/problems/burst-balloons/description/)

-> [My Solution](https://leetcode.com/problems/burst-balloons/submissions/1170332059/)

Here, we will use a 1 based indexing, Rest of the code is self explanatory

### 12) Microsoft OA

Dp[i][j] =  maximum number of moves we can make from the index i and j

In MS OA;  what will be the meaning of dp[i][j] =\> maximum number of moves which can be performed on the array from “i” to “j”

-> Whichever first move you make; whatever is the sum of that ; the same sum is to be achieved in all the future operations!

-> Which are the first possible moves.?

-> 1st + last =\> sum1
->1st + 2nd =\> sum2.
->2nd last + last =\>sum3.

->There can only be exactly 3 different sum for all the operations to be performed.

dp[i][j][sum1] =\> maximum number of moves which can be performed on the array from “i” to “j” such that all operations have sum as sum1.

dp[i][j][sum2] =\> max number of operations from i to j having sum as "sum2"
dp[i][j][sum3] = max number of operations from i to j having sum as "sum3"

My Solution :

```cpp
/*
Understanding :-> We will be given an array ; you have to perform some operations on the array
-> select last 2 elements.
-> select first two elements.
-> select the last + first element

-> sum of both elements in each move should be same.

->How many maximum moves you can perform.

[1 5 6 1 5 1 5]

-> 3. (1+5⇒6.)

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();
   vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(n + 1, vector<int>(4, 0)));

   // calculating the 3 possibilities
   int sum1 = arr[0] + arr[1];
   int sum2 = arr[0] + arr[n - 1];
   int sum3 = arr[n - 1] + arr[n - 2];

   // calculating for length = 2;

   for (int i = 0; i < n; i++)
   {
       int sum = arr[i] + arr[i + 1];
       int move = 1;

       if (sum == sum1)
       {
           dp[i][i + 1][1] = max(move, dp[i][i + 1][1]);
       }

       if (sum == sum2)
       {
           dp[i][i + 1][2] = max(move, dp[i][i + 1][2]);
       }

       if (sum == sum3)
       {
           dp[i][i + 1][3] = max(move, dp[i][i + 1][3]);
       }
   }

   // for length = 3 onwards (2nd index)
   for (int length = 2; length <= n; length++)
   {
       for (int start = 0; start < n - length + 1; start++)
       {
           int end = start + length - 1;
           int move = 1;

           // First 2 elements
           int sum = arr[start] + arr[start + 1];

           if (sum == sum1)
           {
               dp[start][end][1] = max(move + dp[start + 2][end][1], dp[start][end][1]);
           }

           if (sum == sum2)
           {
               dp[start][end][2] = max(move + dp[start + 2][end][2], dp[start][end][2]);
           }
           if (sum == sum3)
           {
               dp[start][end][3] = max(move + dp[start + 2][end][3], dp[start][end][3]);
           }

           // Last 2 elements
           sum = arr[end] + arr[end - 1];

           if (sum == sum1)
           {
               dp[start][end][1] = max(move + dp[start][end - 2][1], dp[start][end][1]);
           }

           if (sum == sum2)
           {
               dp[start][end][2] = max(move + dp[start][end - 2][2], dp[start][end][2]);
           }

           if (sum == sum3)
           {
               dp[start][end][3] = max(move + dp[start][end - 2][3], dp[start][end][3]);
           }

           // first and last element
           sum = arr[start] + arr[end];

           if (sum == sum1)
           {
               dp[start][end][1] = max(move + dp[start + 1][end - 1][1], dp[start][end][1]);
           }

           if (sum == sum2)
           {
               dp[start][end][2] = max(move + dp[start + 1][end - 1][2], dp[start][end][2]);
           }

           if (sum == sum3)
           {
               dp[start][end][3] = max(move + dp[start + 1][end - 1][3], dp[start][end][3]);
           }
       }
   }
   return max(dp[0][n - 1][1], max(dp[0][n - 1][2], dp[0][n - 1][3]));
}

int main()
{
   vector<int> arr = {1, 5, 6, 1, 5, 1, 5};
   cout << solve(arr) << endl;
   return 0;
}
```

### 13) Google K partition m sum

[Google K partition m sum](https://docs.google.com/document/d/1-6Wz38cZtW5R616MT2c9qy80R7YfOaiGu3kGtBl7zYM/edit)

Given an array of size ‘N’; find the number of ways to partition it such that the sum of each part is \<=M;
but you should only make k partitions!

-> dp[i] = you fix the last number “i” + dp[i-1] (the number ways to divide the the start part i-1 of size i-1)

You can fix the last 2 numbers “i, i-1” + dp[i-2]
.
.
.
You can fix the last i-1 numbers + dp[1]
…
.
Fix all the i numbers + dp[0]
-> My Code :

```cpp
/*
Given an array of size ‘N’; find the number of ways to partition it such that the sum of each part is <=M;
but you should only make k partitions!
*/
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> arr, int m, int k)
{
   int n = arr.size();

   // assigning negative values because for subarray of size n, we cannot to more than n partitions,
   // so after the final dp calculation, these values will be highly negative and not be considered
   // by the algorithm we will write
   // dp[i][k] = number of ways to do partitions till index “i” but the numbers of partitions are exactly k

   vector<vector<int>> dp(n + 1, vector<int>(k, -1e9));

   // base cases
   dp[0][0] = 1; // for 0 size partition, there is the default array so 1 way
   dp[1][1] = 1; // for 1 partition, no of ways = 1;

   /*
   dp[i] = you fix the last number “i” + dp[i-1] (the number ways to divide the the start part i-1 of size i-1)

       You can fix the last 2 numbers “i i-1” + dp[i-2]
       .
       .
       .
       You can fix the last i-1 numbers + dp[1]
       …
       .
       Fix all the i numbers + dp[0]

   */

   for (int i = 2; i <= n; i++)
   {
       int partition = 1;
       while (partition <= k)
       {
           int j = i;
           int sum = arr[i];
           int v = 0;
           while (j > 1 && sum <= m)
           {
               int g = dp[j - 1][partition - 1];
               if (g > 0)
               {
                   v += g;
               }
               j--;
               sum += arr[j];
           }

           if (v > 0)
           {
               dp[i][partition] = v;
           }

           partition++;
       }
   }
   return dp[n][k];
}

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   int k;
   cin >> k;
   vector<int> arr(n + 1); // 1 based indexing

   for (int i = 0; i < n; i++)
   {
       cin >> arr[i];
   }

   cout << solve(arr, m, k) << endl;

   return 0;
}
```

### 15) Leetcode Partition Binary String

[Leetcode Partition Binary String](https://leetcode.com/contest/biweekly-contest-108/problems/partition-string-into-minimum-beautiful-substrings/)

Understanding :-\> Given a binary string ; divide it into some parts such that each part is a power of 5(5^0 or 5^1 or 5^2 or……)
And the number of parts should be as minimum as possible. :)

According to Nayan Law:-\>
dp[i] = minimum number of partitions such that size of string is i
Dp[0] = when there is no string there is no partition.
Dp[i] = (1 + dp[i-1] ) (only possible if “i” is a power of 5)
OR
(1 + dp[i-2]) (only possible if according to Nayan’s law i and i-1 is a power of 5)
OR
(1 + dp[i-3])
OR
(1 + dp[i-4])
OR
..
..
..
..
..
1 + dp[0] …..(the full string must be power of 5)
Answer is the minimum of all these.

How can we efficiently check if binary string is power of 5

If any number is divisible by 15625 then it is power of 5, because, according to the constraints, largest string is of size 15,
111111111111111 ; last number closest to this that is divisible by 5 is 15625, hence, the conclusion.
-> [My Solution](https://leetcode.com/submissions/detail/1173244813/)

### 16) Cisco Ideathon

dp[i] = minimum number of partitions if array is of size “i” ; all the partitions are of size\>=K and all the partitions have max - min \<=M

Dp[i] = 1 + dp[i-1] →(1\>=K) (big-small\>=m)

OR

1 + dp[i-2] -> (2\>=K) (b-s\>=m)

OR

1 + dp[i-3] -> (3\>=K)

OR
….

1 + dp[0] -> (i\>=K)

Understanding :-\> Given an array of Size “N”; divide it into some parts; such that each part is of size\>=K
and the max - min of that part should be \<=M. The number of parts must be minimum

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k, int m)
{
   int n = arr.size();

   vector<int> dp(n, 1e8);

   dp[0] = 0;

   for (int i = 1; i < n; i++)
   {
       int big = arr[i];
       int small = arr[i];
       for (int j = i - 1; j >= 0; j--)
       {
           if (big - small <= m && i - j + 1 >= k)
           {
               dp[i] = min(dp[i], dp[j] + 1);
           }
           big = max(big, arr[j]);
           small = min(small, arr[j]);
       }
   }
   return dp[n - 1];
}

int main()
{
   vector<int> arr = {4, 5, 5, 3, 2, 6};
   int m = 3;
   int k = 4;
   cout << solve(arr, k, m) << endl;
   return 0;
}
```

### 17) Google OA Server

-> My code

```cpp
/*
   array a[i]: cost of stand alone server a[i]
   array b[i]: cost of the server if it is joined with the adjacent server: b[i] + b[i-1]

   find the minimum cost of utilizing all the servers
*/

#include <bits/stdc++.h>

using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();
   /*
   For dp[0] and dp[1] we can calculate manually,
   But, from dp[3] onwards, we need to start applying partition dp as many possibilities will arise
   */
   vector<int> dp(n);

   dp[0] = a[0];
   dp[1] = min(a[1], b[0] + b[1]);

   for (int i = 2; i < n; i++)
   {
       int individual = a[i] + dp[i - 1]; // cost of ith individual server + the best possible ans before i
       int joined = b[i] + b[i - 1];      // maintaining a running sum of the connected server's cost
       for (int j = i - 1; j >= 0; j--)
       {
           int current_join = joined + dp[j - 1];      // calculating the current sum for joining servers
           individual = min(individual, current_join); // individual will store the min of joining and individual servers
           j--;
           joined = joined + b[j];
       }
       dp[i] = individual; // since individual stores the lowest possible
   }
   return dp[n - 1];
}

int main()
{
   vector<int> a = {3, 5, 2, 1, 9};
   vector<int> b = {1, 1, 10, 5, 3};
   cout << solve(a, b) << endl;
   return 0;
}

Optimized solution :
Dp[i][a] : best ans till the ith element when the last element is from the ath category

Dp[i][a] = a[i] + min (dp[i-1][a], dp[i-1][b])

Dp[i][b2] : best ans till the ith element when atleast the last 2 elements are from the bth category.

Dp[i][b2] = b[i] + b[i-1] + min (dp[i-2][a], dp[i-2][b1])

Dp[i][b1] : it helps to calculate dp[i][b2]. Dp[i][b2] takes into consideration only even number of occurrences of joining cases which is eliminated by dp[i][b1].
Dp[i][b1] : b[i] + min (dp[i-1][a], dp[i-1][b1])

My code :
// Optimized code for Google OA server

/*

Optimized solution :
Dp[i][a] : best ans till the ith element when the last element is from the ath category

Dp[i][a] = a[i] + min (dp[i-1][a], dp[i-1][b2/])

Dp[i][b2] : best ans till the ith element when atleast the last 2 elements are from the bth category.

Dp[i][b2] = b[i] + b[i-1] + min (dp[i-2][a], dp[i-2][b1])

Dp[i][b1] : it helps to calculate dp[i][b2]. Dp[i][b2] takes into consideration only even number of occurrences of joining cases which is eliminated by dp[i][b1].
Dp[i][b1] : b[i] + min (dp[i-1][a], dp[i-1][b2])

*/
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();

   vector<vector<int>> dp(n, vector<int>(3, 0));

   dp[0][0] = a[0];
   dp[0][1] = b[0];
   dp[0][2] = 1e7;

   for (int i = 1; i < n; i++)
   {
       int individual = a[i] + min(dp[i - 1][0], dp[i - 1][2]);
       int joined_even = 0;
       if (i > 1)
           joined_even = b[i] + b[i - 1] + min(dp[i - 2][0], dp[i - 2][1]);
       int joined_odd = b[i] + min(dp[i - 1][0], dp[i - 1][2]);

       dp[i][0] = individual;
       dp[i][1] = joined_odd;
       dp[i][2] = joined_even;
   }
   return min(dp[n - 1][0], dp[n - 1][2]);
}

int main()
{
   vector<int> a = {3, 5, 2, 1, 9};
   vector<int> b = {1, 1, 10, 5, 3};

   cout << solve(a, b) << endl;
   return 0;
}
```

### 18) Given a string s containing only a and b, find the longest substring of s…

Given a string s containing only a and b, find the longest substring of s such that s does not contain more

than two contiguous occurrences of a and b.
Meaning : whenever 'a' comes in continuous fashion, a must come only 2 times
and whenever 'b' comes in continuous fashion, 'b' must come only 2 times

-> My solution :

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(string s)
{
   int n = s.size();

   // we will create 4 states here ,
   // dpa1[i] = largest substring ending at i such that s[i] = 'a' but s[i-1] != 'a'
   // dpa2[i] = largest substring ending at i such that s[i] = 'a' and s[i-1] == 'a'
   // dpb1[i] = largest substring ending at i such that s[i] = 'b' but s[i-1] != 'b'
   // dpb2[i] = largest substring ending at i such that s[i] = 'b' and s[i-1] == 'b'

   vector<int> dpa1(n, 0);
   vector<int> dpa2(n, 0);
   vector<int> dpb1(n, 0);
   vector<int> dpb2(n, 0);

   int maxi = 0, ans = 0;
   // base case, initialization of the states
   if (s[0] == 'a')
   {
       dpa1[0] = 1;
   }
   if (s[0] == 'b')
   {
       dpb1[0] = 1;
   }

   for (int i = 1; i < n; i++)
   {
       if (s[i] == 'a')
       {
           dpa1[i] = 1 + max(dpb1[i - 1], dpb2[i - 1]);

           if (i > 1 && s[i - 1] == 'a')
           {
               dpa2[i] = 2 + max(dpb1[i - 2], dpb2[i - 2]);
           }
           else if (s[i - 1] == 'a') // when the string starts with "aa"
           {
               dpa2[i] = 2;
           }
       }
       else // s[i] == 'b'
       {
           dpb1[i] = 1 + max(dpa1[i - 1], dpa2[i - 1]);
           if (i > 1 && s[i - 1] == 'b')
           {
               dpb2[i] = 2 + max(dpa1[i - 2], dpa2[i - 2]);
           }
           else if (s[i - 1] == 'b') // The string starts from "bb"
           {
               dpb2[i] = 2;
           }
       }
       maxi = max(dpa1[i], max(dpa2[i], max(dpb1[i], dpb2[i])));
       ans = max(ans, maxi);
   }

   return ans;
}

int main()
{
   string s = "aabbaabbaabbaaa";
   cout << solve(s) << endl;
   return 0;
}
```

### 19) Meesho OA

Understanding -> Given an array of size “N” ; you can do a particular type of operation any
number of times(even 0) ; the final size of the array should be as small as possible!

In one operation ; you are allowed to pick any subarray which starts and ends at the same element and
remove it from array

-> If there is a bigger range covering the smaller range then the smaller range is of no use!

-> You will never remove the overlapping ranges for the final best answer

-> Final answer will be some set of ranges which are not touching each other

Solution description: Here,

dp[i][0] = best ans to the question till the ith index when u do not select i as end point of any range
dp[i][1] = best ans to the question till the ith index when u select i as an end point of a range
dp_ans[i] = best ans to the question till the ith index = min (dp[i][0] , dp[i][1])
dp[i][0] = ans[i-1] + 1; // +1 because we do not select i so it's length (1) is added
dp[i][1] = ans[x]+0; // x -> index till where the array remains after selecting this element arr[i]

To optimize this, we can eliminate a loop in our solution and use hashing to store the data of similar element {element, ans if that element is selected}

Solution :

```cpp
#include <bits/stdc++.h>
using namespace std;
int solve(vector<int> &arr)
{
   int n = arr.size();
   /*
   dp[i][0] = best ans to the question till the ith index when u do not select i as end point of any range
   dp[i][1] = best ans to the question till the ith index when u select i as an end point of a range
   dp_ans[i] = best ans to the question till the ith index = min (dp[i][0] , dp[i][1])
   dp[i][0] = ans[i-1] + 1; // +1 because we do not select i so it's length (1) is added
   dp[i][1] = ans[x]+0; // x -> index till where the array remains after selecting this element arr[i]
   */
   vector<vector<int>> dp(n, vector<int>(2, 0));
   vector<int> dp_ans(n);
   dp[0][0] = 1;   // for size 1, length = 1;
   dp[0][1] = 1e8; // for size 1, we cannot select only 1 element when we do not know if the same element is present so it is infinity
   dp_ans[0] = min(dp[0][0], dp[0][1]);

   for (int i = 1; i < n; i++)
   {
       dp[i][0] = dp_ans[i - 1] + 1;
       int j = i - 1;
       int x = INT_MAX;
       while (j >= 0) // we can use a hashmap so that we do not have to traverse this loop
       {
           if (arr[i] == arr[j])
           {
               x = min(x, dp_ans[j - 1]);
           }
           j--;
       }
       dp[i][1] = x;

       dp_ans[i] = min(dp[i][1], dp[i][0]);
   }
   return dp_ans[n - 1];
}

int optimizedSolve(vector<int> &arr)
{
   int n = arr.size();
   vector<vector<int>> dp(n, vector<int>(2, 0));
   vector<int> dp_ans(n, 0);

   dp[0][0] = 1;   // first element size = 1;
   dp[0][1] = 1e8; // for size 1, we cannot select only 1 element when we do not know if the same element is present so it is infinity
   dp_ans[0] = min(dp[0][0], dp[0][1]);

   unordered_map<int, int> mp; // {element, ans for that element so that we can directly use it}

   for (int i = 0; i < n; i++)
   {
       dp[i][0] = dp_ans[i - 1] + 1;

       int x = 1e8;
       if (mp.find(arr[i]) == mp.end()) // element not found
       {
           dp[i][1] = x;
       }
       else
       {
           x = min(dp_ans[i - 1], mp[arr[i]]);
           dp[i][1] = x;
       }

       dp_ans[i] = min(dp[i][0], dp[i][1]);

       // storing the map
       if (mp.find(arr[i]) == mp.end()) // not found
       {
           mp[arr[i]] = dp_ans[i - 1];
       }
       else
       {
           mp[arr[i]] = min(mp[arr[i]], dp_ans[i - 1]);
       }
   }
   return dp_ans[n - 1];
}

int main()
{
   vector<int> arr = {4, 2, 2, 3, 3, 1};
   cout << solve(arr) << endl;
   cout << optimizedSolve(arr) << endl;
   return 0;
}
```

### 20) Google maximum consecutive length

```cpp
/*
Problem:
Given array of positive integers of length n (n<=100000)

(a[i]<=100000)
You have to maximize the consecutive integers count in array (Order doesn't matter)
Only operation you are allowed to with array elements is to make
ai=ai+1 i.e increase the element by one  OR
ai=ai+0 i.e do nothing with element
for all 0<=i<n
E.gInput
n=9
arr:1 1 3 4 6 6 6 8 10
ans = 5
Explanation:
You can transform the array to
1 1 4 5 6 7 6 8 10
maximum consecutive length is 5 with elements 4,5,6,7,8
*/

Solution Description :
We calculate the ans for all the numbers in the constraint (1e5)

Dp[i] = best ans for the number i (1<=i<=1e5)

According to Kaustav’s law:->

g[i] =  Same as dp[i] but you are ignoring one occurrence of i from array (because that single i you will use in future to make it i+1)

If i exist in the array
dp[i] = dp[i-1] + 1

—------------------------------------
If i is not in the array

Then check if i-1 is in the array :->

dp[i] = 1(i-1→i) + g[i-1]
(g[i]= same as dp[i] but you are ignoring 1 occurrence of i)

If not :-> then.
dp[i]=0

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   unordered_map<int, int> mp; // {element, frequency} to help create the extra array
   for (int i = 0; i < n; i++)
   {
       mp[arr[i]]++ ;
   }

   //  constraints : 10^5, so we create a dp array of size 10^5 so that we calculate all the values

   vector<int> dp(1e5 + 1, 0);
   vector<int> extra(1e5 + 1, 0);

   int ans = INT_MIN;
   for (int i = 1; i <= 1e5; i++)
   {
       // calculating the dp array
       if (mp[i] >= 1)
       {
           dp[i] = dp[i - 1] + 1;
       }
       else
       {
           if (mp[i - 1] >= 1)
           {
               dp[i] = extra[i - 1] + 1  ;
           }
           else
           {
               dp[i] = 0;
           }
       }

       // calculating the 'extra' array
       if (mp[i] >= 2)
       {
           extra[i] = dp[i - 1] + 1;
       }
       else if (mp[i] == 1) // if the current number exists, then we can take 1 from it's prev number if possible
       {
           if (mp[i - 1] >= 1)
           {
               extra[i] = 1 + extra[i - 1];
           }
           else
           {
               extra[i] = 0;
           }
       }
       else
       {
           extra[i] = 0;
       }

       ans = max(ans, dp[i]);
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 1, 3, 4, 6, 6, 6, 8, 10};
   cout << solve(arr) << endl;
   return 0;
}
```

### 21) Uber OA

```cpp
/*
There are two arrays , 'A' and 'B' ; size of both the arrays is 'N' ;

-> At each index you are supposed to pick up an element either from array 'A' or array 'B'

->You have to maximize the final sum of all elements

->Condition : You cannot select 3 or more than 3 elements consecutively from the same array.

Example :

3

5 10

3 10

4 10

Output : 25

1<=N<=100000

1<=B[i];C[i]<=10000000000
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();

   // we need a 3d dp [index][selecting array a][selecting array b]
   vector<vector<vector<int>>> dp(n, vector<vector<int>>(2, vector<int>(2, 0)));

   // dp[i][0][0] : selecting the last and 2nd last from array a
   // dp[i][1][1] : selecting the last and 2nd last from array b
   // dp[i][0][1] : selecting the last from array a and 2nd last from array b
   // dp[i][1][0] : selecting the last from b and 2nd last from array a

   dp[0][0][0] = a[0];
   dp[0][0][1] = a[0];
   dp[0][1][0] = b[0];
   dp[0][1][1] = b[0];

   dp[1][0][0] = a[1] + a[0];
   dp[1][0][1] = a[1] + b[0];
   dp[1][1][0] = b[1] + a[0];
   dp[1][1][1] = b[1] + b[0];

   int ans = INT_MIN;

   for (int i = 2; i < n; i++)
   {
       // when we select the 2 elements from array a, we can select the prev from array b and a or both from array b
       dp[i][0][0] = a[i] + a[i - 1] + max(dp[i - 2][1][0], dp[i - 2][1][1]);

       // when we select one from a and one from b, we can select the prev 2 from array a, one from a one from b or one from b than next one from a
       dp[i][0][1] = a[i] + b[i - 1] + max(dp[i - 2][0][0], max(dp[i - 2][1][0], dp[i - 2][0][1]));

       // when we select one from b and then one from a, we can select prev 2 from a, one from a then one from b or one from b then one from a
       dp[i][1][0] = b[i] + a[i - 1] + max(dp[i - 2][1][1], max(dp[i - 2][1][0], dp[i - 2][0][1]));

       // when we select both from b, we can select prev 2 from a, one from a then one from b or one from b then one from a
       dp[i][1][1] = b[i] + b[i - 1] + max(dp[i - 2][0][0], dp[i - 2][0][1]);

       ans = max(ans, max(dp[i][0][0], max(dp[i][0][1], max(dp[i][1][0], dp[i][1][1]))));
   }
   return ans;
}

int main()
{
   vector<int> a = {5, 3, 4};
   vector<int> b = {10, 10, 10};
   cout << solve(a, b) << endl;
   return 0;
}
```

### 22) Modified Knapsack with 2 bags

Here, we take a 3d dp
Dp[ith item] [remaining capacity of 1st  bag] [remaining capacity of 2nd]

```cpp
/*
Same as knapsack problem but in place of 1 bag we have 2 bags
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &weight, vector<int> &price, int c1, int c2)
{
   int n = weight.size();
   vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(c1 + 1, vector<int>(c2 + 1, 0)));

   // base case has already been taken care of as all elements are assigned 0

   for (int i = 1; i < n; i++)
   {
       for (int j = 1; j <= c1; j++)
       {
           for (int k = 1; k <= c2; k++)
           {
               int notTake = dp[i - 1][j][k];
               int takeBag1 = 0;
               if (j - weight[i] > 0)
               {
                   takeBag1 = price[i - 1] + dp[i - 1][j - weight[i]][k];
               }
               int takeBag2 = 0;
               if (k - weight[i] > 0)
               {
                   takeBag2 = price[i - 1] + dp[i - 1][j][k - weight[i]];
               }
               dp[i][j][k] = max(notTake, max(takeBag1, takeBag2));
           }
       }
   }
   return dp[n][c1][c2];
}

int main()
{
   int n;
   cin >> n;
   int c1;
   cin >> c1;
   int c2;
   cin >> c2;

   vector<int> weight(n);
   vector<int> price(n);

   for (int i = 0; i < n; i++)
   {
       cin >> weight[i];
   }
   for (int i = 0; i < n; i++)
   {
       cin >> price[i];
   }

   cout << solve(weight, price, c1, c2) << endl;

   return 0;
}
```

### 23) DE Shaw DP OA

```cpp
/*
Understanding :- You are given an array of size N name cost[i] ; you are also given happy[i] -> Happiness ;
max[i]-> Maximum amount of item i you can purchase ;
min[i] -> Minimum amount of item i you can purchase ;
m-> total budget

Shop in such a way such that you get maximum happiness in the given money
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &cost, vector<int> &happy, vector<int> &min, vector<int> &max, int money)
{
   int n = cost.size();

   // dp[ith item][money left]
   vector<vector<int>> dp(n + 1, vector<int>(money, 0));

   // dp[0][0] = 0 as we cannot buy anything with 0 money
   dp[0][0] = 0;
   // initializing all the other values to INT_MIN
   for (int i = 1; i < n; i++)
   {
       for (int j = 0; j < money; j++)
       {
           dp[i][j] = INT_MIN;
       }
   }

   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= money; j++)
       {
           int finalAns = INT_MIN;

           int count = min[i - 1];
           while (count <= max[i - 1])
           {
               int totalCost = count * cost[i];
               if (j - totalCost >= 0)
               {
                   int ans = count * happy[i - 1] + dp[i - 1][j - totalCost];
                   finalAns = max(finalAns, ans);
               }
               count++;
           }
           dp[i][j] = finalAns;
       }
   }
   return dp[n][money];
}

int main()
{
   vector<int> cost = {5, 5, 6};
   vector<int> happy = {100, 5, 1};
   vector<int> max = {5, 5, 1};
   vector<int> min = {1, 2, 3};
   int money = 30;

   cout << solve(cost, happy, min, max, money) << endl;
   return 0;
}
```

### 24) Total subarrays with sum k

// given an array arr, and an integer k,  find the number of ways to get the sum k by adding the elements of arr

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   vector<vector<int>> dp(n + 1, vector<int>(k + 1, 0));
   dp[0][0] = 1; // since to get a sum 0 and choose no elements, it takes 1 way

   for (int i = 1; i <= n; i++) // since we need to calculate for [i-1] to avoid runtime i=0 is taken care of
   {
       for (int j = 0; j <= k; j++)
       {
           if (j - arr[i] >= 0)
           {
               dp[i][j] = dp[i - 1][j] + dp[i - 1][j - arr[i]];
           }
           else
           {
               dp[i][j] = dp[i - 1][j];
           }
       }
   }
   return dp[n][k];
}

int main()
{
   vector<int> arr = {3, 34, 4, 12, 5, 2};
   int k = 9;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 25) Google DE Shaw

```cpp
/*
Understanding :-> We are given “N” rows. Each row has “M” elements; we have to select 1 element from each row.

Once we are done with it final sum of all elements should be as close as possible to K
i.e return the absolute value of (final_sum - k)

N = 5
M = 2
K = 40
{
3 5
5 8
8 8
10 10
11 15
}

Output :-> Abs(sum-K) = (40-40) = 0
*/

#include <bits/stdc++.h>
using namespace std;

int solve(int n, int m, int k, vector<vector<int>> &arr)
{
   vector<vector<bool>> dp(n + 1, vector<bool>(5000 + 1, false)); // max limit of sum = 5000 (constraints)
   // dp[i][sum] : is it possible to get the sum 'sum' by using the first i rows
   dp[0][0] = true;

   for (int i = 1; i <= n; i++)
   {
       for (int sum = 1; sum <= 5000; sum++) // testing the possibility of all the sum in every row
       {
           for (int j = 1; j <= m; j++) // loop to traverse the given array
           {
               int y1 = arr[i][j]; // since loop starts from 1 but we need to access the 0th as well
               int possibility = sum - y1;

               if (possibility >= 0 && dp[i - 1][possibility] == true)
               {
                   dp[i][sum] = true;
               }
           }
       }
   }

   int ans = INT_MAX;

   // searching the last row of the dp array for the closest value of k (least abs value of difference)
   for (int sum = 1; sum <= 5000; sum++) // checking across all the values of the sum
   {
       if (dp[n][sum] == true)
       {
           int possibility = abs(sum - k);
           ans = min(ans, possibility);
       }
   }
   cout << "\n Printing the DP array  :\n";
   for (int i = 0; i < n; i++)
   {
       for (int j = 0; j < k; j++)
       {
           cout << dp[i][j] << " ";
       }
       cout << endl;
   }

   return ans;
}

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   int k;
   cin >> k;

   vector<vector<int>> arr(n + 1, vector<int>(m + 1));
   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= m; j++)
       {
           cin >> arr[i][j];
       }
   }

   cout << solve(n, m, k, arr) << endl;

   return 0;
}
```

### 26) Longest Increasing Subset

Dp[i] : longest increasing subset till the ith index
Dp[0] = 1 as every element is individually of size 1 and is valid
Solution description:  Here, we use 2 loops, the
First loop i-\> 1 to n-1
Second loop j-\> i-1 to 0 // to find all number occurring before arr[i] which are \> arr[i]

Code :

// Given an array arr, find the longest increasing subset

```cpp
#include <bits/stdc++.h>
using namespace std;
int solve(vector<int> &arr)
{
   int n = arr.size();
   // dp[i] = largest increasing subset such that the last element of the subset is a[i]
   vector<int> dp(n, 1); // as all elements individually are of size 1 and are individually valid
   dp[0] = 1;
   for (int i = 1; i < n; i++)
   {
       int current = arr[i];
       for (int j = i - 1; j >= 0; j--) // j is running backwards and finding all elements lesser than arr[i] coming before arr[i]
       {
           if (current > arr[j])
           {
               dp[i] = max(dp[i], 1 + dp[j]);
           }
       }
   }
   // now we need to find the best ans
        int ans = INT_MIN;
        for (int i = 0; i<n; i++)
        {
            ans= max (ans, dp[i]);
        }
        return ans;

}
int main()
{
   vector<int> arr = {2, 100, 4, 50, 8, 10, 12, 14, 15};
   cout << solve(arr) << endl;
   return 0;
}
```

### 27) Longest increasing subset with each subsequent element’s difference == k

Solution description : Here, the same approach of above is used except in the if condition : if (current_element - arr[j] == k) is used

We can further optimize this solution by using a hashmap where we store the best ans for a particular element in the map and access it directly by subtracting k from the current element

```cpp
if (mp.find(current - k) != mp.end())
        {
            dp[i] = max(dp[i], 1 + mp[current - k]);
        }
```

// Then we  update the map
mp[current] = max(mp[current], dp[i]);

Code :

// Given an array, find the longest increasing subset with a difference between each element as k

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   vector<int> dp(n, 1); // since all elements individually are of size 1 and they are valid
   dp[0] = 1;            // first number is one, same reason as above, no need to write this actually

   unordered_map<int, int> mp; // {element, best ans for the element}
   mp[arr[0]] = 1;             // since it is the first element so the size of Longest Increase Subset = 1;

   for (int i = 1; i < n; i++)
   {
       int current = arr[i];

       // eliminating this loop and replacing it with a map for O(N) Approach
       /*
       for (int j = i - 1; j < n; j++)
       {
           if (current - arr[j] == k)
           {
               dp[i] = max(dp[i], 1 + dp[j]);
           }
       }
       */

       if (mp.find(current - k) != mp.end())
       {
           dp[i] = max(dp[i], 1 + mp[current - k]);
       }

       // updating the map
       mp[current] = max(mp[current], dp[i]);
   }
   return dp[n - 1];
}

int main()
{
   vector<int> arr = {2, 4, 6, 8, 1, 2, 3};
   int k = 1;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 28) Find the largest subset in an array such that each element is thrice the

previous element

Like previous question, only the if statement changes :

```cpp
if (current % 3 == 0 && mp.find(current / 3) != mp.end())
        {
            dp[i] = max(dp[i], 1 + mp[current / 3]);
        }
```

// updating the map
mp[arr[i\]\] = max(mp[arr[i\]\], dp[i]);
Code :

// Find the largest subset in an array such that each element is thrice the previous element

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   unordered_map<int, int> mp; // {element, best ans }

   vector<int> dp(n, 1); // individually, the length is always 1 and is a valid subset

   // The first element has an individual length of 1
   dp[0] = 1;
   mp[arr[0]] = 1;

   for (int i = 0; i < n; i++)
   {
       int current = arr[i];

       if (current % 3 == 0 && mp.find(current / 3) != mp.end())
       {
           dp[i] = max(dp[i], 1 + mp[current / 3]);
       }

       // updating the map
       mp[arr[i]] = max(mp[arr[i]], dp[i]);
   }

   // finding the largest ans
   int ans = INT_MIN;
   for (int i = 0; i < n; i++)
   {
       ans = max(ans, dp[i]);
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 5, 15, 3, 9};
   cout << solve(arr) << endl;
   return 0;
}
```

### 29) Microsoft OA

Understanding :-\> We are given an array of size “N”;
we can select some numbers from this array and we can rearrange them in any order we want ;
whatever order you get it should be the largest possible Arithmetic sequence.

```cpp
/*
Understanding :-> We are given an array of size “N”;
we can select some numbers from this array and we can rearrange them in any order we want ;
whatever order you get it should be the largest possible Arithmetic sequence.
*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // dp[i][diff] = length of subset when we choose the arr[i]th element and the common difference is diff
   vector<vector<int>> dp(n, vector<int>(100, 1)); // given constraint : diff can be max of 100

   dp[0][0] = 1;
   int ans = INT_MIN;
   // we can sort the array as it is said that we can rearrange.
   sort(arr.begin(), arr.end());
   for (int i = 1; i < n; i++)
   {
       for (int diff = 0; diff <= 100; diff++)
       {
           int v = 0;
           for (int j = i - 1; j >= 0; j--)
           {

               if (arr[j] + diff == arr[i])
               {
                   int possible = dp[j][diff];
                   v = max(v, possible);
               }
           }
           dp[i][diff] = v + 1; // +1 because we need to add the current element size i.e 1
           ans = max(ans, dp[i][diff]);
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {18, 26, 18, 22, 24, 20, 25};
   cout << solve(arr) << endl;
   return 0;
}
```

### 30) Airbnb OA

*/\**
*There are 'n' stones in a row from 0 to n-1. For every ith stone ,*
*there are 2 values associated with it, a[i] and b[i] .*
*You have to remove all the stones from the row one by one. Removing the ith stone follows the rule :*

*If (i-1)th and (i+1)th stones are still present , then , cost of removing the ith stone is b[i].*

*if either (i-1)th or (i+1)th stone is present , then cost of removing the ith stone is a[i].*

*if neither (i-1)th nor (i+1)th stone is present , the cost of removing the ith stone is 0.*

*Find the minimum total cost of removing all the stones.*

*Constraints :*
*1 \<= n \<= 50000*
*1 \<= a[i] , b[i] \<= 1000*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
   int n = a.size();

   // dp[i] = cost of removing all the stones till the ith index assuming the ith index is the last index
   // Here, the issue is that we do not know when to pick which stone
   // so we introduce another state
   // dp[i][last] : it means min cost to solve the problem if ith index is the last index and we need to pick the ith index before picking up
   //               i-1th index.
   // dp[i][second_last] : it means minimum cost to solve the problem ith index is the last index and we need to pick i-1 before picking up the
   //               ith index.

   // dp[i][last] = a[i] + min (dp[i-1][last], dp[i-1][second_last])

   // dp[i][second_last] = b[i-1] + 0 // 0 because the adjacent of the last element is already picked

   // dp[i][0] = second_last
   // dp[i][1] = last
   vector<vector<int>> dp(n, vector<int>(2, 0));

   // for the first element, ans = 0;
   dp[0][0] = 0;
   dp[0][1] = 0;

   // if the array has only 2 elements
   dp[1][0] = a[0] + 1; // second last i.e i-1th index
   dp[1][1] = a[1] + 0; // last i.e ith index

   for (int i = 2; i < n; i++)
   {
       // last
       dp[i][1] = a[i] + min(dp[i - 1][1], dp[i - 1][0]);

       // second last
       int v1 = b[i - 1] + min(dp[i - 2][0], dp[i - 2][1]); // i-1 is picked first so b[i-1] and then we pick i-2 so calculate min of that
       int v2 = a[i - 1] + dp[i - 1][0];                    // i-2 is picked before i-1 so a[i-1] as i is still present and dp[i-1][0] as it is the second last after i-2
       dp[i][0] = min(v1, v2);
   }
   return min(dp[n - 1][0], dp[n - 1][1]);
}

int main()
{
   vector<int> a = {1, 2, 3};
   vector<int> b = {1, 2, 3};
   cout << solve(a, b) << endl;
   return 0;
}
```

### 31) Cisco OA Rook Chess

Simple counting technique and maintaining a prefix sum array

*/\**
*Given a chessboard of size "N X M" ; Rook is standing at (0,0) ; find the number of ways to reach (n,m) ;*
*if the rook is only allowed to go right or down.*

*1\<=N\<=1000*

*1\<=M\<=1000*

*Find the answer modulo 1e9 + 7.*
*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int mod = (int)1e9 + 7;
int solve(int n, int m)
{
   // dp[i][j] =  no of ways to reach from (0,0) to (i,j)
   vector<vector<int>> dp(n, vector<int>(m, 0));
   dp[0][0] = 1; // no of ways to reach the first cell = 1 as we bwgin with it

   // We will use 2 prefix arrays h and v to calculate horizontal and vertical sum will i for easy calculation of dp [i][j]
   vector<vector<int>> h(n, vector<int>(m, 0));
   vector<vector<int>> v(n, vector<int>(m, 0));
   h[0][0] = 1;
   v[0][0] = 1;
   dp[1][1] = 1;

   for (int i = 1; i < n; i++)
   {
       for (int j = 1; j < m; j++)
       {
           // down
           dp[i][j] = (dp[i][j] + v[i - 1][j]) % mod;

           // left-right
           dp[i][j] = (dp[i][j] + h[i][j - 1]) % mod;

           h[i][j] = (h[i][j - 1] + dp[i][j]) % mod;
           v[i][j] = (v[i - 1][j] + dp[i][j]) % mod;
       }
   }
   return (dp[n - 1][m - 1]) % mod;
}
int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   cout << solve(n, m) << endl;
   return 0;
}
```

### 32) A report containing the stock prices for the n days are given to a data…

A report containing the stock prices for the n days are given to a data analyst to analyze in array StockPrices[]

The analyst is required to choose a subsequent of stock prices , called ChosenDays[]

Given the condition :

StockPrice[ChosenDays[i\]\] - StockPrice[ChosenDays[j\]\] == ChosenDays[i] - ChosenDays[j]
which means arr[i]-arr[j] = i - j
So, to simplify,
arr[i] - arr[j] = i - j
=\>  arr[i] - i = arr[j] - j !!! EUREKA!

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // given -> arr[i] - arr[j] == i - j
   // rewrite-> arr[i]-i == arr[j]-j !!! EUREKA!
   // we need to print the max sum of elements that satisfy this condition

   // hashmap to store the {arr[i]-i, sum of elements satisfying it}

   unordered_map<int, int> mp;

   for (int i = 0; i < n; i++)
   {
       int x = arr[i] - i;
       mp[x] += arr[i]; // adding the sum of the element that satisfies the condition along with arr[i]-i to identify such elements
   }
   int maxi = 0; // we need to find the largest sum
   for (auto it : mp)
   {
       int sum = it.second;
       maxi = max(maxi, sum);
   }
   return maxi;
}

int main()
{
   vector<int> arr = {1, 5, 3, 7, 8};

   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

### 33) An array of costs was given.

You always start at index 1.
You can either take 2 jumps forward or 1 jump backward. If you land on a particular index,
you have to add the cost to your total.
Find the minimum cost needed to cross the array or reach the end of the array.
You can visit any index for only 1 time.

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   vector<vector<int>> dp(n, vector<int>(2));
   // We make 2 states here, dp[i][forwardJump], dp[i][backwardJump]
   /*
   dp[i][0] = backward jump
   dp[i][1] = forward jump
   */

   dp[0][1] = arr[0];
   dp[0][0] = 1e9;                        // as it is not possible to make backward jump to the first index, as we are not allowed to get to the same index more than ones
   dp[1][1] = 1e9;                        // not possible as we can make 2 jumps not 1
   dp[1][0] = arr[2] + arr[1] + dp[0][1]; // arr[2] + arr[1] as we make the backward jump from 2nd index to first index, dp[0][1] as we go from 0th index till arr[2] forward.

   for (int i = 2; i < n - 1; i++) // the loop will go till n-2 as for the n-1th (last) index, we cannot make a backward jump so we manually set it
   {
       dp[i][1] = arr[i] + min(dp[i - 2][1], dp[i - 2][2]);
       dp[i][0] = arr[i] + arr[i + 1] + dp[i - 2][1]; // arr[i+1] because we come back from one index ahead
   }
   dp[n - 1][1] = arr[n - 1] + min(dp[n - 3][1], dp[n - 3][0]);
   dp[n - 1][0] = 1e9; // as it is not possible

   return min(dp[n - 2][1], min(dp[n - 1][1], dp[n - 1][0]));
}

int main()
{
   vector<int> arr = {2, 5, 8};
   cout << solve(arr) << endl;

   return 0;
}
```

### 33) Domino And Tromino TILING DP

[Domino And Tromino TILING DP](https://leetcode.com/problems/domino-and-tromino-tiling/description/)

[My Solution](https://leetcode.com/problems/domino-and-tromino-tiling/submissions/1184541438/)

### 34) Tower Research Tiling DP

```cpp
/*
Virender gave a puzzle to David, the puzzle begins as follows: There is a wall of size 4Xn and there is
an infinite supply of bricks of size 4X1 and 1X4, Find the total number of ways in which the bricks can be
arranged so that the entire wall is covered
*/

#include <bits/stdc++.h>
using namespace std;

int solve(int n)
{
   vector<int> dp(n + 1);
   dp[1] = 1;                   // no of ways then the size of the wall is 4X1
   dp[2] = 1;                   // no of ways then the size of the wall is 4X2
   dp[3] = 1;                   // no of ways then the size of the wall is 4X3
   dp[4] = 2;                   // no of ways then the size of the wall is 4X4
   for (int i = 5; i <= n; i++) // no of ways then the size of the wall is 4XN
   {
       dp[i] = dp[i - 1] + dp[i - 4];
   }
   return dp[n];
}

int main()
{
   int n;
   cin >> n;
   cout << solve(n) << endl;
   return 0;
}
```

### 35) Servicenow OA

```cpp
/*
Understanding : given a classroom of length NX3, find no of ways to fill it with sticks of length 1X2 or 2X1
*/

#include <bits/stdc++.h>
#define ll long long
using namespace std;

int solve(int n)
{
   vector<vector<int>> dp(n + 1, vector<int>(3, 0));
   // dp[i][0] = full cover
   // dp[i][1] = right side bulge
   // dp[i][2] = left side bulge

   // base cases
   dp[1][0] = 0; // 0 ways to fill the first row completely with sticks of size 1X2 or 2X1
   dp[1][1] = 0; // 0 ways to fill the first row, 3rd column (only 1 cell) completely with sticks of size 1X2 or 2X1
   dp[1][2] = 0; // 0 ways to fill the first row, 1st column (only 1 cell) completely with sticks of size 1X2 or 2X1
   if (n >= 2)
   {
       dp[2][0] = 3; // there are 3 ways to fill 2 rows of size 3x2 with 1x2 and 2x1 sticks
       dp[2][1] = 1; // only 1 way
       dp[2][2] = 1; // only 1 way
   }
   if (n >= 3)
   {
       dp[3][0] = 6; // there are 6 ways to fill 3 rows of size 3x2 with 1x2 and 2x1 sticks
       dp[3][1] = 3; // 3 ways
       dp[3][2] = 3; // 3 ways
   }

   if (n >= 4)
   {
       for (int i = 4; i <= n; i++)
       {
           dp[i][0] = 3 * dp[i - 2][0] + dp[i - 2][1] + dp[i - 2][2];
           dp[i][1] = dp[i - 2][0] + dp[i - 2][1];
           dp[i][2] = dp[i - 2][0] + dp[i - 2][2];
       }
   }

   return dp[n][0];
}

int main()
{
   int n;
   cin >> n;
   cout << solve(n) << endl;
   return 0;
}
```

36) Session 55 Codenation OA -> Bit Manipulaiton
### 37) Google Hard OA

```cpp
/*
Find the smallest size subset in array whose sum>=K and numbers are in increasing(non-decreasing)
order

-> If there is even a single number in array which is >=K; then answer will be 1

*/
#include <bits/stdc++.h>
using namespace std;
int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   /*
   dp[i][j] = select subsequence till index “i” but size of that subsequence should be j -> their
   sum should be as large as possible.
   dp[i][j] will store the sum here, j will be the length of the subsequence, i will be the index till
   which we have calculated. Lastly, we need to find the minimum j that satisfies the sum >=k
   */
   vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));

   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= i; j++)
       {
           for (int l = 1; l <= i - 1; l++) // l will go from 1 till i-1 and is used for calculation of sum of elements till i
           {
               if (arr[l] <= arr[i])
               {
                   dp[i][j] = arr[i] + dp[l][j - 1]; // Here, dp[i][j] is storing the sum of the numbers from 1st index till the ith index if the elements of the array are in increasing order
               }
           }
       }
   }
   int ans = INT_MAX;

   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= i; j++)
       {
           if (dp[i][j] >= k)
           {
               ans = min(ans, j); // as j stores the length
           }
       }
   }
   return ans;
}
int main()
{
   // declare an array with 1 based indexing
   return 0;
}
```

New Skate Board : <https://www.youtube.com/watch?v=cB6GkXuHo8k>

### 38) Uber OA

```cpp
/*
You are given a positive integer 'N’. Your task is to find and return the minimum number of steps that 'N' has to take to get reduced to 1.
You can perform any one of the following 3 steps:
1) Subtract 1 from it. (n = n - ­1) ,
2) If n is divisible by 2, divide by 2.( if n % 2 == 0, then n = n / 2 ) ,
3) If n is divisible by 3, divide by 3. (if n % 3 == 0, then n = n / 3 ).

*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(int n)
{
   vector<int> dp(n + 1, 0);
   dp[0] = 0;
   dp[1] = 0;
   dp[2] = 1;
   for (int i = 3; i <= n; i++)
   {
       int p1 = 1 + dp[i - 1];
       int p2 = 1e9;
       int p3 = 1e9;

       if (i % 2 == 0)
       {
           p2 = 1 + dp[i / 2];
       }

       if (i % 3 == 0)
       {
           p3 = 1 + dp[i / 3];
       }

       dp[i] = min({p1, p2, p3});
   }
   return dp[n];
}

int main()
{
   int n;
   cin >> n;
   cout << solve(n) << endl;
   return 0;
}
```

### 39) Meesho OA

```cpp
/*
A supercomputer has several processors to deploy for execution. They are arranged sequentially in a row from 1 to n. The efficiency of each processor depends on the order of deployment of its adjacent processors.

For the ith processor, the efficiency of the ith processor is no_adjacent[i], one_adjacent[i], or both_adjacent[i], depending on whether neither, one, or both adjacent processors are deployed before it.

Find the maximum possible sum of efficiencies among all possible orders of deployment.

Note: The first and nth processors can only have one adjacent processor.
Example
n = 4

no_adjacent   = [1, 2, 3, 4]
one_adjacent  = [4, 4, 2, 1]
both_adjacent = [1, 1, 1, 1]

Consider the following orders of deployment (1-based indexing):
1. If the deployment sequence is (1 -> 3 -> 4 -> 2), the sum of efficiencies is:

  no_adjacent[1] + no_adjacent[3] + one_adjacent[4] + both_adjacent[2] = 1 + 3 + 1 + 1 = 6
2. For the sequence (4 -> 2 -> 1 -> 3):

  no_adjacent[4] + no_adjacent[2] + one_adjacent[1] + both_adjacent[3] = 4 + 2 + 4 + 1 = 11
3. Let the deployment sequence be (4 -> 3 -> 2 -> 1):

  no_adjacent[4] + one_adjacent[3] + one_adjacent[2] + one_adjacent[1] = 4 + 2 + 4 + 4 = 14
Similarly, other deployment orders can be performed.

Among all possible deployments, the maximum possible sum of efficiencies is 14.
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(vector<int> no_adjacent, vector<int> one_adjacent, vector<int> both_adjacent)
{
   int n = no_adjacent.size();

   vector<vector<int>> dp(n, vector<int>(4, 0));

   /*
   dp[i][0] -> choosing the ith element such that no adjacent elements are chosen
   dp[i][1] -> choosing the ith element such that the i-1th adjacent element is chosen
   dp[i][2] -> choosing the ith element such that the i+th adjacent element is chosen
   dp[i][3] -> choosing the ith element such that both the elements i+1th and i-1th are chosen.
   */

   dp[0][0] = no_adjacent[0];
   dp[0][1] = -1e8; // no element present at i-1
   dp[0][2] = one_adjacent[0];
   dp[0][3] = -1e8; // no 2 adjacent elements present

   for (int i = 1; i < n; i++)
   {
       dp[i][0] = no_adjacent[i] + max(dp[i - 1][3], dp[i - 1][2]);
       dp[i][1] = one_adjacent[i] + max(dp[i - 1][0], dp[i - 1][1]);
       dp[i][2] = one_adjacent[i] + max(dp[i - 1][0], dp[i - 1][2]);
       dp[i][3] = both_adjacent[i] + max(dp[i - 1][0], dp[i - 1][1]);
   }
   dp[n - 1][2] = -1e8;
   dp[n - 1][3] = -1e8;

   return max({dp[n - 1][0], dp[n - 1][1], dp[n - 1][2], dp[n - 1][3]});
}

int main()
{

   vector<int> no_adjacent = {1, 2, 3, 4};
   vector<int> one_adjacent = {4, 4, 2, 1};
   vector<int> both_adjacent = {1, 1, 1, 1};

   cout << solve(no_adjacent, one_adjacent, both_adjacent) << endl;

   return 0;
}
```

### 40)

```cpp
/*
In the city of Dreamland, there lived a thief famous for its unbeatable robbery strategy. One fine day, the thief decided to rob a street where wealthy people used to live. All the houses on the street are aligned in a row $H$, a house at index $i$ is having money $H(i)$. The thief decided to follow the following principles during the robbery.
- He will rob exactly 2 consecutive houses in one go.
- He can also rob exactly 3 consecutive houses in one go.
- He will leave a gap of at least one house between two adjacent robberies if both robberies involved 2 houses each.
- He will leave a gap of at least two houses between two robberies if any of the two adjacent robberies involved 3 houses.
The thief wants to maximise the robbery amount by sticking to the above conditions. Find the maximum amount of money a thief can collect if he robs optimally.
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(const vector<int> &arr)
{
   int n = arr.size();
   vector<vector<int>> dp(n, vector<int>(2, 0));
   /*
   dp[i][0] -> 2 adjacent houses
   dp[i][1] -> 3 adjacent houses
   */

   dp[0][0] = 0;
   dp[0][1] = 0;

   if (n > 1)
   {
       dp[1][0] = arr[0] + arr[1];
       dp[1][1] = 0;
   }

   if (n > 2)
   {
       dp[2][0] = max(dp[1][0], arr[2] + arr[1]);
       dp[2][1] = arr[2] + arr[1] + arr[0];
   }
   for (int i = 3; i < n; i++)
   {
       int prev1 = i - 4 >= 0 ? dp[i - 4][1] : 0;
       dp[i][0] = max(dp[i - 1][0], arr[i - 1] + arr[i] + max(dp[i - 3][0], prev1)); // i-3 becasue leaving one house
       int prev2 = i - 5 >= 0 ? max(dp[i - 5][0], dp[i - 5][1]) : 0;
       dp[i][1] = max(dp[i - 1][1], arr[i - 2] + arr[i - 1] + arr[i] + prev2);
   }
   return max(dp[n - 1][0], dp[n - 1][1]);
}

int main()
{
   vector<int> arr = {1, 5, 5, 5, 1}; // ans-> (4+1) + (5+6)
   cout << solve(arr) << endl;
   return 0;
}
```

### 41)

```cpp
/*
You are given an array A. Size = N. You start at index 1 and end at index N. Find the number of ways to reach from index 1 to index N.

You have to follow some conditions.
Each number A[i] = 1 or 2 ; if you are at index i you can make a jump of size from range [1…..A[i]]
You are allowed to jump backwards also but utmost 1 time. You can visit any index atmost once.
*/
#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(const vector<int> &arr)
{
   int n = arr.size();

   vector<vector<int>> dp(n, vector<int>(2, 0));
   /*
   dp[i][0] -> number of journeys till the ith index without any backward jump till i
   dp[i][1] -> number of journeys till the ith index with 1 backward jump between 0 and i-1
   dp[i][1] does not necessarily mean jumping back from i+1. It is the number of journeys till ith index where a jump was made previously before reaching index i. This is because we have not calculated the (i+1)th value yet.
   */

   dp[0][0] = 1; // 0 different ways so 0! = 1;
   dp[0][1] = 0;

   dp[1][0] = 1; // only 1 way to go from 0 to 1.

   for (int i = 2; i < n; i++)
   {
       dp[i][0] = dp[i - 1][0];

       if (arr[i - 2] == 2)
       {
           dp[i][0] += dp[i - 2][0];
       }

       dp[i][1] = dp[i - 1][1];

       if (arr[i - 2] == 2)
       {
           dp[i][1] += dp[i - 2][1];
       }

       if (arr[i - 3] == 2 && arr[i - 2] == 2)
       {
           dp[i][1] += dp[i - 3][0];
       }
   }

   return dp[n - 1][0] + dp[n - 1][1];
}

int main()
{
   vector<int> arr = {1, 2, 1, 1, 2};
   cout << solve(arr) << endl;
   return 0;
}
```

42) <https://leetcode.com/problems/paint-house-iv/description/>
### 43) Uber OA

```cpp
/*
Understanding :- Given an array of size “N” ; select <=x subarrays of size “1” ; select <=y subarrays of size “2” ; select <=z subarrays of size “3” -> they should all be non-overlapping

-> Select them in such a way so that the final sum is maximum

*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

ll solve(const vector<ll> &arr, ll x, ll y, ll z)
{
   ll n = arr.size();
   vector<vector<vector<vector<ll>>>> dp(n + 1, vector<vector<vector<ll>>>(x + 1, vector<vector<ll>>(y + 1, vector<ll>(z + 1, 0))));

   /*
   dp[i][x][y][z] -> maximum sum till the ith index when:
                     x number of subarrays of size 1
                     y number of subarrays of size 2
                     z number of subarrays of size 3 are present
   */

   for (int i = 1; i <= n; i++)
   {
       for (int a = 0; a <= x; a++)
       {
           for (int b = 0; b <= y; b++)
           {
               for (int c = 0; c <= y; c++)
               {
                   int p1 = 0, p2 = 0, p3 = 0, p4 = 0;
                   p1 = dp[i - 1][a][b][c];
                   if (a - 1 >= 0)
                       p2 = arr[i] + dp[i - 1][a - 1][b][c];

                   if (b - 1 >= 0 && i - 2 >= 0)
                       p3 = arr[i] + arr[i - 1] + dp[i - 2][a][b - 1][c];

                   if (c - 1 >= 0 && i - 3 >= 0)
                   {
                       p3 = arr[i] + arr[i - 1] + arr[i - 2] + dp[i - 3][a][b][c - 1];
                   }

                   // take the max of p1, p2, p3, p4 permutations
                   dp[i][a][b][c] = max({p1, p2, p3, p4});
               }
           }
       }
   }
   return dp[n - 1][x][y][z];
}

int main()
{
   vector<ll> arr = {0, 3, 7, -2, 5, 4, 6}; // 1 based indexing
   ll x = 1;
   ll y = 1;
   ll z = 1;
   cout << solve(arr, x, y, z) << endl;
   return 0;
}
```

### 44) Edit Distance DP.

When solving edit distance DP, always take the dp size as n+1 because dp[0] means the string is empty.

```cpp
/*
Article link: https://www.geeksforgeeks.org/dsa/edit-distance-dp-5/#better-approach-2-using-bottomup-dp-tabulationomn-time-and-omn-space
*/
#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(string s1, string s2)
{
   int n = s1.size();
   int m = s2.size();

   vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0)); // dp[0][0] means empty strings

   for (int i = 0; i <= n; i++)
   {
       dp[i][0] = i;
   }
   for (int j = 0; j <= m; j++)
   {
       dp[0][j] = j;
   }
   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= m; j++)
       {
           if (s1[i - 1] == s2[j - 1])
           {
               if (i > 0 && j > 0)
               {
                   dp[i][j] = dp[i - 1][j - 1];
               }
           }
           else
           {
               if (i > 0 && j > 0)
               {
                   dp[i][j] = 1 + min({dp[i - 1][j], dp[i - 1][j - 1], dp[i][j - 1]});
               }
               else
               {
                   dp[i][j] = 1;
               }
           }
       }
   }
   return dp[n][m];
}

int main()
{
   string s1 = "abcd";
   string s2 = "bcfe";

   cout << solve(s1, s2) << endl;

   return 0;
}
```

### 45)

```cpp
/*
Given two strings word1 and word2, find the minimum number of operations required to convert word1 into word2.

You process word1 using a cursor from left to right. At your current position, you may perform one of the following operations:

- Match: If the current character of word1 matches the required character in word2, move past both characters (cost 0).

- Replace: Replace the current character of word1 to match the required character in word2, and move past both characters (cost 1).

- Delete: Delete the current character of word1 (cost 1).

Side effect: All remaining unprocessed characters to the right of your cursor immediately undergo ROT13.

- Insert: Insert the required character for word2 at your current position (cost 1).

Side effect: All remaining unprocessed characters to the right of your cursor immediately undergo ROT13.

ROT13 shifts a lowercase letter forward by 13 positions in the alphabet, wrapping around (e.g., 'a' becomes 'n', 'm' becomes 'z', 'n' becomes 'a'). Applying ROT13 twice restores the original character.
*/
#include <bits/stdc++.h>

using namespace std;

#define ll long long

char ROT13(char ch)
{
   return 'a' + (ch - 'a' + 13) % 26;
}

int solve(string s1, string s2)
{
   int n = s1.size();
   int m = s2.size();

   vector<vector<vector<int>>> dp(n + 1, vector<vector<int>>(m + 1, vector<int>(2, 1e8)));
   /*
   dp[i][j][k] -> check the ith index of s1 with jth index of s2 when Number of ROT13 operations are k
                   where k = parity of ROT13 applications so far
                  (k = 1 means the unprocessed part of s1 is currently ROT13'd)
   */

   dp[0][0][0] = 0; // empty strings, no operations yet

   for (int i = 1; i <= n; i++)
   {
       dp[i][0][i % 2] = i;
   }
   for (int j = 1; j <= m; j++)
   {
       dp[0][j][j % 2] = j;
   }

   for (int i = 1; i <= n; i++)
   {
       for (int j = 1; j <= m; j++)
       {
           for (int k = 0; k < 2; k++)
           {
               char ch = (k == 0) ? s1[i - 1] : ROT13(s1[i - 1]);
               int cost = (ch == s2[j-1]) ? 0 : 1;
               dp[i][j][k] = min({dp[i - 1][j - 1][k] + cost, dp[i - 1][j][k ^ 1] + 1, dp[i][j - 1][k ^ 1] + 1});
           }
       }
   }
   return min(dp[n][m][0], dp[n][m][1]);
}

int main()
{

   string s1 = "abc";
   string s2 = "def";

   cout << solve(s1, s2) << endl;

   return 0;
}
```

### 46)

```cpp
/*
Given an alphabet set S = {'a', 'b', 'c', 'd'} and an integer N, find the total number of valid strings of length N that satisfy the following conditions:
The characters are ordered as {a, b, c, d}.
Two consecutive characters in the string cannot be adjacent in this ordering. In other words, if index(a)=0, index(b)=1, index(c)=2, and index(d)=3, then for every adjacent pair (x, y) in the string:
∣index(x)−index(y)∣ != 1
The total number of occurrences of the character 'a' in the string must be odd.
The forbidden adjacent pairs are:
ab, ba
bc, cb
cd, dc
All other adjacent pairs are allowed.
Since the answer can be very large, return it modulo 10^9+7.
Input Format
A single integer N (1≤N≤10^5).
Output Format
Print a single integer representing the number of valid strings of length N modulo 10^9+7.
Example 1
Input
1
Output
1
Explanation
The possible strings are a, b, c, and d. Only a contains an odd number of characters.
Example 2
Input
2
Output
4
Explanation
The valid strings with an odd number of a characters are:
ac
ad
ca
da
Hence, the answer is 4.
Example 3
Input
3
Output
11
Example 4
Input
4
Output
30
*/
/*
We need to store the even as well as odd occurrences of ‘a’ as the even occurrences of ‘a’ will help us build the subsequent odd occurrences. */
#include <bits/stdc++.h>

using namespace std;

#define ll long long
int MOD = 1e9 + 7;

int solve(int n)
{
   vector<vector<vector<int>>> dp(n, vector<vector<int>>(4, vector<int>(2, 0)));
   /*
   dp[i][a/b/c/d][even/odd]-> maximum ans till index i when the last character is a/b/c/d and
   the number of occurrence of a is even/odd
   even->0, odd->1
   a->0, b->0, c->0, d->0
   */
   dp[0][0][1] = 1; // Only "a" is the possible ans
   dp[0][1][0] = 1;
   dp[0][2][0] = 1;
   dp[0][3][0] = 1;

   for (int i = 1; i < n; i++)
   {
       // last element a, odd/even
       dp[i][0][0] = (dp[i - 1][0][1] + dp[i - 1][2][1] + dp[i - 1][3][1]) % MOD;
       dp[i][0][1] = (dp[i - 1][0][0] + dp[i - 1][2][0] + dp[i - 1][3][0]);

       // last element b, odd/even
       dp[i][1][0] = (dp[i - 1][1][0] + dp[i - 1][3][0]);
       dp[i][1][1] = (dp[i - 1][1][1] + dp[i - 1][3][1]);

       // last element c, odd/even
       dp[i][2][0] = (dp[i - 1][2][0] + dp[i - 1][0][0]);
       dp[i][2][1] = (dp[i - 1][2][1] + dp[i - 1][0][1]);

       // last element d, odd/even
       dp[i][3][0] = (dp[i - 1][3][0] + dp[i - 1][1][0] + dp[i - 1][0][0]);
       dp[i][3][1] = (dp[i - 1][3][1] + dp[i - 1][1][1] + dp[i - 1][0][1]);
   }
   return dp[n - 1][0][1] + dp[n - 1][1][1] + dp[n - 1][2][1] + dp[n - 1][3][1];
}

int main()
{
   int n;
   cin >> n;
   cout << solve(n) << endl;
   return 0;
}
```

### 47) Linkedin OA Partition DP

```cpp
/*
Given an array - divide the array into K-parts such that the sum of the maximum of each part is minimized.
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(const vector<int> &arr, int k)
{
   int n = arr.size();

   vector<vector<int>> dp(n + 1, vector<int>(k + 1, INT_MAX));
   /*
   dp[i][k] -> minimized maximum sum to divide the array of i elements into k parts
   The easiest way to understand this pattern in partition DP is to think: I am fixing the last partition and asking how optimally I can divide everything before it.
   */

   for (int parts = 0; parts <= k; parts++)
   {
       dp[0][parts] = 0; // to divide 0 into any part, it is always 0
   }

   for (int i = 1; i <= n; i++)
   {
       for (int parts = 1; parts <= k; parts++)
       {
           int maxi = INT_MIN; // This variable will be used to find the largest element from i to j
           for (int j = i; j > 0; j--)
           {
               maxi = max(maxi, arr[j - 1]);

               if (dp[j - 1][parts - 1] != INT_MAX)
               {
                   dp[i][parts] = min(dp[i][parts], maxi + dp[j - 1][parts - 1]);
               }
           }
       }
   }
   return dp[n][k];
}

int main()
{
   vector<int> arr = {1, 2, 1, 3, 5};
   int k = 3;
   cout << solve(arr, k) << endl;
   return 0;
}
```

48) Leetcode biweekly Contest
