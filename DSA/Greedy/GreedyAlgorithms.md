The **Greedy Exchange Argument Lemma** is a mathematical technique used to prove that a greedy algorithm produces an optimal solution.

The central idea is:

> If an optimal solution does not make the greedy choice, we can modify it to include the greedy choice without making the solution worse.

In other words, we can exchange a decision made by an optimal solution with the greedy decision while preserving optimality.

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

### IBM OA

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
### Goldman Sachs OA

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

### Atlassian OA

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

### Atlassian OA String value sum

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

### Salesforce OA

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
