Trees are same like graphs except
General rule of a tree : the number of nodes = n; then the number of edges = n-1

i.e the number of nodes is always 1 more than the number of edges
There can be only one parent of a child node
To travel from ‘x’ node to ‘y’ node, there is only one possible path in a tree and hence it is the shortest / longest path.

### 1) Google Question :

Google Interview Problem.-> Given a Tree of ‘N’ Nodes and ‘N-1’ Edges; rooted at Node-1 ; each node is assigned either 1 or 0 ; for each node “i” ; find the number of 1’s on the shortest path from node 1 to node “i”

-> In a tree, there is only one path to travel from one node to another!

-> Solution :

```cpp
/*
Google Interview Problem.-> Given a Tree of ‘N’ Nodes and ‘N-1’ Edges; rooted at Node-1 ;
each node is assigned either 1 or 0 ; for each node “i” ; find the number of 1’s on the shortest path from
node 1 to node “i”
*/

#include <bits/stdc++.h>
using namespace std;

vector<int> solve(vector<int> adj[], vector<int> &b, int n)
{
   queue<int> q;

   vector<int> ans(n + 1, 0); // ans array
   ans[1] = b[1];
   vector<int> used(n + 1, 0); // used array to prevent using one node more than 1 time
   // inserting the first node and using it
   used[1] = 1;
   q.push(1);

   while (!q.empty())
   {
       int node = q.front();
       q.pop();
       for (auto child : adj[node])
       {
           if (used[child] == 0)
           {
               q.push(child);
               used[child] = 1;
               if (b[child] == 1)
               {
                   ans[child] = ans[node] + 1;
               }
               else
               {
                   ans[child] = ans[node];
               }
           }
       }
   }

   return ans;
}

int main()
{
   int n;
   cin >> n;
   vector<int> b(n + 1, 0); // values of the node
   vector<int> adj[n + 1];  // adjacency list of the node

   // inserting the adjacency list
   for (int i = 1; i <= n; i++)
   {
       int u, v;
       cin >> u >> v;
       adj[u].push_back(v);
       adj[v].push_back(u);
   }

   // inserting the values of the nodes
   for (int i = 1; i <= n; i++)
   {
       cin >> b[i];
   }

   vector<int> ans = solve(adj, b, n);
   for (auto it : ans)
   {
       cout << it << " ";
   }
   cout << endl;

   return 0;
}
```

### 2) DFS

DFS Used Recursion

```cpp
// Write the DFS Traversal on trees
#include <bits/stdc++.h>
using namespace std;

void dfs(int currentNode, vector<int> adj[], vector<int> b, int n, vector<int> &visited, vector<int> &parent)
{
   int node = currentNode;
   visited[node] = 1;
   cout << node << " ";
   for (auto child : adj[node])
   {
       if (!visited[child])
       {
           visited[child] = 1;
           // do other operations if present in question
           parent[child] = node;
           // Call DFS on the child
           dfs(child, adj, b, n, visited, parent);
       }
   }
}
```

### 3) Reverse DFS

Reverse DFS : It is used to print the leaf nodes first followed by the parent nodes and finally the root node. It is used to calculate DP on trees where the final ans is always stored in dp[root], We first need to calculate the leaf node ans followed by the parent nodes and finally the root node

To calculate like this, we need a special traversal of DFS that happens from the bottom
Here, everything remains the same except we should print the nodes after the for loop ends and not before it starts
In this way, we print the leaf nodes and all the child nodes after that we print the root node in the end

```cpp
#include <bits/stdc++.h>
using namespace std;
void reverseDFS(int currentNode, vector<int> adj[], vector<int> b, int n, vector<int> &visited, vector<int> &parent)
{
   int node = currentNode;
   visited[node] = 1;
   // cout << node << " "; // this is the normal DFS
   for (auto child : adj[node])
   {
       if (!visited[child])
       {
           visited[child] = 1;
           // do other operations if present in question
           parent[child] = node;
           // Call DFS on the child
           reverseDFS(child, adj, b, n, visited, parent);
       }
   }

   cout << node << " "; // here it is printed from the leaf to the root// used for DP on trees
}
```

### 4) Max sum of subtree

Here also we use reverse DFS to find the maximum sum of a subtree or the sum of each subtree which is stored in the prefSum array

```cpp
// DP on trees can be performed to solve a problem
/*
For trees, the final dp ans is stored on the root node !
dp[root] holds the final ans
for this, we need to first calculate the leaf nodes, then all the child nodes and then we come up to the parent node for the final ans

To calculate like this, we need a special traversal of DFS that happens from the bottom
Here, everything remains the same except we should print the nodes after the for loop ends and not before it starts
In this way, we print the leaf nodes and all the child nodes after that we print the root node in the end
*/
#include <bits/stdc++.h>
using namespace std;
void reverseDFS(int currentNode, vector<int> adj[], vector<int> b, int n, vector<int> &visited, vector<int> &parent)
{
   int node = currentNode;
   visited[node] = 1;
   // cout << node << " "; // this is the normal DFS
   for (auto child : adj[node])
   {
       if (!visited[child])
       {
           visited[child] = 1;
           // do other operations if present in question
           parent[child] = node;
           // Call DFS on the child
           reverseDFS(child, adj, b, n, visited, parent);
       }
   }

   cout << node << " "; // here it is printed from the leaf to the root// used for DP on trees
}
```
