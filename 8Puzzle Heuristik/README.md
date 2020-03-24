
* Heuristic itu gabungan dari kedua h1 dan h2 di mana h1 itu 
h(n) <= h*(n) dimana h*(n) -> cost sebenarnya, dan h2 itu
h(n) >= 0 sehingga h(G) = 0 untuk goal G.
jadi menurut saya tidak ada perbedaan antara heuristic 1 & 2, 
melainkan heuristic itu merupakan metode algoritma informed search

    //Program untuk mencetak path dari root node ke destination node
    //untuk algoritma puzzle M * M -1 menggunakan Branch and Bound
    //Solusinya mengasumsikan bahwa contoh puzzle dapat dipecahkan 
    #include <bits/stdc++.h> 
    using namespace std; 
    #define M 3 

   
    struct Node 
{ 
 // stores the parent node of the current node 
 // helps in tracing path when the answer is found 
 Node* parent; 

 // stores matrix 
 int mat[M][M]; 

 // stores blank tile coordinates 
 int a, b; 

 // stores the number of misplaced tiles 
 int cost; 

 // stores the number of moves so far 
 int level; 
}; 

// Function to print M x M matrix 
int printMatrix(int mat[M][M]) 
{ 
 for (int i = 0; i < M; i++) 
 { 
  for (int j = 0; j < M; j++) 
   printf("%d ", mat[i][j]); 
  printf("\n"); 
 } 
} 

// Function to allocate a new node 
Node* newNode(int mat[M][M], int a, int b, int newA, 
   int newB, int level, Node* parent) 
{ 
 Node* node = new Node; 

 // set pointer for path to root 
 node->parent = parent; 

 // copy data from parent node to current node 
 memcpy(node->mat, mat, sizeof node->mat); 

 // move tile by 1 position 
 swap(node->mat[a][b], node->mat[newA][newB]); 

 // set number of misplaced tiles 
 node->cost = INT_MAX; 

 // set number of moves so far 
 node->level = level; 

 // update new blank tile cordinates 
 node->a = newA; 
 node->b = newB; 

 return node; 
} 

// botton, left, top, right 
int row[] = { 1, 0, -1, 0 }; 
int col[] = { 0, -1, 0, 1 }; 

// Function to calculate the number of misplaced tiles 
// ie. number of non-blank tiles not in their goal position 
int calculateCost(int initial[M][M], int final[M][M]) 
{ 
 int count = 0; 
 for (int i = 0; i < M; i++) 
 for (int j = 0; j < M; j++) 
  if (initial[i][j] && initial[i][j] != final[i][j]) 
  count++; 
 return count; 
} 

// Function to check if (a, b) is a valid matrix cordinate 
int isSafe(int a, int b) 
{ 
 return (a >= 0 && a < M && b >= 0 && b < M); 
} 

// print path from root node to destination node 
void printPath(Node* root) 
{ 
 if (root == NULL) 
  return; 
 printPath(root->parent); 
 printMatrix(root->mat); 

 printf("\n"); 
} 

// Comparison object to be used to order the heap 
struct comp 
{ 
 bool operator()(const Node* lhs, const Node* rhs) const
 { 
  return (lhs->cost + lhs->level) > (rhs->cost + rhs->level); 
 } 
}; 

// Function to solve M*M - 1 puzzle algorithm using 
// Branch and Bound. a and b are blank tile coordinates 
// in initial state 
void solve(int initial[M][M], int a, int b, 
  int final[M][M]) 
{ 
 // Create a priority queue to store live nodes of 
 // search tree; 
 priority_queue<Node*, std::vector<Node*>, comp> pq; 

 // create a root node and calculate its cost 
 Node* root = newNode(initial, a, b, a, b, 0, NULL); 
 root->cost = calculateCost(initial, final); 

 // Add root to list of live nodes; 
 pq.push(root); 

 // Finds a live node with least cost, 
 // add its childrens to list of live nodes and 
 // finally deletes it from the list. 
 while (!pq.empty()) 
 { 
  // Find a live node with least estimated cost 
  Node* min = pq.top(); 

  // The found node is deleted from the list of 
  // live nodes 
  pq.pop(); 

  // if min is an answer node 
  if (min->cost == 0) 
  { 
   // print the path from root to destination; 
   printPath(min); 
   return; 
  } 

  // do for each child of min 
  // max 4 children for a node 
  for (int i = 0; i < 4; i++) 
  { 
   if (isSafe(min->a + row[i], min->b + col[i])) 
   { 
    // create a child node and calculate 
    // its cost 
    Node* child = newNode(min->mat, min->a, 
       min->b, min->a + row[i], 
       min->b + col[i], 
       min->level + 1, min); 
    child->cost = calculateCost(child->mat, final); 

    // Add child to list of live nodes 
    pq.push(child); 
   } 
  } 
 } 
} 

// Driver code 
int main() 
{ 
 // Initial configuration 
 // Value 0 is used for empty space 
 int initial[M][M] = 
 { 
  {1, 2, 3}, 
  {5, 6, 0}, 
  {7, 8, 4} 
 }; 

 // Solvable Final configuration 
 // Value 0 is used for empty space 
 int final[M][M] = 
 { 
  {1, 2, 3}, 
  {5, 8, 6}, 
  {0, 7, 4} 
 }; 

 // Blank tile coordinates in initial 
 // configuration 
 int a = 1, b = 2; 

 solve(initial, a, b, final); 

 return 0; 
}
