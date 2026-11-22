Graph Kumar k

### Paypal OA

Solution description:  Here, we do Topological sort where the node with the lowest indegree is printed first and so on . . .

We use mp to store the preference of music x over music y and later on use this to create our adjacency list

After that, we do topo sort using BFS and print the nodes in the topologically sorted fashion.

```cpp
/*
AS a part of enhancing a music streaming platforms user experience, implement a feature that ranks songs
by their popularity. Given m users and n songs, each user has a preference list pref[i] which is a permutation
of numbers 0-n-1 indicating the user's preference for a song. If a < b, the user prefers song pref[i][a] over
song pref[i][b]
To rank songs, use the following approach. Song x is said to be beat song y is x is if x is preferred over y
by more than half of the users or if x is preferred same as y but x has a smaller id i.e if song x and song y
has similar preference than x is preferred more because it has a smaller id.
*/

#include <bits/stdc++.h>
using namespace std;

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   vector<vector<int>> pref(n, vector<int>(m));
   for (int i = 0; i < n; i++)
   {
       for (int j = 0; j < m; j++)
       {
           cin >> pref[i][j];
       }
   }

   // creating a 2d array to map the pairs
   vector<vector<int>> mp(m, vector<int>(m));
   for (int i = 0; i < n; i++)
   {
       for (int i1 = 0; i1 < m; i1++)
       {
           for (int i2 = i1 + 1; i2 < m; i2++)
           {
               int x = pref[i][i1];
               int y = pref[i][i2];
               mp[x][y] = mp[x][y] + 1;
           }
       }
   }

   // indegree vector
   vector<int> indegree(m, 0);

   // creating an adjacency list with the help of mp
   vector<int> adj[m];

   // filling up the adjacency list
   for (int i = 0; i < m; i++)
   {
       for (int j = i + 1; j < m; j++)
       {
           if (mp[i][j] > n / 2)
           {
               adj[i].push_back(j);
               indegree[j]++;
           }
           else if (n % 2 == 0)
           {
               if (mp[i][j] == n / 2)
               {
                   adj[i].push_back(j);
                   indegree[j]++;
               }
               else
               {
                   adj[j].push_back(i);
                   indegree[i]++;
               }
           }
           else
           {
               adj[j].push_back(i);
               indegree[i]++;
           }
       }
   }

   // we can do a topo sort here using BFS
   queue<int> q;

   // inserting in queue nodes with indegree 0;
   for (int i = 0; i < m; i++)
   {
       if (indegree[i] == 0)
           q.push(i);
   }

   // BFS
   vector<int> ans;
   while (!q.empty())
   {
       int node = q.front();
       ans.push_back(node);
       q.pop();

       for (auto it : adj[node])
       {
           indegree[it]--;
           if (indegree[it] == 0)
               q.push(it);
       }
   }

   cout << endl
        << endl;
   for (auto it : ans)
   {
       cout << it << " ";
   }
   cout << endl;

   return 0;
}
```

### Microsoft OA destroy Nodes

```cpp
/*
Understanding - You are given a graph; whichever node has 1 or 0 edge; connected to it you have to destroy
that node; keep doing this each second. Finally tell how many seconds it takes to do this process?

*/
#include <bits/stdc++.h>
using namespace std;

int main()
{
   int n;
   cin >> n;
   int m;
   cin >> m;
   vector<int> a(n);
   vector<int> b(n);

   for (int i = 0; i < m; i++)
   {
       cin >> a[i];
   }
   for (int i = 0; i < m; i++)
   {
       cin >> b[i];
   }

   vector<int> adj[n]; // [n] vectors will be accomodated

   // creating the adjacency list
   for (int i = 0; i < m; i++)
   {
       adj[a[i]].push_back(b[i]);
       adj[b[i]].push_back(a[i]);
   }

   queue<int> q;
   vector<int> d(n, 0);    // to store the no. of edges of a node (degree of a node)
   vector<int> used(n, 0); // to keep a track of the nodes destroyed so far

   // storing all the nodes with edges 0 and 1 in the queue
   for (int i = 0; i < n; i++)
   {
       int v = adj[i].size(); // degree of the ith node
       d[i] = v;              // storing the degree in the degree array
       // pushing it in the queue if it's edges are <= 1
       if (d[i] <= 1)
       {
           q.push(i);
           used[i] = 1;
       }
   }
   // now the queue contains all the nodes with degree 0 or 1
   int cnt = 0;
   int trigger = 0; // to keep a track of the size  of the queue. If queue is empty, trigger is 1 and then the traversal is ended and the ans is returned.

   if (q.size() == 0)
   {
       trigger = 1;
   }

   // traversing the queue

   while (trigger == 0)
   {
       queue<int> q2;

       // Doing BFS on q till it is empty
       while (!q.empty())
       {
           int node = q.front();
           q.pop();

           // removing all the edges (0 or 1 edge) connected to the node (destroying the node)
           for (auto it : adj[node])
           {
               d[it] = d[it] - 1; // removing one edge from the neighbour (destroying the current node)

               if (d[it] <= 1 && used[it] == 0) // now, if the neighbour also has 0 or 1 edge, we need to destroy it
               {
                   q2.push(it);
                   used[it] = 1;
               }
           }
       }

       q = q2;

       int len = q.size();
       if (len == 0)
       {
           trigger = 1;
       }

       cnt++;
   }
   cout << cnt << endl;
   return 0;
}
```

### [Flood Fill](https://leetcode.com/problems/flood-fill/)

Here, we use BFS to start from the source and find all the neighbour cells with the same colour as the source before colouring it and then we colour the cells and then do dfs on the cells.. We keep repeating this process till the queue is empty
We store the indices of the cells in the queue in pairs

```cpp
queue<pair<int,int>>q;

bool check (int n, int m, int ni, int nj)
   {
       if (ni < n && ni >=0 && nj <m && nj >= 0)
           return true;
       else
           return false;
   }

   vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
       int n = image.size();
       int m = image[0].size();

       int current = image[sr][sc];

       queue<pair<int,int>>q;
       vector<vector<int>>visited(n, vector<int>(m, 0));

       q.push({sr, sc});
       visited[sr][sc] = 1;

       while (!q.empty())
       {
           int i = q.front().first; // sr
           int j = q.front().second; // sc
           q.pop();
           image[i][j] = color;

           vector<int>drow = {-1, 0, 1, 0};
           vector<int>dcol = {0, -1, 0, 1};

           for (int ii = 0; ii<4; ii++)
           {
               int ni = i + drow[ii];
               int nj = j + dcol[ii];

               if (check(n, m, ni, nj) && image[ni][nj] == current && !visited[ni][nj])
               {
                   q.push({ni, nj});
                   visited[ni][nj] = 1;
               }
           }
       }
       return image;

   }
```

### [Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

(The approach is similar to the previous problem )

->

```cpp
/*
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water),
return the number of islands.

An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically.
You may assume all four edges of the grid are all surrounded by water.
*/
#include <bits/stdc++.h>
using namespace std;

bool isSafe(int ni, int nj, int n, int m)
{
   if (ni >= 0 && ni < n && nj >= 0 && nj < m)
       return true;
   else
       return false;
}
void bfs(int node, int i, int j, vector<vector<char>> &grid, int n, int m, vector<vector<int>> &visited)
{
   queue<pair<int, int>> q;
   q.push({i, j});
   visited[i][j] = 1;

   vector<int> drow = {-1, 0, 1, 0};
   vector<int> dcol = {0, -1, 0, 1};
   while (!q.empty())
   {
       int ii = q.front().first;
       int jj = q.front().second;
       q.pop();
       // visiting the neighbours
       for (int k = 0; k < 4; k++)
       {
           int ni = ii + drow[k];
           int nj = jj + dcol[k];

           if (isSafe(ni, nj, n, m) && grid[ni][nj] == '1' && !visited[ni][nj])
           {
               q.push({ni, nj});
               visited[ni][nj] = 1;
           }
       }
   }
}
int numIslands(vector<vector<char>> &grid)
{
   int n = grid.size();
   int m = grid[0].size();

   vector<vector<int>> visited(n, vector<int>(m, 0));

   int cnt = 0;

   for (int i = 0; i < n; i++)
   {
       for (int j = 0; j < m; j++)
       {
           if (grid[i][j] == '1' && !visited[i][j])
           {
               cnt++;
               bfs(grid[i][j], i, j, grid, n, m, visited);
           }
       }
   }

   return cnt;
}

int main()
{
   vector<vector<char>> grid = {{'1', '1', '1', '1', '0'},
                                {'1', '1', '0', '1', '0'},
                                {'1', '1', '0', '0', '0'},
                                {'0', '0', '0', '0', '0'}};

   cout << numIslands(grid) << endl;

   return 0;
}
```

### Distance of the nearest cell

```cpp
/*
Given a binary grid of n*m. Find the distance of the nearest 1 in the grid for each cell.
The distance is calculated as |i1  - i2| + |j1 - j2|, where i1, j1 are the row number and column number
of the current cell, and i2, j2 are the row number and column number of the nearest cell having value 1.
There should be atleast one 1 in the grid.

Input: grid = {{1,0,1},{1,1,0},{1,0,0}}
Output: {{0,1,0},{0,0,1},{0,1,2}}
Explanation: The grid is-
1 0 1
1 1 0
1 0 0
0's at (0,1), (1,2), (2,1) and (2,2) are at a
distance of 1, 1, 1 and 2 from 1's at (0,0),
(0,2), (2,0) and (1,1) respectively.

Ans:
0 1 0
0 0 1
0 1 2
*/

#include <bits/stdc++.h>
using namespace std;

bool isSafe(int ni, int nj, int n, int m)
{
   if (ni >= 0 && ni < n && nj >= 0 && nj < m)
       return true;
   else
       return false;
}
// Function to find distance of nearest 1 in the grid for each cell.
vector<vector<int>> nearest(vector<vector<int>> grid)
{
   // Code here
   int n = grid.size();
   int m = grid[0].size();
   vector<vector<int>> ans(n, vector<int>(m, INT_MAX));

   vector<vector<int>> visited(n, vector<int>(m, 0));

   // first we need to insert all the 1's indices in the queue
   queue<pair<int, int>> q;
   for (int i = 0; i < n; i++)
   {
       for (int j = 0; j < m; j++)
       {
           if (grid[i][j] == 1)
           {
               q.push({i, j});
               visited[i][j] = 1;
               ans[i][j] = 0;
           }
       }
   }

   // BFS
   vector<int> drow = {-1, 0, 1, 0};
   vector<int> dcol = {0, -1, 0, 1};

   while (!q.empty())
   {
       int ii = q.front().first;
       int jj = q.front().second;
       q.pop();

       for (int i = 0; i < 4; i++)
       {
           int ni = ii + drow[i];
           int nj = jj + dcol[i];

           if (isSafe(ni, nj, n, m) && ans[ni][nj] > ans[ii][jj] + 1 && !visited[ni][nj])
           {
               ans[ni][nj] = ans[ii][jj] + 1;
               q.push({ni, nj});
               visited[ni][nj] = 1;
           }
       }
   }

   return ans;
}

int main()
{
   vector<vector<int>> grid = {{1, 0, 1},
                               {1, 1, 0},
                               {1, 0, 0}};

   vector<vector<int>> ans = nearest(grid);

   for (int i = 0; i < ans.size(); i++)
   {
       for (int j = 0; j < ans[0].size(); j++)
       {
           cout << ans[i][j] << " ";
       }
       cout << endl;
   }
   return 0;
}
```
