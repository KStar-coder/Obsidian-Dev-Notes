**Doubts** **: Q8 , Q10, Q13,**

### 1) Two sum

[Two sum](https://leetcode.com/problems/two-sum/description/) sum of two numbers must be equal to a given target, return indices of the two numbers

-> [My solution](https://leetcode.com/problems/two-sum/submissions/1091128126/)

Solution description : use a unordered_map to store the element and it’s index
Now , for each value in the array, we need to find an element who’s value is t*arget-arr\[i\]*.
If it is present in the map them we return the index of the element present in the map and the current index ‘i’.

### 2) Max distance between same elements

[Max distance between same elements](https://practice.geeksforgeeks.org/problems/max-distance-between-same-elements/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab) . From an array find the maximum distance between the occurrences of two same elements

-> [My Solution](https://practice.geeksforgeeks.org/problems/max-distance-between-same-elements/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab)

Solution Description : create an unordered_map {elemment, first index of occurrence}
If the element is present in the subsequent iterations find the difference between that index and the index of the first occurrence stored in the hash map. Keep a track of the maximum distance after each iteration to find the maximum occurrence.

### 3) Find Common Characters

[Find Common Characters](https://leetcode.com/problems/find-common-characters/description/) : given an array of strings, print the common characters present in all the strings.

-> [My solution](https://leetcode.com/problems/find-common-characters/submissions/1091496456/) : create two unordered_map

First unordered_map ={character, min. No of Occurrences in all words}

Second unordered_map  = {character, no of occurrences in that particular word}

Then after all the iterations, print the common characters minimum no of times.

### 4) Longest Consecutive Sequence

[Longest Consecutive Sequence](https://leetcode.com/problems/longest-consecutive-sequence/description/) : find the longest consecutive sequence from an array

-> [My solution](https://leetcode.com/problems/longest-consecutive-sequence/submissions/1091878345/)

Create 2 unordered_map \<int, bool\> present ,checked;
Present : {number present in array, true} : used to find if a number is present in the array
Checked: {number checked, true} : used to find if a number is already checked for a sequence to avoid checking it again

The idea is to find the starting number for a sequence and then add +1 to that number till the time the +1 number is also present in the present map and update the longest consecutive sequence after that and mark the checked\[number\] to true in each iteration.

### 5) Largest Subarray with Sum 0

[Largest Subarray with Sum 0](https://practice.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab) : Given an array having both positive and negative integ[Largest subarray with 0 sum \| Practice \| GeeksforGeeks](https://practice.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab)ers. The task is to compute the length of the largest subarray with sum 0.

-> [My Solution](https://practice.geeksforgeeks.org/problems/largest-subarray-with-0-sum/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab)

Create an unordered_map\<int,int\> with prefixSum and First Occurrence

Whenever that prefix sum has occurred again, subtract that current index i with the *FirstOccurrence* index and keep it in a maxi variable.

```cpp
int maxLen(vector<int>&arr, int n)
    {
        // Your code here
        unordered_map<int,int>mp; //{prefixSum , first Occurrence}

        int psum = 0;
        int ans = 0;
        for (int i = 0; i<n; i++)
        {

            psum += arr[i];

            if (mp.find(psum) != mp.end())
            {
                int len = i-mp[psum] +1 ;
                ans = max(ans, len);
            }

            // updating the map for the first occurrence only
            else
            {
                mp[psum] = i;
            }
        }
        return ans;
    }
```

### 6) Count No of Pairs with Absolute Difference K

[Count No of Pairs with Absolute Difference K](https://leetcode.com/problems/count-number-of-pairs-with-absolute-difference-k/description/) : Given an integer array nums and an integer k, return *the number of pairs* (i, j) *where* i \< j *such that* \|nums\[i\] - nums\[j\]\| == k

-> [My Solution](https://leetcode.com/problems/count-number-of-pairs-with-absolute-difference-k/submissions/1091973735/)

We will take a hashmap to store the elements and their occurrences. Here the modulo expression is used
\|x-y\| = z;
This can be interpreted as
-x+y = z ; x-y = z
=\> *x = y-z ; x = y+z (Taking x on one side for easy coding)*
We can use this in our code

***\# we will be creating the hashmap on the go while iteration so that we do not take one pair twice. We should take one pair of x and y only ones in our ans.***

### 7) Subarrays with sum k

[Subarrays with sum k](https://practice.geeksforgeeks.org/problems/subarrays-with-sum-k/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab) : Given an unsorted array of integers, find the number of continuous subarrays having sum exactly equal to a given number k.

-> [My Solu](https://practice.geeksforgeeks.org/problems/subarrays-with-sum-k/1?utm_source=geeksforgeeks&utm_medium=article_practice_tab&utm_campaign=article_practice_tab)tion 👍
Create a hash map of the prefix sum and the number of it’s occurrences

*Unordered_map -> {prefixSum, noOfOccurrences}*
During iteration,
If the sum is = k then we increment the ans to +1;
If not then we increment the count by the number of occurrences of        the currentSum - k. Then we insert the current sum’s occurrence in the map as mp\[currentSum\]++ which increases the occurrence of the currentSum by 1. At the end of the loop we return the ans .

```cpp
int findSubArraySum(int Arr[], int N, int k)
    {
        unordered_map<int,int> mp;

        int count = 0;
        int sum = 0;
        for (int i = 0;i <n; i++)
        {
            sum += Arr[i];

            // if the sum is already equal to k then we simply increment the count
            if (sum == k)
                count++;

            // increment the count to the number of occurrences of sum-k (can be 0 occurrences as well)
            count += mp[sum-k];

            mp[sum]++; //increment the occurrence of sum in map by 1 after each traversal
        }
        return sum;
    }
```

### 8) Subsets with XOR value

[Subsets with XOR value](https://practice.geeksforgeeks.org/problems/subsets-with-xor-value2023/1?utm_source=geeksforgeeks&utm_medium=ml_article_practice_tab&utm_campaign=article_practice_tab) : Given an array arr of N integers and an integer K, find the number of subsets of arr having XOR of elements as K.

![[Hashing Documentation-image1.png]]

### 9) Prefix Common Array of Two Arrays

[Prefix Common Array of Two Arrays](https://leetcode.com/contest/biweekly-contest-103/problems/find-the-prefix-common-array-of-two-arrays/) : A prefix common array of A and B is an array C such that C\[i\] is equal to the count of numbers that are present at or before the index i in both A and B. Return the prefix common array of A and B. A sequence of n integers is called a permutation if it contains all integers from 1 to n exactly once.

-> [My Solution](https://leetcode.com/submissions/detail/1094210876/)
Here, we use two unordered maps to store the frequency of elements of A and B. We do this iteratively at the same time while finding out the common elements on both in a single for loop. This is done to get the common elements at each step individually otherwise our output array will consist of n elements of same value
Eg test case :
Input: A = \[1,3,2,4\], B = \[3,1,2,4\]
Output: \[0,2,3,4\]
Explanation: At i = 0: no number is common, so C\[0\] = 0.
At i = 1: 1 and 3 are common in A and B, so C\[1\] = 2.
At i = 2: 1, 2, and 3 are common in A and B, so C\[2\] = 3.
At i = 3: 1, 2, 3, and 4 are common in A and B, so C\[3\] = 4.

Hence, we have to insert elements in the hash map and simultaneously find the common elements.

### 10) Longest Subarray with sum k

[Longest Subarray with sum k](https://practice.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article)

-> [My Solution](https://practice.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1?utm_source=gfg&utm_medium=article&utm_campaign=bottom_sticky_on_article) : Here, we have taken an unordered_map\<int,int\> -> {prefixSum, index} to keep a track of the prefix sum and the index of the prefix sum.
We fill the unordered_map simultaneously while calculating the largest subarray at each instance and do not fill it beforehand.

3 conditions in the for loop;
If Sum is present in hashmap
Update the length of maxi = max(maxi, i+1)

If Sum - target is present in the hashmap
Update the length of maxi = max(maxi, i-mp\[sum-k\])
If sum not found in map
Store it in map as mp\[sum\] = i;

### 11) Longest palindrome by concatenation of two letters

[Longest palindrome by concatenation of two letters](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/description/)

**Input:** words = \["lc","cl","gg"\]
**Output:** 6
**Explanation:** One longest palindrome is "lc" + "gg" + "cl" = "lcggcl", of length 6.
Note that "clgglc" is another longest palindrome that can be created.

-> [My Solution](https://leetcode.com/problems/longest-palindrome-by-concatenating-two-letter-words/post-solution/?submissionId=1094708379)

Here for each word in words, we have two cases
i) Two letters are not same in the word eg : lc, bc, mc

Here, we can check if the reverse of the string exists in the map eg: for lc we check if cl is present in the map. If yes, then it can form a palindrome.
Now, there can be multiple occurrences of lc and cl, for a palindrome, we need to take equal number of both of them so we take the min (words\[i\], mp\[string\])
And update the length as len += min(words\[i\],mp\[string\])\*4;
\*4 is done because lccl -> 2\*2 -> 4 size

ii) Two letters are the same in the word eg : gg, ll, cc etc.

Here, if the occurrence of the same letter word is even then we can simply increase the length as
Len += noOfOccurrences \* 2;

If The occurrence of the same letter word is odd then we have to do something else…
We keep a flag variable initialized to 0,
So, if flag == 0 then we simply add the length as length += count\*2; and update flag  = 1;

If flag == 1 then we need to update the length as
Length += (count-1)\*2;
*This is done so because we need to alternately add the substring completely or 1 less time to make it palindrome if the characters in the string are same and it has occurred odd no. of times* (revise this concept again and again!)

### 12) Question link

[Question link](https://www.desiqna.in/13267/microsoft-coding-oa-sde-1-may-3-2023) : Question1: Given an array A consisting of N integers, returns the maximum sum of two numbers whose digits add up to an equal sum.

if there are not two numbers whose digits have an equal sum, the function should return -1.
Example1:
Input:
A = \[51, 71, 17, 42\]
Output: 93
Explanation: There are two pairs of numbers whose digits add up to an equal sum: (51, 42) and (17, 71), The first pair sums up to 93

Example2:
Input:
A = \[42, 33, 60\]
Output: 102
Explanation: The digits of all numbers in A add up the same sum, and choosing to add 42 and 60 gives the result 102

Example3:
Input:
A = \[51, 32, 43\]
Output: -1
Explanation: All numbers in A have digits that add up to different, unique sums

```cpp
int dsum(int a)
    {
        int num = a;
        int sum = 0;
        while (num > 0)
        {
            int dgt = num % 10;
            sum += dgt;
            num = num / 10;
        }
        return sum;
    }

    int solve(vector<int> &arr)
    {
        int n = arr.size();

        // unordered_map to store the {digitSum, arr[i]}
    unordered_map<int, int> mp;
        int maxi = -1;
        for (int i = 0; i < n; i++)
        {
            int sum = 0;
            int x = dsum(arr[i]);
            if (mp.find(x) != mp.end())
            {
                sum += mp[x] + arr[i];
                maxi = max(maxi, sum);
            }
            else
                mp[x] = arr[i];
        }
        return maxi;
    }
```

Explanation : we will use an unordered_map\<int,int\> to store {digitSum, element}

Initialize maxi to -1
Enter the loop
If a digit sum is present in the array, we add the maximum of the two elements with the same digit sum.
Else a digit sum is not present int he array, we add it in the map
Then after this if else condition we go to the next iteration. We return maxi at the end of the loop.

---

**Range update concept (Google)**

Trick : we set arr\[L\] = 1 and arr\[R+1\] = -1 for all the queries.
Ones that is done, we run prefix sum algorithm for the entire array.

\[0,0,0,0,0,0,\]
L = 3, r = 6
-> \[0,0,1,0,0,0,-1\] -> we take array of size n+1 to accommodate if the R = n;
Prefix sum : \[0,0,1,1,1,1\] -> array transformed successfully without another loop of traversal

---

### 13) Google’s question

[Google’s question](https://www.desiqna.in/13650/google-girl-hackathon-coding-questions-solutions-2023-kumar)

Eg : n = 3
\[5,8,10\]; k = 2; so range of addition/subtraction = \[-2,2\]
Ans : \[ (5+0) (8+2) (10 + 0)\] -> \[5,10,10\]
Max no of similar elements = 2

Soln : \[5,8,10\] ; let k = 3

For 5, possible values : 2,3,4,5,6,7,8
For 8, possible values : 5,6,7,8,9,10,11
For 10, possible values : 7,8,9,10,11,12,13

Now, we create a map to find the common elements with a given sum
Sum : 2 3 4 5 6 7 8 9 10 11 12 13
Freq: 1 1 1 2 2 3 3 2  2    2    1   1

We can observe that highest freq = 3 and largest no. with freq 3 is : 8
Hence, array can be transformed to -> \[8,8,8\] when k = 3;
Ans : 3 (highest frequency or maximum no of similar elements)

### 14) Lamp Illumination

[Lamp Illumination](https://www.desiqna.in/16114/visa-oa-sde-intern-ctc-30-lac-27th-oct) : Question Explanation :

You are given an array A of all zeroes. For each query Q containing l and r, fill the array arr\[i\] with arr\[i\]+1 for the range of indices \[l,r\] in the array
Eg test case :

```cpp
   vector<vector<int>> lamps = {
       {1, 7},
       {5, 11},
       {7, 9}};
   vector<int> points = {7, 1, 5, 10, 9, 15};
  ans : {3,1,2,1,2,0};
*/

->
void prefixSum(vector<int> &ans, int l, int r)
{

   for (int i = l + 1; i <= r; i++)
   {
       ans[i] += 1;
   }
}
void solve(vector<vector<int>> &lamps, vector<int> &points, int l, int r, vector<int> &ans)
{
   ans[l] += 1;
   ans[r + 1] += -1;
   prefixSum(ans, l, r);
   ans[r + 1] += 1; // To revert back
}

int main()
{
   vector<vector<int>> lamps = {
       {1, 7},
       {5, 11},
       {7, 9}};

   vector<int> points = {7, 1, 5, 10, 9, 15};

   int n = lamps.size(); //  number of queries

   vector<int> ans(1e9, 0); // points can be of any range so we are taking a large size array
   for (int i = 0; i < n; i++)
   {
       int l, r;
       l = lamps[i][0];
       r = lamps[i][1];
       // find the no of lamps illuminating these above points
       solve(lamps, points, l, r, ans);
   }

   // printing the desired points
   for (int i = 0; i < points.size(); i++)
   {
       cout << ans[points[i]] << " ";
   }
   cout << endl;
   return 0;
}
```

The code is self Explanatory

### 15) Pair Search (Google OA)

[Pair Search (Google OA)](https://www.desiqna.in/13820/google-oa-sde2-jan-2023) :  a very simple hashmap frequency of element finder

-> no need you already know it

### 16) Maximum Sum Selection

[Maximum Sum Selection](https://www.desiqna.in/16087/media-net-oa-sde1-ctc-28-l) : (Medianet OA)

Maximum Sum Selection

You are given an integer array A of size N. Your task is to select exactly B elements from either the left end or the right end of the array A in order to maximize the sum of the selected elements.

Here are the details of the problem:

You are given an integer array A of size N where 1 \<= N \<= 10^5.
You need to select exactly B elements from either the left end or the right end of the array A.
The goal is to find and return the maximum possible sum of elements you can pick.

Input:

An integer array A of size N where each element A\[i\] satisfies -103 \<= A\[i\] \<= 103.
An integer B where 1 \<= B \<= N.
Output:

An integer representing the maximum possible sum of selected elements.

0  1   2  3  4
\[5, -2, 3, 1, 2\], 3 -> 8

```cpp
int solve(vector<int> &arr, int k)
    {
       int n = arr.size();
       // storing the prefix and suffix sum
       vector<int> suffix(n);
       suffix[n - 1] = arr[n - 1];
       for (int i = n - 2; i >= 0; i--)
       {
           suffix[i] = suffix[i + 1] + arr[i];
       }

       int presum = 0;
       int maxi = 0;
       for (int i = 0; i < k; i++)
       {
           presum += arr[i];
           maxi = max(maxi, (presum + suffix[n - k + 1 + i]));
       }
       return maxi;
    }
```

We are using prefix and suffix sum technique for this problem.

### 17) Arsencium OA

[Arsencium OA](https://www.desiqna.in/13736/arcesium-oa-sde1-jan-2023)

```cpp
bool cmpMap(map<char, int> m1, map<char, int> m2)
    {
       char g = 'a';
       while (g <= 'z')
       {
           // if the character is present in both the maps
           if (m1.find(g) != m1.end() && m2.find(g) != m2.end())
           {
               if (m1[g] != m2[g])
               {
                   return false;
               }
           }
           // if the character is present in only one map
           else if ((m1.find(g) == m1.end() && m2.find(g) != m2.end()) || (m1.find(g) != m1.end() && m2.find(g) == m2.end()))
           {
               return false;
           }

           g++;
       }
       return true;
    }

    int main()
    {
       string s1 = "tggt";
       string s2 = "ggtt";

       // we will create 2 maps for each string
       // odd map to store odd indices char frequency
       // even map to store even indices char frequency

       // we will use map and not unordered_map as maps are easy to compare as they store data in a sorted manner
       map<char, int> odd1, even1, odd2, even2;

       int n = s1.size(); // size of s1 ans s2 is same according to the question

       for (int i = 0; i < n; i++)
       {
           if (i % 2 == 0) //  even index
           {
               even1[s1[i]]++;
               even2[s2[i]]++;
           }
           else
           {
               odd1[s1[i]]++;
               odd2[s2[i]]++;
           }
       }
       if (cmpMap(odd1, odd2) == true && cmpMap(even1, even2) == true)
       {
           cout << "Yes" << endl;
       }
       else
       {
           cout << "No" << endl;
       }

       return 0;
    }
```

we will create 2 maps for each string
odd map to store odd indices char frequency
even map to store even indices char frequency
Then, we will compare the maps with each other using the cmpMap function.
We are using map and not unordered_map as we need to store the characters in a sorted manner for easy comparison.

### 18) 4SumII

[4SumII](https://leetcode.com/problems/4sum-ii/description/) (google question)

Given four integer arrays nums1, nums2, nums3, and nums4 all of length n, return the number of tuples (i, j, k, l) such that:

- 0 \<= i, j, k, l \< n
- nums1\[i\] + nums2\[j\] + nums3\[k\] + nums4\[l\] == 0

**Example 1:**

**Input:** nums1 = \[1,2\], nums2 = \[-2,-1\], nums3 = \[-1,2\], nums4 = \[0,2\]
**Output:** 2
**Explanation:**
The two tuples are:
1. (0, 0, 0, 1) -> nums1\[0\] + nums2\[0\] + nums3\[0\] + nums4\[1\] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> nums1\[1\] + nums2\[1\] + nums3\[0\] + nums4\[0\] = 2 + (-1) + (-1) + 0 = 0

-> [My Solution](https://leetcode.com/problems/4sum-ii/submissions/1101247908/)

We will create a hashMap to store the combination of sum of all the elements in num 3 and num 4 in a nested for loop. We will use unordered_map\<int,int\> to store the {sum, frequency}

Then we will iterate over num1 and num2 in another nested for loop to check if the  -1\*(num1\[i\] + num2\[i\] ) is present in the hashmap.
We will use unordered_map\<int,int\> to store the {sum, frequency}

### 19) Question link : Google OA

Question link : [Google OA](https://docs.google.com/document/d/1LthiOPD4CvHTu9d0_h_XKJF9U9BYdjYYetnjjAPQLpo/edit)

*Problem.-\> Given an array of size “N”; find the number of triplets; such that A\[i\] \>A\[j\]\< A\[k\] such that i \< j \< k ;*

Solution description : Here, we create 2 hashmaps, pref and suff
pref\[i\] -> it will store the number of elements greater than arr\[i\] present before arr\[i\]
suff\[i\] -> it will store the number of elements greater than arr\[i\] present after arr\[i\]
\*/

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // declaring the pref and suff array
   vector<int> pref(n + 1);
   vector<int> suff(n + 1);

   pref[0] = 0; // first element of the pref is 0 as there is not element before pref

   // finding the pref array
   int j = 2;
   while (j <= n)
   {
       int i = 1;
       int c = 0;

       while (i <= j - 1) // i != j as we cannot compare the same element with itself
       {
           if (arr[i] > arr[j])
           {
               c++;
           }
           i++;
       }
       pref[j] = c;
       j++;
   }

   // finding the suff array
   suff[n] = 0; //  last element of the suff array is 0 as no element exists after it
   int k = n - 1;
   while (k >= 1)
   {
       int c = 0;
       int l = k + 1;
       while (l <= n) // check for values lying after the kth index
       {
           if (arr[k] > arr[l])
           {
               c++;
           }
           l++;
       }
       suff[k] = c;
       k--;
   }

   // finding the overall sum
   int count = 0;
   int j = 1;
   while (j <= n)
   {
       int k = j + 1;
       while (k <= n)
       {
           if (arr[j] < arr[k])
           {
               count = count + pref[j] * suff[k];
           }
           k++;
       }
       j++;
   }
   return count;
}
int main()
{
   vector<int> arr = {8, 3, 5, 6, 2, 1, 23, 5, 6, 2};

   return 0;
}
```

20) [Question and solution. (self explanatory)](https://www.geeksforgeeks.org/count-quadruplets-with-sum-k-from-given-array/)

### 21) Microsoft OA Rehab

[Microsoft OA Rehab](https://docs.google.com/document/d/143re9hqaxWxVkGTFKZJ24RhR_6bxs64su0Dmb8Kjdw4/edit)

-> My solution
*/\**
*A patient needs rehabilitation for N days (numbered from 0 to n-1). The rehabilitation consists of x sessions.*
*For each rehabilitation session other than the last one,the next session is exactly y days later.*

*You are given an array A of n integers where A\[i\] is the cost of the individual rehabilitation*

*return the minimum cost of rehabilitation*

*eg :*
*vector\<int\> arr = {4, 2, 5, 4, 3, 5, 1, 4, 2, 7};*
*int x = 3, y = 2;*

*Ans : 6*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int x, int y)
{
   int n = arr.size();

   // creating the prefix arr
   vector<int> pref(n);

   int i = 0;
   while (i < n)
   {
       if (i - y >= 0)
       {
           pref[i] = arr[i] + pref[i - y];
       }
       else
       {
           pref[i] = arr[i];
       }
       i++;
   }

   int ans = INT_MAX;

   for (int i = 0; i < n; i++)
   {
       // we will be taking the last x-1 elements that fall in y steps before the current element
       int index = i - (x - 1) * y; // for the previous element in pref
       if (index >= 0)
       {
           int g = pref[i];
           if (index - y >= 0)
           {
               g = g - pref[index - y];
           }
           ans = min(g, ans);
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {4, 2, 5, 4, 3, 5, 1, 4, 2, 7};
   int x = 3, y = 2;
   int ans = solve(arr, x, y);
   cout << ans << endl;
   return 0;
}
```

Here, we will keep a prefix array that contains the prefix sum for y steps back for each element. We only need the last x elements so while calculating we will use an index to remove the extra elements from our sum in the line -> index = i - (x-1)\*y

### 22) Leetcode contest question

[Leetcode contest question](https://leetcode.com/contest/weekly-contest-365/problems/maximum-value-of-an-ordered-triplet-i/) : Return *the maximum value over all triplets of indices* (i, j, k) *such that* i \< j \< k. If all such triplets have a negative value, return 0.

The value of a triplet of indices (i, j, k) is equal to (nums\[i\] - nums\[j\]) \* nums\[k\]

```cpp
long long maximumTripletValue(vector<int>& nums) {

        ll n = nums.size();

        vector<ll> pref(n,0);

        // setting the first value of the pref array

        pref[1] = nums[0]-nums[1];

        // x -> maximum value with which we need to subtract nums[i] to get a bigger value in each iteration

        ll x = max (nums[0], nums[1]);

        ll ans = 0;

        // starting the loop from 2 as pref[1] is already calculated

        for (int i = 2; i<n; i++)

        {

            // getting the pref array for the current index before calculating the triplet value

            pref[i] = max (pref[i-1], x-nums[i]);

            // calculating the triplet

            ll triplet = pref[i-1]*nums[i];

            // updating the answer

            ans = max (ans, triplet);

            // updating the value of x

            x = max (x, ll(nums[i]));

        }

        return ans;

    }
```

We will use a pref array where pref\[i\] is the largest difference between elements possible till the ith index.

### 23) No of Mountain Subarrays

[No of Mountain Subarrays](https://docs.google.com/document/d/1_d1CH0HblrFYUgtAPq5FYsNvXo1YD0XsUyZ6OTVHb80/edit)

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // now we create a prefix and a suffix array
   // prefix[i] -> to find the longest increasing subarray till the ith index of arr
   // suffix[i] -> to find the longest decreasing subarray till the ith index of arr

   vector<int> prefix(n);
   vector<int> suffix(n);

   // creating the prefix array
   prefix[0] = 1;
   for (int i = 1; i < n; i++)
   {
       if (arr[i - 1] < arr[i])
       {
           prefix[i] = prefix[i - 1] + 1;
       }
       else
       {
           prefix[i] = 1;
       }
   }

   // creating the suffix array
   suffix[n - 1] = 1;
   for (int i = n - 2; i >= 0; i--)
   {
       if (arr[i + 1] < arr[i])
       {
           suffix[i] = suffix[i + 1] + 1;
       }
       else
       {
           suffix[i] = 1;
       }
   }

   // finding the mountain for each index and adding the total
   // ans[i] = (prefix[i]-1)*(suffix[i]-1)

   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       ans += (prefix[i] - 1) * (suffix[i] - 1);
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 2, 4, 2, 1};
   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

### 24) You are given an array, Find the maximum sum of two non overlapping…

[You are given an array, Find the maximum sum of two non overlapping subarrays. The 2nd subarray must be strictly increasing](https://docs.google.com/document/d/19QBr6ahoVh3himYXB6Fd0JscC6yhjV9qLyt2fGpdLhk/edit)

-> Here, we will mainly create 2 arrays, prefix and suffix. Prefix will start from the 0th index and prefix\[i\] -> largest subarray sum till ith index
Suffix will start from the (n-1)th index and
Suffix\[i\] -> largest subarray sum from the (i+1)th index.

Prefix array is simply created

For the suffix array, it should be strictly increasing and should not overlap the prefix array elements so it’s a little complicated.

For suffix array, we will have another array big\[n\] because there can be negative values in strictly increasing manner which will reduce the overall sum so big\[i\] -> max (suffix\[i\], arr\[i\]) to avoid this problem and take the smallest negative value in the worst case.

At the end, we need two loops to calculate the prefix\[i\] and suffix\[j\]’s maximum value for each prefix\[i\]. Where tc = O(n^2). In order to avoid this large time complexity, we will create another array called max_suffix where,
Max_suffix\[j\] -> maximum sum for the 2nd subarray till the jth index.
So, we can calculate the final ans in O(n) time iterating through the prefix array only as we will already have the suffix\[j\] value for each prefix\[i\] value.

```cpp
/*
You are given an array, Find the maximum sum of two non overlapping subarrays.
The 2nd subarray must be strictly increasing

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // creating a prefix and suffix subarray
   // prefix[i]-> largest subarray sum till the ith index from the beginning
   // suffix[i] -> largest subarray sum that is strictly increasing for the 2nd subarray

   // prefix array
   vector<int> prefix(n);
   prefix[0] = arr[0];

   for (int i = 0; i < n; i++)
   {
       prefix[i] = max(arr[i], prefix[i - 1] + arr[i]);
   }

   // suffix array which is strictly increasing
   vector<int> suffix(n);

   // this vector will help calculate the final ans in O(n) as opposed to O(n^2)
   vector<int> max_suffix(n); // to keep a tab of the maximum suffix value at the ith index

   vector<int> big(n); // to store the final max suffix in each step as suffix might store -ve numbers

   suffix[n - 1] = arr[n - 1];

   for (int j = n - 2; j >= 0; j--)
   {
       if (arr[j] < arr[j + 1]) // strictly increasing
       {
           suffix[j] = arr[j] + suffix[j + 1];
       }
       else // not strictly increasing
       {
           suffix[j] = arr[j];
       }

       big[j] = max(arr[j], suffix[j]); // avoiding the bigger negative number

       max_suffix[j] = max(big[j], max_suffix[j]);
   }

   // final loop to calculate the ans
   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       int x = prefix[i] + max_suffix[i + 1]; // as the array should not be intersecting
       ans = max(ans, x);
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 4, -1, -10, 7, 6, 12, 13, 14, -15, 16};
   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

### 25) Stable Segments (ZScaler) Very Hard OA

[Stable Segments (ZScaler) Very Hard OA](https://www.desiqna.in/15839/zscaler-oa-2023-sep-set-10)

Formula :
focus_sum : sum of the numbers between the segments
Sum : Total sum of the array till the ith index
p(i) = p(j) - 2\*c(j)
Where p(i) -> focus sum
p(j) -> sum till the jth index
c(j) -> current element

Focus sum = sum - 2\* current element

We will use a map\<pair\<int,int\>,int\> to store
{{sum, element at which the sum occurred}, frequency of the sum}

```cpp
#include <bits/stdc++.h>
using namespace std;
int solve(vector<int> &arr)
{
   int n = arr.size();

   // map<pair<int,int>>mp -> { {sum, element at which the sum occurred}, frequency of the sum}
   map<pair<int, int>, int> mp;

   int sum = 0;
   int count = 0;
   for (int i = 0; i < n; i++)
   {
       sum += arr[i];

       int focus_sum = sum - (2 * arr[i]);
       count += mp[{focus_sum, arr[i]}];

       mp[{sum, arr[i]}]++;
   }
   return count;
}
// 8,4,5,1,2,2,5
// 9,3,3,9
// 9,3,3,3,9
int main()
{

   vector<int> arr = {8, 4, 5, 1, 2, 2, 5, 5, 1, 2, 2, 5};
   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

### 26) Tricky Atlassian OA

[Tricky Atlassian OA](https://docs.google.com/document/d/1f2VzhDu7z0fxBgn5TcpW0xf0wVyPFePNW_Iezlvk_oI/edit)

-> */\**

*A report containing the stock prices for the n days are given to a data analyst to analyze in array StockPrices\[\]*

*The analyst is required to choose a subsequent of stock prices , called ChosenDays\[\]*

*Given the condition :*

*StockPrice\[ChosenDays\[i\]\] - StockPrice\[ChosenDays\[j\]\] == ChosenDays\[i\] - ChosenDays\[j\]*
*which means arr\[i\]-arr\[j\] = i - j*
*So, to simplify,*
*arr\[i\] - arr\[j\] = i - j*
*=\>  arr\[i\] - i = arr\[j\] - j !!! EUREKA!*

*\*/*

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

The code is self explanatory

### 27) Before this, we need to know a few formulas

- **(A+B)%K = (A%K + B%K)%K**
- **If (X\<Y) ; X % Y = X**
- **Rule of Infinity:**
- **(x%k + y%k - z%k + a%k . . . . . . . . )**
- **Let ans = g; then g will always be g=g%k**
- **g = g%k ; g = g%k%k%k%k . . . .**

- **(A\*B)%K = (A%K \* B%K)%K**
- **(A-B)%K = (A%K - B%K +K)%K**

[**Question (Amazon Ireland)**](https://docs.google.com/document/d/1ndmwNjcfyY3kTTou4-olNLki34aBg6xpgBXgxkz_PWk/edit)

**-> /\***
**Given array “A” of size “N”; find the number of pairs in the array (i,j) such that a\[i\] + a\[j\] is divisible by k**

**Eg : -> \[5 2 3 1 1\]**
**k = 3**
**ans : 4**

**\*/**

**\#include \<bits/stdc++.h\>**
**using namespace std;**

**int solve(vector\<int\> &arr, int k)**
**{**
**int n = arr.size();**

**// map to store {arr\[i\]%k , frequency of it}**
**unordered_map\<int, int\> mp;**

**int count = 0;**
**for (int i = 0; i \< n; i++)**
**{**
**int g = k - (arr\[i\] % k); // so that (g + arr\[i\])%k == 0**
**count += mp\[g\];**
**mp\[arr\[i\] % k\]++; // storing this as well as it might be satisfied with other pairs as well**
**}**
**return count;**
**}**

**int main()**
**{**
**vector\<int\> arr = {5, 2, 3, 1, 1};**
**int k = 3;**
**int ans = solve(arr, k);**
**cout \<\< ans \<\< endl;**

**return 0;**
**}**

### 27) Find the subarray with maximum sum such that the first and last elements are…

[Find the subarray with maximum sum such that the first and last elements are the same.  (Google OA )](https://leetcode.com/discuss/interview-experience/1359948/Google-or-Intern-or-HyderabadBanglore-Offer)

```cpp
/*
Given an array of size “N” ; find the maximum sum subarray which has the same elements in the start and end.
Example :-

       [1 8 10 8 -5 8]

       Output :-> 29.

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // map to store {element, sum so far}
   unordered_map<int, int> mp;

   int sumSoFar = 0;
   int ans = -1;
   for (int i = 0; i < n; i++)
   {
       ans = max(ans, arr[i]); // an individual no occurring only once might be very large like 10^7
       if (mp.find(arr[i]) == mp.end())
       {
           // element not found in the map so we insert it
           mp[arr[i]] = sumSoFar;
       }
       else
       {
           // element already exists in the map so we need to find the subarray sum and compare
           // finding the subarray sum
           int x = sumSoFar + arr[i] - mp[arr[i]]; // subarray sum
           ans = max(ans, x);                      // updating the ans if this subarray sum is bigger

           mp[arr[i]] = min(mp[arr[i]], sumSoFar); // the map should contain the element and least subarray sum so far
                                                   // so that when subtracted with the other end, it gives the maximum value
       }
       sumSoFar += arr[i];
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 8, 10, 8, -5, 8};
   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

We will keep a track of sum so far and the current element in the hashmap

Everytime we find the element again, we find the subarray sum from it’s previous occurrence to it’s current occurrence and update the ans array for the maximum subarray sum. In the map, we keep the lowest sumSoFar for each element because according to the required ans,
Subarray_sum \[j\] = sumSoFar + arr\[j\] - (sum till the last occurrence of j)

So, sum till the last occurrence of j has to be minimized! Hence, we maintain the min sumSoFar for each element in the hashmap

### 28) Intuit OA Counting unique pairs

[Intuit OA Counting unique pairs](https://www.desiqna.in/15111/intuit-oa-sde1-2023-may)

-> We will use an unordered_map to store the elements and their frequency.
We will also use another unordered_map\<pair\<int,int\>,int\>visited to store unique pairs ( We can also use adjacency matrix for this )

```cpp
/*
Given an integer k and a list of integers, count the number of distinct valid pairs of integers (a,b)
where a + k = b; Two pairs of integers are said to be distinct if atleast one element of (a,b) does not
belong to (c,d).

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   // map {element, frequency}
   unordered_map<int, int> mp;
   map<pair<int, int>, int> visited;
   for (int i = 0; i < n; i++)
   {
       mp[arr[i]]++;
   }
   for (int i = 0; i < n; i++)
   {
       for (int j = 0; j < n; j++)
       {
           visited[{arr[i], arr[j]}] = 0;
       }
   }

   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       int partner = arr[i] + k;
       if (mp.find(partner) != mp.end())
       {
           // partner found
           if (visited[{arr[i], partner}] == 0)
           {
               ans += 1;
               visited[{arr[i], partner}] = 1;
               visited[{partner, arr[i]}] = 1;
           }
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 1, 1, 2, 2};
   int k = 1;
   int ans = solve(arr, k);
   cout << ans << endl;
   return 0;
}
```

### 29) Atlassian Tricky OA Special map

[Atlassian Tricky OA Special map](https://www.desiqna.in/15241/atlassian-sde-interview-coding-problem-2023) *Given an array of size N ;  find the maximum sum subarray which has a difference of "k"*

*between the start and end elements of the array*
*Eg:*
*N = 8*

*K = 5*

*1 5 -5 8 8 8 10 15*

Here, we will use a tricky hashmap
{element , sum till the previous element} to store the element (arr\[i\]) and sum till arr\[i-1\]

So, subsetSUm = sum till prev element + currentElement - mp\[sum before the matching element\];

*/\**
*Given an array of size N ;  find the maximum sum subarray which has a difference of "k"*
*between the start and end elements of the array*
*Eg:*
*N = 8*

*K = 5*

*1 5 -5 8 8 8 10 15 // prefix sum : 1 , 6, 1, 9, 17, 25, 35, 50*

*Output - \[5 -5 8 8 8 10\] = 34. (also difference b/w start and end element is abs(5-10) = 5*
*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   // we will create a running map a.k.a a map that we will fill during the computation
   // it will contain the elements sum till the previous element
   // map -> {current element, sum till previous element}
   unordered_map<int, int> mp;

   int sum = 0;
   int ans = -1;
   for (int i = 0; i < n; i++)
   {
       int num1 = arr[i] - k;
       int num2 = arr[i] + k;

       if (mp.find(arr[i]) == mp.end())
       {
           // current element is not in the map so we insert it in the map with the previous sum
           mp[arr[i]] = sum;
       }

       if (mp.find(num1) != mp.end())
       {
           int subsetSum = sum + arr[i] - mp[num1];
           ans = max(ans, subsetSum);
           mp[arr[i]] = min(mp[arr[i]], sum); // updating the map for the current element
       }

       if (mp.find(num2) != mp.end())
       {
           int subsetSum = sum + arr[i] - mp[num2];
           ans = max(ans, subsetSum);
           mp[arr[i]] = min(mp[arr[i]], sum); // updating the map for the current element
       }

       sum += arr[i]; // updating the sum of elements at the end and not at the beginning as we want to keep the previous sum
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 5, -5, 8, 8, 8, 10, 15};
   int k = 5;
   int ans = solve(arr, k);
   cout << ans << endl;
   return 0;
}
```

### 30) DE SHAW HARD OA

[DE SHAW HARD OA](https://www.desiqna.in/16043/shaw-coding-question-solution-sde1-september-2023-subarrays) *We are given an array B of size “N”; Divide the array into 4 continuous parts such that g = part1 -part2 +part3 -part4*

*is maximized.*

*Any part is allowed to be empty as well ; but collectively they should cover the whole array*

-> We will only calculate p2 and p4; we will minimize the p2\[i\] + p4\[i\] so that we can get the maximized sum from the expression
P1+p3-p2-p4 =\> (p1+p3)-(p2+p4)
will be maximized if p2 + p4 is minimized
Range of p2 can be from i = 1 to n-1 as P4 can be null as well
Range of p4 can be from i = 1 to n-1

Here’s the code for the problem 👍
*/\**
*We are given an array B of size “N”; Divide the array into 4 continuous parts such that g = part1 -part2 +part3 -part4*
*is maximized.*

*Any part is allowed to be empty as well ; but collectively they should cover the whole array*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   vector<int> p2(n + 1, 0);
   vector<int> p4(n + 1, 0);

   // calculating the total sum of all the elements
   int totsum = 0;
   for (auto it : arr)
   {
       totsum += it;
   }

   // for p2
   for (int i = 1; i < n; i++)
   {
       p2[i] = min(arr[i], arr[i] + p2[i - 1]);
   }

   // for p4
   int sum = 0;
   for (int i = n - 1; i >= 0; i--)
   {
       sum += arr[i];
       p4[i] = min(sum, p4[i + 1]);
   }

   // calculating the min
   int d = INT_MAX;
   for (int i = 0; i < n; i++)
   {
       // x = p2 + p4
       int x = min(0, p2[i]) + min(0, p4[i + 1]);
       d = min(d, x);
   }

   // now, totsum = p1+p2+p3+p4
   // Reqd ans = p1+p3 - p2-p4 => in terms of totsum; => p1+p2+p3+p4 - p2-p4  - p2-p4 => totsum - 2*d
   int ans = totsum - 2 * d;
   return ans;
}

int main()
{
   vector<int> arr = {-3, 4, -5, 2, 6, -5};
   int ans = solve(arr);
   cout << ans << endl;
   return 0;
}
```

### 31) Nice Subarray (Google)

[Nice Subarray (Google)](https://leetcode.com/problems/count-number-of-nice-subarrays/)

-> In the array, we can convert the odd numbers to 1 and even numbers to 0 because we do not need to know the value of the numbers. We only need to know if it is odd or even

Eg: 1 3 2 5 1
->   1 1 0 1 1

We will need an
unordered_map\<int,int\> -> {no Of current Odd elements, no of subarrays with that odd element}

[My Solution](https://leetcode.com/problems/count-number-of-nice-subarrays/submissions/) (code is self explanatory)

### 32) Good Binary String

[Good Binary String](https://www.codechef.com/problems/GOODBINSTR)

![[Hashing Documentation-image2.png]]

![[Hashing Documentation-image3.png]]

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    // your code goes here
    int t;
    cin>>t;
    while (t--)
    {
        string s;
        cin>>s;
        int n = s.size();
        if (s[0] != s[n-1])
        {
            cout<<2<<endl;
        }
        else
        {
            cout<<n-2<<endl;
        }
    }
    return 0;
}
```

# Hashing in 2D matrix

### 33) Completely Painted Row

[Completely Painted Row](https://leetcode.com/problems/first-completely-painted-row-or-column/)

-> Here, we will HASH the values of the matrix with the corresponding values of the arr :
For i in range (1-n)
hash(arr\[i\])=i;

This will help is find the index of the 1D array for the number in the 2d matrix

Since, we traverse the 1D array in a sequential manner,  while painting the element on arr\[i\] we have already painted all the matrix elements till arr\[i-1\];
So, on each row and each column, painting the largest arr index  in that row or column will mean we have painted that entire row or that entire column.

So, for each row and each column, we need to find the largest index present and our ans will be the minimum of those as we need the min arr index on painting who’s element we will get atleast one row/column painted.

-> [My solution](https://leetcode.com/problems/first-completely-painted-row-or-column/submissions/1116467342/)

### 34) Find the sum of all subarrays which are in Arithmetic Progression! Each…

[Find the sum of all subarrays which are in Arithmetic Progression! Each array element is also an Arithmetic Progression!  Difference should only be 0 or 1](https://docs.google.com/document/d/1ofRxtA3WLltEsuTj6vCe0cFEq8G2llmDtxkjqp8EQyw/edit)

```cpp
/* Find the sum of all subarrays which are in Arithmetic Progression!
Each array element is also an Arithmetic Progression!
Difference should only be 0 or 1 */

/*
   Here, At each index ; we smartly calculate the sum of all valid subarrays ending at index “i” ;
   we add this to the final sum.
   We keep a count of the common difference for a range and add their sum accordingly as
   the sum should also contain sum of the individual elements

*/

#include <bits/stdc++.h>
using namespace std;
int solve(vector<int> &arr)
{
   int n = arr.size();
   // first, finding the sum of all the elements of the array
   int totSum = 0;
   for (auto it : arr)
   {
       totSum += it;
   }

   int count = 0;
   int sum = 0;
   int prev = 0;
   int ans = 0;
   // for difference of 1
   for (int i = 0; i < n; i++)
   {
       if (i == 0) // first iteration
       {
           prev = arr[i];
           sum += arr[i];
       }
       else
       {
           if (arr[i] - arr[i - 1] == 1)
           {
               count++;
               sum = prev + (arr[i] * (count + 1));
               prev = sum;
           }
           else
           {
               count = 0;
               sum = arr[i];
               prev = arr[i];
           }
       }
       ans = ans + sum;
   }

   // resetting the values to 0 for the next iteration when difference = 0
   count = 0;
   sum = 0;
   prev = 0;

   // for difference of 0
   for (int i = 0; i < n; i++)
   {
       if (i == 0) // first iteration
       {
           sum += arr[i];
           prev = arr[i];
       }
       else
       {
           if (arr[i] - arr[i - 1] == 0)
           {
               count++;
               sum = prev + (arr[i] * (count + 1));
               prev = sum;
           }
           else
           {
               count = 0;
               sum = arr[i];
               prev = arr[i];
           }
       }
       ans = ans + sum;
   }
   // now, the total sum gets added twice in the ans so we need to remove it one time
   ans -= totSum;
   return ans;
}
int main()
{
   vector<int> arr = {1, 2, 3, 10, 18, 20};
   cout << solve(arr) << endl;
   return 0;
}
```

### 35) Continuous subarray sum

[Continuous subarray sum](https://leetcode.com/problems/continuous-subarray-sum/)

-> [My Solution](https://leetcode.com/problems/continuous-subarray-sum/submissions/1120477944/)

-> Solution description :

![[Hashing Documentation-image4.png]]

### 36) Brick Wall

[Brick Wall](https://leetcode.com/problems/brick-wall/)

-> [My Solution](https://leetcode.com/problems/brick-wall/submissions/1120517676/)
-> We will use an unordered_map\<int,int\> -> {cracksAtIndex, countRows} to store rows where a crack has occurred at that particular index in each row

### 37) Q. Find the count. of subarray of shortest length whose sum==k

**A = {10,5,2,7,1,9,8,7}   k = 15**

**O/p: 2**

Eg. subarrays having sum =15 , {10,5} , {5,2,7,1} , {8,7}
Smallest length of subarray having sum 15 is 2 and there are two such subarrays :

{10,5} \| {8,7}

Hence the count and output is 2.

-> We will store the prefix sum and it’s index of occurrence in a vector first. Then we will use another loop where we will implement sliding window as all the elements are positive integer to find the minimum/maximum subarray length

*/\**
*Q. Find the count. of subarray of shortest length whose sum==k*
*A = {10,5,2,7,1,9,8,7}   k = 15*
*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   // vector to store prefixSum
   // we will implement sliding window after storing the prefix sum as it contains only positive number
   // if the sum >15, window size will decrease from left i.e left++ else right ++
   // storing the prefix sum

   int sum = 0;
   vector<int> pref(n + 1);
   for (int i = 0; i < n; i++)
   {
       sum += arr[i];
       pref[i] = sum;
   }

   // sliding window to find the sum
   int mini = INT_MAX;
   int maxi = INT_MIN;
   int left = 0;
   int right = 0;
   for (int i = 0; i < n; i++)
   {
       if (pref[right] - pref[left] < k)
       {
           right++;
       }
       else if (pref[right] - pref[left] > k)
       {
           left++;
           if (right < left)
               right = left;
       }
       else
       {
           // pref[right]-pref[left] == k
           int len = right - left;
           maxi = max(maxi, len);
           mini = min(mini, len);
           left = right + 1;
           right = left;
       }
   }
   return maxi;
}

int main()
{
   vector<int> arr = {10, 5, 2, 7, 1, 9, 8, 7};
   int k = 15;
   cout << solve(arr, k) << endl;
   return 0;
}
```

### 38) Cheeky Cisco

[Cheeky Cisco](https://docs.google.com/document/d/12EwezVPCqLO51mNGvSA9qSR0sko1VWYcjeuK-PSURiQ/edit)

*/\**
*Design a TCP system.Given a 2d array of size (nX2) containing the time of packet arrival*
*and the no packets arraived, rate : rate of packet consumption per second*
*maxPacket : maximum no of packets the tcp can hold before dropping the*
*additional packets.*
*Output the total no of packets dropped*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<vector<int>> &arr, int rate, int maxPackets)
{
   int n = arr.size();

   // unordered_map to store the arrival time and packets that arrived at that time
   // We will also find the max time will when we will recieve the packets as we do not need to keep a track after that
   int maxTime = INT_MIN;
   unordered_map<int, int> mp;
   for (int i = 0; i < n; i++)
   {
       int t = arr[i][0];
       int packets = arr[i][1];
       mp[t] = packets;
       maxTime = max(maxTime, t);
   }

   // now we need to find the total dropped packets

   int current = 0;
   int ans = 0;
   for (int t = 1; t <= maxTime; t++)
   {
       if (mp[t] >= 1) // packets are received at time 't'
       {
           current = current + mp[t];

           // in case of excess packets
           if (current > maxPackets)
           {
               int drop = current - maxPackets;
               ans += drop;
           }
           // packets disposed per second
           current = current - rate;
       }
       else
       {
           // no packet has arrived so only disposed per second
           current = current - rate;
       }
       if (current < 0)
       {
           current = 0; // As current packets cannot be negative
       }
   }
   return ans;
}

int main()
{
   vector<vector<int>> arr = {
       {1, 8}, {4, 9}, {6, 7}};

   int rate = 2;
   int maxPackets = 10;

   cout << solve(arr, rate, maxPackets) << endl;

   return 0;
}
```

39) [MEX of all subarrays](https://docs.google.com/document/d/1G5w-x-cr9ym6qa9wMNWicVtyQEwClBy8-QCAwiyHtuI/edit)

### 40) Intuit OA

[Intuit OA](https://docs.google.com/document/d/1j_GcVecF7BEkGjmzYouWorFWGQ1vRmOoIe5UEowh0CI/edit)

-> pref1\[i\] -> count of 1s from 1 to i
Pref0\[i\] -> count of 0s from 1 to i

No of 1s from an index i to j = pref1\[j\] - pref1\[i-1\] (Common logic)

Similarly, 0s from an index i to j = pref0\[j\] - pref0\[i-1\]

Required condition :

Zeroes/One  = x/y

Pref0\[j\] - Pref0\[i-1\] / Pref1\[j\] - Pref1\[i-1\]  = x/y

-> (pref0\[j\] - pref0\[i-1\])\*y = x\*(pref1\[j\] - pref1\[i-1\])

(take all the j on the left side AND ALL THE I-1 ON RIGHT SIDE.)

->  pref0\[i-1\]\*y - pref1\[i-1\]\*x = pref0\[j\]\*y - x\*(pref1\[j\]) (RHS)
(This is the condition if b\[i…..j\] is valid)

Now, we can keep a hashmap and calculate the LHS as we move in the map. When we find the same value again, we have found a subarray!

*/\**
*You are given a binary array of 0s AND 1s and 2 integers x and y w*
*Let number of 0s = x and number of 1s = y*
*find the number of subarrays that satisfy :*
*no of zeroes : no of ones =  x : y*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int x, int y, int n)
{

   // creating pref1 and pref0 vectors where
   // pref1[i] -> count of 1s appearing from 0th index till ith index
   // pref0[i] -> count of 1s appearing from 0th index till ith index

   vector<int> pref1(n + 1, 0), pref0(n + 1, 0);
   int one = 0, zero = 0;
   for (int i = 1; i <= n; i++)
   {
       if (arr[i] == 1)
       {
           one++;
           pref1[i] = one;
           pref0[i] = zero;
       }
       else
       {
           zero++;
           pref0[i] = zero;
           pref1[i] = one;
       }
   }

   /*
   EQUATION :
       pref0[j] - pref0[i-1] / pre1[j] - pref1[i-1] = x / y
   =>  y * (pref0[j] - pref0[i-1]) = x* (pre1[j] - pref1[i-1])
       Putting all the js on one side and is on the other side
   =>  y* pref0[j] - x* pref1[j] = y* pref0[i-1] - x*pref1[i-1]

   */
   // We can create a hashmap to store the LHS values and when we find the same value we get a subarray!

   unordered_map<int, int> mp;
   int ans = 0;
   for (int i = 1; i <= n; i++)
   {
       int lhs = y * pref0[i] - x * pref1[i];

       mp[lhs]++;

       int rhs = y * pref0[i - 1] - x * pref1[i - 1];

       ans += mp[rhs];
   }
   return ans;
}

int main()
{
   // for faster input and output
   ios_base::sync_with_stdio(false);
   cin.tie(NULL);

   // code
   int n;
   cin >> n;
   int x, y;
   cin >> x >> y;

   vector<int> arr(n + 1);
   // inserting the elements ;

   for (int i = 1; i <= n; i++)
   {

       cin >> arr[i];
   }

   cout << solve(arr, x, y, n) << endl;
   return 0;
}
```

### 41) Amazon Dublin

[Amazon Dublin](https://docs.google.com/document/d/1hjAR4nG7qiEQW4LpeNOzuAlq5v8ABlk33a6WaswQn7U/edit)

Understanding :-\> You are given an array of N integers; consider all the subarrays of the array ; for each subarray find the count of distinct number in the subarray ; finally output the total count

\[1 2 1\]

-> 9.

Contribution technique -> For each arr\[i\] calculate the number of distinct elements it gives to the subarrays.
At each index ‘i’, we calculate the ‘g’ -> the contribution done by the ith element in all the sub-arrays for the final ans.

We create 2 arrays, one\[\] and two\[\]:

One\[i\] -> the contribution done by arr\[i\] if arr\[i\] was strictly occurring only ones in the subarray from left to right

arr\[i\] , L - - - - arr\[i\] - - - - - -R , arr\[i\]

Formula to find the subarray contribution of arr\[i\] in one\[\] :
Let’s say, we have x elements from L to arr\[i\] and y elements from arr\[i\] to R;
Formula : x\*y + x + y + 1

Now, we only considered the contribution of arr\[i\] when it was coming for 1 time. We did not consider the contribution when it was coming more than 1 times.

Two\[i\] -> he contribution done by arr\[i\] if arr\[i\] was occurring \>=2 times in the subarray from left to right

Front \[i\] -> when the ith element is coming recently on the RHS
Back \[i\] -> when the ith element is coming recently on the LHS

```cpp
/*
Q) Your team at amazon wants to develop a service that takes in an array of integers where the ith element represents the
category of the ith product viewed by the customer and returns the sum of the number of distinct categories on all contiguous
segments of the array
Given an array of n integers, categories, find the sum of the numbers of distinct integers over all of it's subarrays.

Understanding :-> You are given an array of N integers; consider all the subarrays of the array ;
for each subarray find the count of distinct number in the subarray ; finally output the total count

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int n)
{
   vector<int> front(n + 1); // storing the index of the similar elements occurring at the right side
   vector<int> back(n + 1);  // storing the index of the similar elements occurring at the left side

   // map to store element and it's index of first / last occurrence
   unordered_map<int, int> mp;

   // back array
   for (int i = 1; i <= n; i++)
   {
       if (mp.find(arr[i]) == mp.end()) // element not found in the map so it will be the first time from the beginning
       {
           back[i] = 0;
           mp[arr[i]] = i;
       }
       else
       {
           back[i] = mp[arr[i]];
           mp[arr[i]] = i; // updating to the new index of the element
       }
   }

   // clearning the map for the front array
   mp.clear();
   // front array
   for (int i = n; i >= 1; i--)
   {
       if (mp.find(arr[i]) == mp.end()) // element not found in the map so it will be the first time from the end
       {
           front[i] = n + 1;
           mp[arr[i]] = i;
       }
       else
       {
           front[i] = mp[arr[i]];
           mp[arr[i]] = i; // updating to the new index of the element
       }
   }

   int one[n + 1] = {0};

   // one[i] = means the contribution to final sum
   // by b[i] ; considering b[i] is only coming for 1 time.
   // in those subarrays!
   int two[n + 1] = {0};
   // two[i] = means the contribution to final sum
   // by b[i] ; considering b[i] is coming for >=2 times;
   // also one more thing is assumed; only those subarrays.
   // where b[i] is coming for the first time!
   // other guys same as b[i] are on the left

   int ans = 0;

   // for one[i] array
   for (int i = 1; i <= n; i++)
   {
       int x = abs(i - back[i]) - 1;  // no of elements before the ith index in the array
       int y = abs(front[i] - i) - 1; // no of elements after the ith index in the array

       one[i] = (x * y) + x + y + 1;
       ans += one[i];
   }

   // for two[i] array
   for (int i = 1; i <= n; i++)
   {
       if (back[i] != 0)
       {
           int x = back[i]; // no -1 as we are taking in the last occurrence of arr[i] itself!
           int y = abs(front[i] - i) - 1;

           two[i] = x * y;
       }

       ans += two[i];
   }

   cout << "Printing the back array :" << endl;
   for (auto it : back)
   {
       cout << it << " ";
   }
   cout << endl;
   cout << "Printing the FRONT array :" << endl;
   for (auto it : front)
   {
       cout << it << " ";
   }
   cout << endl;

   return ans;
}

int main()
{
   int n;
   cin >> n;
   vector<int> arr(n + 1);
   for (int i = 1; i <= n; i++)
   {
       cin >> arr[i];
   }
   cout << solve(arr, n) << endl;
   return 0;
}
```

### 42) Amazon OA

![[Hashing Documentation-image5.png]]

![[Hashing Documentation-image6.png]]

-> We will use a set to store the data locations. The we will iterate in a loop the moved_from and moved_to array. We will check if moved_from element is present in the set we will change it to moved_to

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> solve(vector<int> &data, vector<int> &from, vector<int> &to)
{
   int n = data.size();
   int m = from.size();

   // set -> store the elements
   unordered_set<int> st;
   for (int i = 0; i < n; i++)
   {
       st.insert(data[i]);
   }

   vector<int> ans;
   for (int i = 0; i < m; i++)
   {
       if (st.find(from[i]) != st.end())
       {
           st.erase(from[i]);
           st.insert(to[i]);
       }
   }
   for (auto it : st)
   {
       ans.push_back(it);
   }
   sort(ans.begin(), ans.end());
   return ans;
}

int main()
{
   ios_base::sync_with_stdio(false);
   cin.tie(NULL);

   vector<int> data = {1, 7, 6, 8};
   vector<int> from = {1, 7, 2};
   vector<int> to = {2, 9, 5};

   vector<int> ans = solve(data, from, to);

   for (auto it : ans)
   {
       cout << it << " ";
   }
   cout << endl;

   return 0;
}
```

### 43) Count vowel strings in range

[Count vowel strings in range](https://leetcode.com/problems/count-vowel-strings-in-ranges/description/)

-> [My Solution](https://leetcode.com/problems/count-vowel-strings-in-ranges/submissions/1125696580/)

-> We will create an array to store if a particular string in the satisfies the condition ans mark it as 1 if it does else we will mark it as 0;

We will then create the prefix array to store prefix sum of the above array so that we can avoid running a loop and calculating for each query.

If the query starts from the 0th index
Ans : prefix_sum \[right\];
Else
Ans : prefix_sum \[right\] - prefix_sum \[left - 1\]

### 44) ZScaler Hard OA

[ZScaler Hard OA](https://www.desiqna.in/13097/zscaler-oa-sde1-2023-coding-questions-and-solutions-april)

-> for maximum profit, we take the subarray P\[i\] - P\[i-1\]. For this, P\[i-n\] must be smallest. To keep a track of the smallest P\[i-n\], we use a priority_queue \<pair\<int,int\>\> minheap to store -> {prefixSum, index}

*/\**
*Given an array of stock price, find the maximum profit subarray sum with the max possible subarray length 'k'*

*\*/*

```cpp
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr, int k)
{
   int n = arr.size();
   vector<int> prefix(n);

   // finding out the prefix sum
   prefix[0] = arr[0];
   for (int i = 1; i < n; i++)
   {
       prefix[i] = arr[i] + prefix[i - 1];
   }

   // creating a priority queue (minheap) <pair<int,int>> {prefixSum, index} to store the smallest prefix sum for calculation
   priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> minheap;
   int len = -1;
   minheap.push({0, 0});

   int ans = INT_MIN;
   for (int i = 0; i < n; i++)
   {
       int r = i;
       int l = i - k; //  for lower range since only k length subarrays are allowed
       int mini = 1e8;
       int kk = 0;
       while (!minheap.empty() && kk == 0)
       {
           int num = minheap.top().first;
           int index = minheap.top().second;
           // cout << "num: " << num << endl;
           // checking if the index is valid i.e if it lies within k length
           if (index >= l && index <= r) // TODO: correction
           {
               mini = min(mini, num);
               ans = max(ans, prefix[i] - prefix[index]);
               kk = 1;
           }
           else
           {
               // the element is out of range so we do not need it in the queue anymore
               minheap.pop();
           }
       }

       // pushing the current element
       minheap.push({prefix[i], i});
   }
   return ans;
}

int main()
{
   vector<int> arr = {-3, 4, 3, -2, 2, 5};
   vector<int> arr2 = {4, 3, -2, 9, -4, 2, 7
   };

   int k = 4;
   cout << solve(arr, k) << endl;
   cout << solve(arr2, 6) << endl;
   return 0;
}
```

### 45) DE Shaw Subarray Removal Hard

[DE Shaw Subarray Removal Hard](https://www.desiqna.in/13234/de-shaw-sde-super-hard-oa-2023) \*\*\* Hard and complicated

-> we will require two arrays ; increasing \[i\] and decreasing\[i\]

Increasing\[i\] -> true if the numbers are increasing till the ith index else it is false
Decreasing\[i\] -> true if the numbers are decreasing till the ith index else it is false;

We will also need a set \<pair\<int,int\>\>  to store values and their indices

Eg :

\[1 2 3 4 5 8 -1 -3 6 7\]
\[1 2 3 4 5 6 7 8 9 10\]

—\> Suppose you are at index 9 ; b\[9\] = 6 ;

—\> push all values with their indices in the set whose prefix(i) == true) and i\<=8
—-\> have to use a set of pairs
{
1,1
2,2
3,3
4,4
5,5
8,6}
Set (value,index)

—\> do upper bound of (b\[i+1\])===upper_bound(7) ===\> (8,6)==\> now just decrement 1 to get the guy just \<=7 ===\> (5,5)====\>now catch the 5

-> It means everything is good from (1…..5) and (6……7)
-> So it means everything is good from (1…..4) and (6….7)

-> So it means everything is good from (1…..3) and (6….7)

-> So it means everything is good from (1…..2) and (6….7)

-> So it means everything is good from (1…..1) and (6….7)

—\> you go at each index ; you try to find the smallest subarray which is valid ending at index i using upper bound function hence you get j ; this means u can remove stuff from \[j+1 to i\] carefree ; similarly u can do for \[j to i\] ; —\> \[j-1\] to i\] …….\[2….i\]....\[1….i\]

TC : O(N\*logN) as at index i ; you use upper bound function which takes log(N) time per index ; also storing all elements in the sets takes log(N) time per element ; so total time is NlogN + NlogN = 2Nlogn = O(NlogN) okjii

```cpp
/*
Given an array of n integers, find the number of it's subarray such that removing the subarray creates a non-empty
array that is sorted in increasing order.
*/
#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   // set {int,int} to store the element and it's index
   set<pair<int, int>> st;

   // increasing[i] -> 1 if all the elements till ith index is increasing. Else it is 0
   vector<int> increasing(n, 0);
   increasing[0] = 1;
   for (int i = 1; i < n; i++)
   {
       if (arr[i] > arr[i - 1] && increasing[i - 1] == 1)
       {
           increasing[i] = 1;
       }
   }

   // decreasing[i] -> 1 if all the elements till ith index is decreasing. Else it is 0
   vector<int> decreasing(n, 0);
   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       int count = 0;
       auto it = st.upper_bound({arr[i + 1], 1e8}); // 1e8 because we need to search for an index and it will always be smaller than 1e8
       if (it != st.begin())                        // it is not the first element in st
       {
           --it;
           pair<int, int> p = (*it); // getting the index and value of the largest element where increasing[it] = 1
           int index = p.second;
           count = count + index; // because all elements from 0 to index have increasing = 1;
       }

       // if the next index is also increasing and it is not the last element
       if (increasing[i + 1] == 1 and (i + 1) < n)
       {
           count++;
       }

       ans = ans + count;

       // creating the decreasing array and inserting elements in the set accordingly
       if (i >= 1 && arr[i - 1] <= arr[i] && decreasing[i - 1] == 1)
       {
           decreasing[i] = 1;
           st.insert({arr[i], i});
       }
       if (i == 0) // first element
       {
           decreasing[i] = 1;
           st.insert({arr[i], i});
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 2, 1, 2};
   cout << solve(arr) << endl;
   return 0;
}
```

### 46) Google OA

```cpp
/*
Given an array of size "N" ; find the number of qudruplets ; such that ; i<j<k<l and

a[i] < a[j] < a[k] < a[l]

1<=N<=1000

1<=A[i]<=100000000
*/

/*
Solution description :
Given : a[i] < a[j] < a[k] < a[l]

we can find a[j] such for all the elements such that they are before/left of a[j] and are smaller than a[j]
   let the number of elements be k2;

Similarly, we can find a[k] for all elements such that all the numbers after/right of a[k] are greater than a[k]
   let the number of elements be k1;

So, for each index i, k1[i] * k2[i+1] will give us the number of quadruplets for that index

*/

#include <bits/stdc++.h>
using namespace std;

int solve(vector<int> &arr)
{
   int n = arr.size();

   vector<int> k1(n, 0);
   for (int i = 0; i < n; i++)
   {
       for (int j = i + 1; j < n; j++)
       {
           if (arr[i] < arr[j])
           {
               k1[i]++;
           }
       }
   }

   cout << "Printing k1 \n";
   for (auto it : k1)
   {
       cout << it << " ";
   }
   cout << endl;

   vector<int> k2(n, 0);

   for (int i = n - 1; i >= 0; i--)
   {
       for (int j = i - 1; j >= 0; j--)
       {
           if (arr[i] > arr[j])
           {
               k2[i]++;
           }
       }
   }

   cout << "Printing k2 \n";
   for (auto it : k2)
   {
       cout << it << " ";
   }
   cout << endl;

   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       for (int j = i + 1; j < n; j++)
       {
           if (arr[i] < arr[j])
               ans = ans + (k2[i] * k1[j]);
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {1, 2, 3, -5, 4, 8};
   cout << solve(arr) << endl;
   return 0;
}
```

### 47) Amazon India.. Ultra Hard OA

[Amazon India.. Ultra Hard OA](https://docs.google.com/document/d/1-TYMJDQNpwvSCXEwv4oRC_xui9Odl4kueMJ4Ue6oXIc/edit)

```cpp
#include <bits/stdc++.h>

using namespace std;

unordered_map<char, int>start;

unordered_map<char,int>en;

vector<vector<int>> freq(100, vector<int>(26,0));

int n;

// function to count if the character is occuring outside / inside the substring

int count(int left, int right, char ch)

{

    int cnt = 0;

    int c = int(ch)-96;

    if (left == 0)

    {

        cnt = freq[right][c];

    }

    else

    {

        cnt = freq[right][c] - freq[left-1][c];

    }

    return cnt;

}

bool check (char c1, char c2)

{

    int left = start[c1];

    int right = en[c2];

    if (left <= right)

    {

        for (char ch = 'a'; ch <='z'; ch++)

        {

            int inside = count(left, right, ch);

            int outside;

            if (left > 0)

                outside= count(0, left-1,ch) + count(right+1,n-1, ch);

            else

                outside= count(right+1,n-1, ch);

            // if it is occurring both outside and inside, it is invalid. It should occur only inside the substring

            if (inside >=1 && outside >=1)

                return false;

        }

        return true;

    }

    return false;

}

int length(char c1, char c2)

{

    return abs(en[c2] - start[c1])+1;

}

int solve (string s)

{

    n = s.size();

    // getting the last index of each character in the string

    // We are creating a 2d matrix of [sizeofString][26] to store the prefix sum frequency of each character

    for (int i = 0; i<n; i++)

    {

        char ch = s[i];

        en[ch] = i;

        int c = int(ch)-96;

        freq[i][c] = 1;

    }

    // getting the first index of each character in the string

    for (int i  = n-1; i>=0; i--)

    {

        char ch = s[i];

        start[ch] = i;

    }

    // adding the frequency to get the prefix sum frequency of each charater at any given index i

    for (int i = 0; i<n; i++)

    {

        for (int j = 0; j<26; j++)

        {

            if (i>0)

                freq[i][j] += freq[i-1][j];

        }

    }

    int ans = INT_MIN;

    for (char c1 = 'a'; c1 <='z'; c1++)

    {

        for (char c2 = 'a'; c2<='z'; c2++)

        {

            // checking if the string is valid whrn the starting character is c1 and the ending character is c2

            if (check(c1, c2) == true)

            {

                if (length(c1, c2) != n)

                    ans = max (ans, length(c1, c2));

            }

        }

    }

    return ans;

}

int main() {

    string s = "amazonoa";

    cout<<solve(s)<<endl;

    return 0;

}
```

### 48) Amazon HackON OA

```cpp
/*
Problem Statement

You are given an array A of N positive integers. The function F(X,Y) is defined as the number obtained by concatenating the decimal representations of X and Y. For example, F(300,90) = 30090 and F(1,1001) = 11001.

Your task is to find the sum of all possible values of F(Ai, Aj) for all pairs (i, j) such that 1 ≤ i < j ≤ N.

In other words, compute the sum:

F(A1,A2) + F(A1,A3) + ... +
F(A2,A3) + F(A2,A4) + ... +
F(AN-1,AN)

Since the answer can be very large, return the answer modulo 998244353.

Input Format

The first line of input contains a single integer N.

The second line of input contains N space-separated integers representing the array A.

Output Format

Print a single integer representing the final sum modulo 998244353.

Sample Testcase #0
Testcase Input
3
3 14 15
Testcase Output
2044
Explanation

In this case, we have an array of 3 elements:

[3, 14, 15]

The possible pairs are:

F(3,14)  = 314
F(3,15)  = 315
F(14,15) = 1415

Therefore:

314 + 315 + 1415 = 2044

So the answer is:

2044

*/

#include <bits/stdc++.h>
using namespace std;

int countDigits(int n)
{
    return to_string(n).length();
}

int main()
{
    vector<int> arr = {3, 14, 15};
    int n = arr.size();
    int power = 1;
    int ans = 0;
    int effectSum = 0;
    for (int i = 0; i < n; i++)
    {
        int digits = countDigits(arr[i]); // count the number of digits in the current element
        /*
            val till the ith index -> number of times arr[i] was used in the previous indices + sum of all the numbers before arr[i] * 10^(number of digits of arr[i])

            basically, when we are in the ith index, we calculate the effective sum arr[i] had contributed till i-1th index.
        */
        int val = (i * arr[i]) + pow(10, digits) * effectSum;
        ans += val;
        effectSum += arr[i];
    }
    cout << ans << endl;
    return 0;
}
```

### 49)

```cpp
/*
Problem Statement

Given an array of positive integers a, your task is to calculate the sum of every possible a[i] ∘ a[j], where:

a[i] ∘ a[j] is the concatenation of the string representations of a[i] and a[j] respectively.

Example 1

For a = [10, 2], the output should be:

solution(a) = 1344

Calculations:

a[0] ∘ a[0] = 10 ∘ 10 = 1010
a[0] ∘ a[1] = 10 ∘ 2  = 102
a[1] ∘ a[0] = 2  ∘ 10 = 210
a[1] ∘ a[1] = 2  ∘ 2  = 22

So:

1010 + 102 + 210 + 22 = 1344
Example 2

For a = [8], the output should be:

solution(a) = 88

There is only one number in a:

a[0] ∘ a[0] = 8 ∘ 8 = 88

Therefore, the answer is 88.

Example 3

For a = [1, 2, 3], the output should be:

solution(a) = 198

Calculations:

a[0] ∘ a[0] = 1 ∘ 1 = 11
a[0] ∘ a[1] = 1 ∘ 2 = 12
a[0] ∘ a[2] = 1 ∘ 3 = 13

a[1] ∘ a[0] = 2 ∘ 1 = 21
a[1] ∘ a[1] = 2 ∘ 2 = 22
a[1] ∘ a[2] = 2 ∘ 3 = 23

a[2] ∘ a[0] = 3 ∘ 1 = 31
a[2] ∘ a[1] = 3 ∘ 2 = 32
a[2] ∘ a[2] = 3 ∘ 3 = 33

Therefore:

11 + 12 + 13 + 21 + 22 + 23 + 31 + 32 + 33 = 198
*/

#include <bits/stdc++.h>
using namespace std;

int countDigit(int n)
{
   return to_string(n).length();
}

int main()
{
   vector<int> arr = {10, 2};
   int n = arr.size();

   /*
   15, 1, 20, 3, 4
   15-> 1515 + 151 + 1520 + 153 + 154
   -> 15*10^2 + 15*10^1 + ... + totalSum

   1-> 151 + 11 + 120 + 13 + 14
   -> 1*10^2 + 1*10^1 +...+ totalSum

   So, ans for each arr[i]: totalSum + arr[i]*(10^2 + 10^1 + 10^2 + 10^1 + 10^1)

   final ans: sum of ans for each arr[i]
   */

   // calculating the total sum
   int totalSum = 0;
   for (int i = 0; i < n; i++)
   {
       totalSum += arr[i];
   }

   // calculating the powers via digit
   int powerSum = 0;
   for (int i = 0; i < n; i++)
   {
       int digits = countDigit(arr[i]);
       powerSum += pow(10, digits);
   }
   int ans = 0;
   for (int i = 0; i < n; i++)
   {
       ans += totalSum + (arr[i] * powerSum);
   }
   cout << ans << endl;
   return 0;
}
```

### 50) Atlassian OA

```cpp
/*
Atlassian tracks various project activities over time, such as issues created, tasks completed, or deployments made each day.
You are given an array representing the activity recorded on consecutive days.
A consistent activity streak is a contiguous sequence of days where the change in activity between every pair of consecutive days remains the same.
Your task is to determine the maximum length of a contiguous consistent activity streak in the given array.
The streak must contain at least two days.
Building on the previous activity-streak problem, suppose Atlassian now allows you to modify at most one day's activity value in the data.
You are given the same array representing activity recorded on consecutive days. You may replace at most one element of the array with any integer you choose, including a negative integer.
A consistent activity streak is a contiguous sequence of days where the change in activity between every pair of consecutive days remains the same.
Your task is to determine the maximum possible length of a contiguous consistent activity streak that can be obtained after replacing at most one element in the original array.
You may choose not to modify the array if the existing data already contains the optimal streak.

*/

#include <bits/stdc++.h>
using namespace std;

int solve(const vector<int> &arr)
{
   int n = arr.size();

   vector<int> pref(n);
   vector<int> suff(n);

   // pref[i] -> count of same common difference for the ith index starting from the first element;
   pref[0] = 1; // count is one for single element
   pref[1] = 2;
   for (int i = 2; i < n; i++)
   {
       if (arr[i] - arr[i - 1] == arr[i - 1] - arr[i - 2])
       {
           pref[i] = pref[i - 1] + 1;
       }
       else
       {
           pref[i] = 2;
       }
   }

   // suff[i] -> count of same common difference for the ith index starting from the last element;
   suff[n - 1] = 1; // count is one for single element
   suff[n - 2] = 2;
   for (int i = n - 3; i >= 0; i--)
   {
       if (arr[i + 2] - arr[i + 1] == arr[i + 1] - arr[i])
       {
           suff[i] = suff[i + 1] + 1;
       }
       else
       {
           suff[i] = 2;
       }
   }
   int ans = 2; // minimum value that it can have

   // existing best without modifying any element
   for (int i = 1; i < n; i++)
   {
       ans = max(ans, pref[i] + 1); // +1 to include the current element
   }

   for (int i = 0; i < n; i++)
   {
       if (i >= 1)
           ans = max(ans, pref[i - 1] + 1);

       if (i < n - 1)
           ans = max(ans, suff[i + 1] + 1);

       if (i >= 1 && i < n - 1)
       {
           int difference = arr[i + 1] - arr[i - 1];

           if (difference % 2 != 0)
           {
               continue;
           }

           int d = difference / 2;

           // a[i] can be modified to d to join both the sides and get maximum length

           bool leftOK = (i - 2 < 0) || (arr[i - 1] - arr[i - 2] == d);
           bool rightOK = (i + 2 >= n) || (arr[i + 2] - arr[i + 1] == d);

           if (leftOK && rightOK)
           {
               int left = (i - 2 >= 0) ? pref[i - 1] : 1;
               int right = (i + 2 < n) ? suff[i + 1] : 1;
               ans = max(ans, left + 1 + right);
           }
       }
   }
   return ans;
}

int main()
{
   // vector<int> arr = {5, 5, 4, 5, 5, 5, 4, 5, 6};
   vector<int> arr = {10, 7, 4, 6, 8, 10, 11};

   cout << solve(arr) << endl;
   return 0;
}
```
