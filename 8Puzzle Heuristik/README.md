
* Heuristic itu gabungan dari kedua h1 dan h2 di mana h1 itu 
h(n) <= h*(n) dimana h*(n) -> cost sebenarnya, dan h2 itu
h(n) >= 0 sehingga h(G) = 0 untuk goal G.
jadi menurut saya tidak ada perbedaan antara heuristic 1 & 2, 
melainkan heuristic itu merupakan metode algoritma informed search

* Gambar compile & run dapat dilihat disini https://github.com/TropEX/Kecerdasan-Buatan-F/issues/2#issue-586691947

    //Program untuk mencetak path dari root node ke destination node
    //untuk algoritma puzzle M * M -1 menggunakan Branch and Bound
    //Solusinya mengasumsikan bahwa contoh puzzle dapat dipecahkan 
    #include <bits/stdc++.h> 
    using namespace std; 
    #define M 3 

   
    struct Node 
    { 
    // menyimpan parent node dari current node
    // membantu dalam melacak jejak ketika jawabannya ditemukan
    Node* parent; 

    // menyimpan matrix 
    int mat[M][M]; 

    // menyimpan kotak koordinat kosong 
    int a, b; 

    // menyimpan jumlah kotak yang salah tempat
    int cost; 

     // menyimpan jumlah gerakan sejauh ini
    int level; 
    }; 

    // Fungsi untuk mencetak M x M matrix 
    int printMatrix(int mat[M][M]) 
    { 
    for (int i = 0; i < M; i++){ 
    for (int j = 0; j < M; j++) 
        printf("%d ", mat[i][j]); 
            printf("\n"); 
        } 
    }       

    //  Berfungsi untuk mengalokasikan node baru
    Node* newNode(int mat[M][M], int a, int b, int newA, 
        int newB, int level, Node* parent) 
    { 
        Node* node = new Node; 

    // atur pointer dari path ke root 
        node->parent = parent; 

    // copy data dari parent node ke current node 
        memcpy(node->mat, mat, sizeof node->mat); 

    // pindahkan kotak dengan 1 posisi 
        swap(node->mat[a][b], node->mat[newA][newB]); 

    // atur jumlah kotak yang salah tempat 
        node->cost = INT_MAX; 

    // atur jumlah gerakan sejauh ini
        node->level = level; 

    // perbarui koordinat kotak kosong baru
        node->a = newA; 
        node->b = newB; 
    return node; 
    } 

    // bawah, kiri, atas, kanan
        int row[] = { 1, 0, -1, 0 }; 
        int col[] = { 0, -1, 0, 1 }; 

    // Berfungsi untuk menghitung jumlah ubin yang salah tempat
    int calculateCost(int initial[M][M], int final[M][M]) 
    { 
       int count = 0; 
       for (int i = 0; i < M; i++) 
       for (int j = 0; j < M; j++) 
            if (initial[i][j] && initial[i][j] != final[i][j]) 
       count++; 
     return count; 
    } 

    // Fungsi untuk memeriksa apakah (a, b) adalah koordinat matriks yang valid
        int isSafe(int a, int b) 
    { 
         return (a >= 0 && a < M && b >= 0 && b < M); 
    } 

    // mencetak path dari root node ke destination node 
    void printPath(Node* root){ 
    if (root == NULL) 
        return; 
    printPath(root->parent); 
    printMatrix(root->mat);     
    printf("\n"); 
    } 

    // Comparison object to be used to order the heap 
    struct comp 
    { 
        bool operator()(const Node* lhs, const Node* rhs) const{
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
