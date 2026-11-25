The **Greedy Exchange Argument Lemma** is a mathematical technique used to prove that a greedy algorithm produces an optimal solution.

The central idea is:

> If an optimal solution does not make the greedy choice, we can modify it to include the greedy choice without making the solution worse.

>In other words, we can exchange a decision made by an optimal solution with the greedy decision while preserving optimality.

Let's understand this step by step.

## The basic intuition

Suppose you have a problem where you need to make a sequence of choices.

- A greedy algorithm makes the locally best choice at every step.
- An optimal algorithm makes choices that produce the globally best answer.

The challenge is proving that the locally best choice is actually safe.

The exchange argument does exactly that.

We start with an optimal solution and gradually transform it into a solution that follows our greedy choices.

If we can do this without worsening the answer, our greedy algorithm must also be optimal.

1) <https://www.codechef.com/problems/TADELIVE?tab=statement>

### 2) #IBM OA

```cpp
/*Given an array of size N; and a target -> find the minimum number of operations needed to make all elements of array equal to target -> there are multiple targets in Q and each target is indivitual i.e once one target is completed, the array resets to it's original numbers before proceeding with the next number;
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

void solve(vector<int> arr, const vector<int> &queries)
{
   int n = arr.size();

   sort(arr.begin(), arr.end());

   /*
       If target > all numbers in the array ->(target > max element of array)
       -> answer = target*n - sum; =>O(1)

       If target < all numbers in the array ->(target < min element of the array)
       -> answer = sum - target*n;
   */

   int sum = 0;

   vector<int> prefSum(n, 0);
   prefSum[0] = arr[0];
   for (int i = 1; i < n; i++)
   {
       prefSum[i] = prefSum[i - 1] + arr[i];
   }
   for (int i = 0; i < queries.size(); i++)
   {
       int ans = 0;
       int q = queries[i];

       // we can binary search to check for numbers < q and numbers > q in the array.
       // We also need prefix sum of the array to help us calculate the sum till ith index where till ith index, the elements are <q or > q

       int lessThenIndex = -1;
       int left = 0;
       int right = n - 1;
       while (left <= right)
       {
           int mid = left + (right - left) / 2;
           if (arr[mid] < q)
           {
               lessThenIndex = mid;
               left = mid + 1;
           }
           else
           {
               right = mid - 1;
           }
       }

       int greaterThenIndex = -1;
       left = 0;
       right = n - 1;
       while (left <= right)
       {
           int mid = left + (right - left) / 2;
           if (arr[mid] > q)
           {
               greaterThenIndex = mid;
               right = mid - 1;
           }
           else
           {
               left = mid + 1;
           }
       }

       int lessThenSum = (lessThenIndex != -1) ? prefSum[lessThenIndex] : 0;
       int greaterThenSum = (greaterThenIndex != -1) ? prefSum[n - 1] - prefSum[greaterThenIndex - 1] : 0;

       // less count and greater count will tell how many elements are less than and greater than q.
       // need this value to calculate the effect of q on elements smaller and larger than q.
       int lessCount = lessThenIndex + 1;
       int greaterCount = n - greaterThenIndex;

       ans += (q * lessCount) - lessThenSum;
       ans += greaterThenSum - (q * greaterCount);
       cout << ans << endl;
   }
}

int main()
{
   vector<int> arr = {1, 2, 3, 4, 5};
   vector<int> queries = {3, 2};

   solve(arr, queries);
   return 0;
}
```

3) <https://www.codechef.com/problems/SHOOT0>
### 3) #GoldmanSachs OA

```cpp
/*
In the icy realm of Antarctica, there is an annual gathering of penguins to celebrate the onset of winter. There are N male penguins and N female penguins attending the grand dance party this year.

In choosing their dance partners (partner is always from the opposite gender), penguins give the utmost importance to their partner's height.

Some penguins will only dance with a partner who is taller, while the other penguins will only dance with a partner who is shorter. No penguin will dance with a partner who is of the same height as themselves.

To indicate this preference, the height of a penguin is mentioned as a positive value if the penguin wishes to have a partner who is taller than themselves, and alternatively as a negative value if the penguin wishes to have a partner who is shorter than themselves.

To create harmony and joy at the grand dance, your task is to respect the wishes of each penguin and determine the maximum number of dancing pairs that can be formed. Each penguin can have, at most, one dancing partner.

Your goal is to pair as many of them as possible, in a way that fulfills their height preferenc
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(vector<int> males, vector<int> females)
{
   int n = males.size();

   /*
   We can create a solution where we have 4 vectors, negative and positive male, negative and positive females.

   The negative male and positive female always have a chance to bond and vice versa giving us the maximum count
   */

   vector<int> posMale, negMale, posFemale, negFemale;

   for (int i = 0; i < n; i++)
   {
       if (males[i] < 0)
       {
           negMale.push_back(males[i]);
       }
       else
       {
           posMale.push_back(males[i]);
       }

       if (females[i] < 0)
       {
           negFemale.push_back(females[i]);
       }
       else
       {
           posFemale.push_back(females[i]);
       }
   }
   // sorting the negatives in descending and positives in ascending will give us the highest result here.

   sort(negMale.rbegin(), negMale.rend());
   sort(posMale.begin(), posMale.end());
   sort(negFemale.rbegin(), negFemale.rend());
   sort(posFemale.begin(), posFemale.end());

   int i = 0;
   int j = 0;
   int ans = 0;
   while (i < negMale.size() && j < posFemale.size())
   {
       if (abs(negMale[i]) > posFemale[j]) // its a jodi
       {
           ans++;
           i++;
           j++;
       }
       else
       {
           i++;
       }
   }

   i = 0;
   j = 0;

   while (i < posMale.size() && j < negFemale.size())
   {
       if (abs(negFemale[j]) > posMale[i]) // its a jodi
       {
           ans++;
           i++;
           j++;
       }
       else
       {
           j++;
       }
   }
   return ans;
}

int main()
{
   vector<int> males = {-1900, -2000, -2500, 1500, 1600, 2500, -2500};
   vector<int> females = {1800, -1550, 2200, -1550, 2100, -2500, -1700};
   cout << solve(males, females) << endl;
   return 0;
}
```

### 4) #Atlassian OA

```cpp
/*
You’re working on a simulation tool at Atlassian that manages a network of islands connected by bridges. Initially, every island is connected to every other island. Each island has an "importance value" associated with it.
A malicious agent, Alex (an Atlassian engineer), can destroy some bridges between islands, but only if the total destruction cost remains within a given budget. The cost to destroy the bridge between island i and island j is importance[i] * importance[j].
Your colleague Jordan (an Atlassian employee) lives on island 1 and enjoys visiting other islands. After Alex destroys some bridges optimally to disconnect the network, we want to determine how many islands Jordan can still access, including his own. Alex wants to minimize this number
🧩 Problem Statement: You are given:
An integer N — the number of islands.
An array A of length N, where A[i] is the importance of the i-th island (1-indexed).
A long integer C — the maximum total cost Alex can spend to destroy bridges.

Each island is initially connected to every other island. Your task is to compute the minimum number of islands (including island 1) that Jordan can access after Alex destroys bridges optimally (to minimize the size of the connected component that contains island 1).
🧾 Input:
First line: integer T — number of test cases.

For each test case:
Line 1: Two integers N and C.

Line 2: N integers A[1] A[2] ... A[N] — importance values.
📤 Output: For each test case, output the minimum number of islands (including island 1) Jordan can visit
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

ll solve(vector<ll> arr, ll c)
{
   ll n = arr.size();

   // to remove an island i, we need to do the following calculation:
   /*
      ( arr[i]*arr[0] + i*arr[1] + ... + i*arr[n-1] ) - arr[i];

      => arr[i]* (arr[0] + arr[1] +...+arr[n-1] - arr[i];

      => arr[i]*(totalSum-arr[i]);
      so, we need the total sum of all the elements
   */
   ll totalSum = 0;
   for (auto it : arr)
   {
       totalSum += it;
   }

   // we do not need the first element so we are creating a new array without it.
   vector<ll> temp(arr.begin() + 1, arr.end());

   /*
       Now, for more than 1 island removal,
       formula for 2 islands -> (arr[i] + arr[j])*(sum-(arr[i]+arr[j])

       let arr[i]+...+arr[j] -> y where y is the sum of the nodes to be removed

       formula: y*(totalSum-y)

       -> This creates a parabola where TotalSum is constant. We need to find the minimum value
       for a parabola, we get the minimum value (0) when y = totalSum or y = 0;
       -> in this case, y has to be as close to totalSum as possible or as far away from totalSum as possible.
       -> To achieve this, we need to sort the array can calculate for every possible length of y where
       y-> smallest values from the beginning of size length and also y-> largest values from the end of size length
   */

   ll m = temp.size();
   sort(temp.begin(), temp.end());

   vector<ll> prefSum(m + 1, 0);

   for (ll i = 1; i <= m; i++)
   {
       prefSum[i] = prefSum[i - 1] + temp[i - 1];
   }
   ll ans = 0;
   for (ll length = 1; length <= m; length++)
   {
       // we will not remove node 1 so never take it into consideration. We start out j pointer from i = 1;
       ll partSum1 = prefSum[length];                  // sum of smallest values, first length number of values
       ll partSum2 = prefSum[m] - prefSum[m - length]; // sum of largest values, first length number of values.

       ll cost1 = partSum1 * (totalSum - partSum1); // general formula
       ll cost2 = partSum2 * (totalSum - partSum2); // general formula

       ll cost = min(cost1, cost2);

       if (cost <= c)
       {
           ans = length;
       }
   }
   return n - ans;
}

int main()
{
   int t;
   cin >> t;
   while (t--)
   {
       ll n, c;
       cin >> n >> c;

       vector<ll> arr(n, 0);

       for (ll i = 0; i < n; i++)
       {
           cin >> arr[i];
       }

       cout << solve(arr, c) << endl;
   }

   return 0;
}
```

### 5) #Atlassian OA String value sum

```cpp
/*
Atlassian has a text collaborative tool called Confluence. Let's assume the text editor gets an array of strings. Each string should be considered as a zero-indexed array of characters.

All characters within the strings will fall within the ASCII range of lowercase letters (a-z), with decimal values ranging from 97 to 122. These decimal values are known as ordinal values and will be denoted as ord[a] - 97, for instance.

Given an array of strings s = [s[0], s[1], ..., s[n-1]] and an integer m, we calculate a value of each s[i] of length len(s[i]) as:
[
value[i] = ord[s[i][0]]^m * ord[s[i][1]]^m
* ....... times ord[s[i][len(s[i])-1]]^m
]

Perform the calculation on each string, sum them up, and determine whether their sum is EVEN or ODD.
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

void solve(vector<string> s)
{
   int n = s.size();

   int oddCount = 0;

   /*
       When m = 10^9; integer overflow and time complexity will explode; hence calculating actual value of vl is not good;

   -> only check if vl will be even or odd.

   -> (odd)^m ; m>=2 -> is always odd.
   -> odd*odd = odd;

   -> if even a single character is even; it means vl is even;

   -> for each string in an instant we know if vl is even or odd.

   -> How to know what will be the result of their final sum.

   -> even + even = even.

   -> odd + odd = even.

   ->even + odd. = odd.

   -> If you have vl coming odd -> odd number of times; then final sum will always be odd only

   */

   for (int i = 0; i < n; i++)
   {
       bool allOdd = true;

       for (int j = 0; j < s[i].size(); j++)
       {
           int num = (int)s[i][j]; // converting the character to int

           if (num % 2 == 0)
           {
               allOdd = false;
               break;
           }
       }
       if (allOdd)
       {
           oddCount++;
       }
   }

   if (oddCount % 2 != 0)
   {
       cout << "odd" << endl;
   }
   else
   {
       cout << "even" << endl;
   }
}

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   vector<string> s(n);

   for (int i = 0; i < n; i++)
   {
       cin >> s[i];
   }

   // we do not need the value of m LOL!
   solve(s);
   return 0;
}
```

### 6) #Salesforce OA

```cpp
/*
In Salesforce's annual hackathon, employees from a team are ranked based on their efficiency scores. These scores are represented as an array. The team lead wants to analyze subgroups of employees to identify groups with specific characteristics.

Given:
- n employees in a team standing in a line.
- An array efficiency representing each employee's efficiency.
- An integer k (1 ≤ k ≤ n), representing the reference employee in the array.
Find the number of subarrays of odd lengths with a median equal to efficiency[k].
Note:
- A subarray is a sequence of consecutive elements of the array.
- The median of an array of odd length, say n, is the $(n+1)/2$th element of the array if sorted in non-decreasing order.
For example, the median of [2, 5, 4, 1, 1, 1, 6] of length 7 is 2, since upon sorting, the array becomes [1, 1, 1, 2, 4, 5, 6] and the $(7+1)/2=4$th element is 2.
Example
Input:
efficiency = [5, 3, 1, 4, 7, 7]
k = 4

efficiency[4] = 4

There are 4 odd-length subarrays with 4 as their median:
1. [4]
2. [1, 4, 7]
3. [5, 3, 1, 4, 7]
4. [3, 1, 4, 7, 7]
Output:
4
*/

#include <bits/stdc++.h>

using namespace std;

#define ll long long

int solve(vector<int> &arr, int k)
{
   int n = arr.size();

   int target = arr[k - 1]; // k is 1 based indexing here

   // arr[k-1] -> 0. elements lesser than arr[k-1] -> -1 and elements greater than arr[k-1] -> 1.
   // we are taking k-1 because k given in question is 1 based indexing
   /*
       We do not care about the elements.We only care about whether they are strictly smaller or strictly greater than arr[k-1].
       now, we calculate how many subarrays have the current prefix sum. we store the prefix sum in a hashmap and iterate the temp array.
       This algorithm also count the even length subarrays whose sum is 0

       -> We should only count odd length subarrays whose sum is 0

       General Algorithm. -> we travel from start till i -> prefix_sum = b[1] + b[2] + ………… + b[i]

       -> count = count + frequency(prefix_sum);
       frequency(prefix_sum)++;

       -> Changes to be done in this algorithm for it :-> If you are at odd index; and your prefix_sum==g
       -> then you need to search in past which even indices had their prefix_sum as g and vice versa.
       -> only they will contribute to 0-sum subarrays of odd-length;

   */
   vector<int> temp(n);

   for (int i = 0; i < n; i++)
   {
       if (arr[i] < target)
       {
           temp[i] = -1;
       }
       else if (arr[i] > target)
       {
           temp[i] = 1;
       }
       else
       {
           temp[i] = 0;
       }
   }

   unordered_map<int, int> oddIndexPrefixSum;
   unordered_map<int, int> evenIndexPrefixSum;

   evenIndexPrefixSum[0] = 1;

   int ans = 0;
   int prefixSum = 0;
   for (int i = 0; i < n; i++)
   {
       prefixSum += temp[i];
       if (i % 2 == 0)
       {
           if (oddIndexPrefixSum.find(prefixSum) != oddIndexPrefixSum.end())
           {
               ans += oddIndexPrefixSum[prefixSum];
           }
           evenIndexPrefixSum[prefixSum]++;
       }
       else
       {
           if (evenIndexPrefixSum.find(prefixSum) != evenIndexPrefixSum.end())
           {
               ans += evenIndexPrefixSum[prefixSum];
           }
           oddIndexPrefixSum[prefixSum]++;
       }
   }
   return ans;
}

int main()
{
   vector<int> arr = {5, 3, 1, 4, 7, 7};
   int k = 4;
   cout << solve(arr, k) << endl;
   return 0;
}
```


# Greedy Algorithm Important Article: 
Here is a very important greedy algorithm article [link](https://www.hackerearth.com/practice/algorithms/greedy/basics-of-greedy-algorithms/tutorial/)


### 6) #Google OA 

Given binary string = "011100001" we can flip 1 to 0 and 0 to 1 any number of times.

Apply the above operation such that the string does not contain "10" as a substring.

Return minimum number of such operations required.

Solution description: 

we require the string to be of the format: `"00000...111111"` So that no "10" is present

Here, we are using the prefix suffix concept of  [[Hashing]]  

- `pref[i]` = number of `1`s from index `0` to `i`. These must be flipped to `0`.
    
- `suff[i]` = number of `0`s from index `n-1` to `0`. These must be flipped to `1`.


We initialise the ans as `min(pref[n-1], suff[0])`.  It is the minimum cost to convert the entire string to 0 or 1.

`pref[n-1]`: Number of 1s that should be converted to 0 till the n-1th index to get the entire string as "111111111".
`suff[0]`: Number of 0s that should be converted to 1s till the 0th index starting from the end to get the entire string as "00000000".

```cpp

#include <bits/stdc++.h>

using namespace std;

int solve(string s)
{

	int n = s.size();
	
	vector<int> pref(n, 0), suff(n, 0);

	if (s[0] == '1')
	{
	
		pref[0] = 1;
	
	}
  

	for (int i = 1; i < n; i++)
	{
		pref[i] = pref[i - 1] + (s[i] == '1' ? 1 : 0);
	}
	
	if (s[n - 1] == '0')
	{
	
		suff[n - 1] = 1;
	
	}

for (int i = n - 2; i >= 0; i--)

	{
	
		suff[i] = suff[i + 1] + (s[i] == '0' ? 1 : 0);
	
	}

	int ans = min(pref[n - 1], suff[0]);
	
	for (int i = 0; i + 1 < n; i++)
	
	{
	
		ans = min(ans, pref[i] + suff[i + 1]);
	
	}
	
	return ans;

}

int main()
{
	string s = "111000101000";
	
	cout << solve(s) << endl;
	
	return 0;

}

```


### 7) #Intuit OA 

A salesperson must sell `n` items in a bag with random IDs. The salesperson can remove as many as `m` items from the bag. Determine the minimum number of different IDs the final bag can contain after performing, at most, `m` deletions.

### **Example**

```
n = 6
ids = [1, 1, 1, 2, 3, 2]
m = 2
```

Two possible actions that give the minimum 2 different IDs:

1. Remove 2 items with ID = 2, and the final bag will contain item `ids' = [1, 1, 1, 3]`.
    
2. Remove 1 item with ID = 2 and 1 item with ID = 3, and the final bag will contain item `ids' = [1, 1, 1, 2]`.
    

The minimum number of distinct IDs is **2**.

``` CPP
#include <bits/stdc++.h>

using namespace std;

int solve(vector<int> &arr, int m)
{
	int n = arr.size();
	  
	sort(arr.begin(), arr.end());
	
	int cnt = 1;
	
	vector<int> freq;
	
	for (int i = 0; i + 1 < n; i++)
	{
		if (arr[i] == arr[i + 1])
		{
			cnt++;
		}
		else
		{
			freq.push_back(cnt);
			cnt = 1;
		}	
	}
	sort(freq.begin(), freq.end());
	
	int i = 0;
	
	while (m - freq[i] > 0 && i < n)
	{
		m = m - freq[i];
		i++;
	}
	
	return freq.size() - i;
}

int main()
{

	vector<int> arr = {1, 1, 1, 2, 3, 2};
	int m = 2;
	cout << solve(arr, m) << endl;
	return 0;
}

```


### 8)  #Intuit OA Harry Potter 

## **Harry Potter and the Goblet of Fire**

A wizard tournament is being held at Hogwarts, and N wizards are auditioning for it. The Goblet of Fire will choose the wizards according to their primary and secondary spell skills. A wizard will advance to the tournament if there is no other wizard with strictly both higher primary and secondary skills at the same time than this one.

Your task is to calculate the number of wizards that will advance in the tournament.

**Input format:** The first line contains n-> number of wizards followed by 2 arrays of size n containing the skill of spell 1 and spell 2.

## **Example #1**

**Input**

```
2
1 2
2 3
```

**Output**

```
1
```


## **Example #2**

**Input**

```
3
1 2 3
3 2 1
```

**Output**

```
3
```

**Explanation**: All 3 wizards will advance in the tournament since there isn't a single wizard who's primary and secondary skills are strictly greater than the other 


```cpp
#include <bits/stdc++.h>

using namespace std;

int solve(vector<int> a, vector<int> b)
{
    int n = a.size();

    vector<pair<int, int>> p;

    for (int i = 0; i < n; i++)
    {
        p.push_back({a[i], b[i]});
    }

    // sorting on the basis of a in descending order
    sort(p.begin(), p.end(), greater<pair<int, int>>());

    // now, a is sorted in descending order
    // we need to traverse p

    int ans = 0;
    int i = 0;
    int maxB = 0;
    while (i < n)
    {
        int j = i;

        // moving the j pointer till the first spells are not same
        while (j < n && p[j].first == p[i].first)
        {
            j++;
        }

        // now, j points to a[j] where a[j] < a[i] since we sorted in decreasing order

        // calculating the ans based on spell b from i to j-1
        // here from i to j-1, spell a is equal so the only wizard to qualify should have spell b larger than
        for (int k = i; k < j; k++)
        {
            if (maxB <= p[k].second)
            {
                ans++;
            }
        }

        // updating the maxB from i to j-1

        for (int k = i; k < j; k++)
        {
            if (maxB < p[k].second)
            {
                maxB = p[k].second;
            }
        }

        // updating the ith pointer
        i = j;
    }
    return ans;
}
int main()
{
    vector<int> a{1600, 4, 4, 4, 1, 2, 3, 0};
    vector<int> b{0, 5, 0, 0, 3, 2, 5, 1600};
    cout << solve(a, b) << endl;
    return 0;
}
```


### 9) Leetcode Mice 

There are two mice and `n` different types of cheese, each type of cheese should be eaten by exactly one mouse.

A point of the cheese with index `i` (**0-indexed**) is:

- `reward1[i]` if the first mouse eats it.
- `reward2[i]` if the second mouse eats it.

You are given a positive integer array `reward1`, a positive integer array `reward2`, and a non-negative integer `k`.

Return _**the maximum** points the mice can achieve if the first mouse eats exactly_ `k` _types of cheese._

**Example 1:**

**Input:** reward1 = [1,1,3,4], reward2 = [4,4,1,1], k = 2
**Output:** 15
**Explanation:** In this example, the first mouse eats the 2nd (0-indexed) and the 3rd types of cheese, and the second mouse eats the 0th and the 1st types of cheese.
The total points are 4 + 4 + 3 + 4 = 15.
It can be proven that 15 is the maximum total points that the mice can achieve.

**Example 2:**

**Input:** reward1 = [1,1], reward2 = [1,1], k = 2
**Output:** 2
**Explanation:** In this example, the first mouse eats the 0th (0-indexed) and 1st types of cheese, and the second mouse does not eat any cheese.
The total points are 1 + 1 = 2.
It can be proven that 2 is the maximum total points that the mice can achieve.



**Solution Description:** Here, we will create a vector<pair<int,int>>diff to store the difference between reward1 and reward2 along with their index i;
We then sort it in descending order which will give us the largest reward1-reward2 value at the first index. Since the pair also contains the index, we will get the first k indices of largest reward1 value!!!!!
now, from 0 to k-1, we iterate diff to calculate the reward1 values and from k to n-1 to calculate the reward2 values

```CPP
#include <bits/stdc++.h>

using namespace std;

int solve(vector<int> &a, vector<int> &b)
{
    int n = a.size();
    int m = b.size();
    unordered_map<int, int> mp;
    int cnt = 1;
    for (int i = 0; i < n; i++)
    {
        mp[a[i]] = cnt;
        cnt++;
    }
    vector<int> bb;
    for (int i = 0; i < m; i++)
    {
        if (mp.find(b[i]) != mp.end())
        {
            bb.push_back(mp[b[i]]);
        }
    }

    // now we find the LIS of B using binary search method of Striver
    vector<int> temp;
    temp.push_back(bb[0]);
    for (int i = 1; i < bb.size(); i++)
    {
        if (bb[i] > temp.back())
        {
            temp.push_back(bb[i]);
        }
        else
        {
            int index = lower_bound(temp.begin(), temp.end(), bb[i]) - temp.begin();
            temp[index] = bb[i];
        }
    }
    return a.size() - temp.size();
}

int main()
{
    vector<int> a = {1, 2, 3, 4, 5};
    vector<int> b = {2, 5, 6, 4, 9, 12};

    cout << solve(a, b) << endl;
    return 0;
}
```


### 10) 