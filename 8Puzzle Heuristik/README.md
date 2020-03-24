
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

        //Berfungsi untuk mengalokasikan node baru
        Node* newNode(int mat[M][M], int a, int b, int newA, 
            int newB, int level, Node* parent) 
        { 
            Node* node = new Node; 

        // atur pointer dari path ke root 
            node->parent = parent; 

        // copy data dari parent node ke current node 
            memcpy(node->mat, mat, sizeof node->mat); 

        //Pindahkan kotak dengan 1 posisi 
            swap(node->mat[a][b], node->mat[newA][newB]); 

        //Atur jumlah kotak yang salah tempat 
             node->cost = INT_MAX; 

        //Atur jumlah gerakan sejauh ini
            node->level = level; 

        //Perbarui koordinat kotak kosong baru
            node->a = newA; 
            node->b = newB; 
        return node; 
        } 

        //bawah, kiri, atas, kanan
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

        //objek perbandingan yang akan digunakan untuk menyusun stack
            struct comp 
        { 
            bool operator()(const Node* lhs, const Node* rhs) const{
            return (lhs->cost + lhs->level) > (rhs->cost + rhs->level); 
            } 
        }; 

        /* Berfungsi untuk memecahkan algoritma puzzle M * M - 1 menggunakan
           Branch dan Bound. a dan b adalah koordinat kotak kosong
           dalam kondisi awal */
          void solve(int initial[M][M], int a, int b, 
          int final[M][M]) 
          { 
        // Buat prioritas queue untuk menyimpan live nodes dari search tree  
            priority_queue<Node*, std::vector<Node*>, comp> pq; 

        // buat root node and menghitung biayanya
            Node* root = newNode(initial, a, b, a, b, 0, NULL); 
            root->cost = calculateCost(initial, final); 

        // Tambahkan root ke daftar live node 
            pq.push(root); 

        /* Menemukan live node dengan biaya paling sedikit,
          tambahkan childerns ke daftar live node dan
          menghapusnya dari daftar * /
        while (!pq.empty()) 
        { 
        // Temukan live node dengan perkiraan biaya terendah
            Node* min = pq.top(); 

        // Node yang ditemukan dihapus dari daftar node hidup
        pq.pop(); 

        // jika min adalah answer node 
        if (min->cost == 0) 
        { 
        // cetak path dari root ke destination; 
            printPath(min); 
            return; 
        } 

        /* lakukan untuk setiap child min
          maks 4 child untuk node */ 
        for (int i = 0; i < 4; i++) 
        { 
            if (isSafe(min->a + row[i], min->b + col[i])) 
        { 
        // buat child node dan menghitung harganya 
            Node* child = newNode(min->mat, min->a, 
            min->b, min->a + row[i], 
            min->b + col[i], 
            min->level + 1, min); 
            child->cost = calculateCost(child->mat, final); 

        //Tambahkan child ke daftar dari live node
            pq.push(child); 
            } 
          } 
         } 
        } 
 
        int main() 
        { 
        // Konfigurasi awal 
        // Nilai 0 digunakan untuk ruang kosong 
            int initial[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 6, 0}, 
            {7, 8, 4} 
        }; 

        //Konfigurasi final yang dapat dipecahkan
        //Nilai 0 digunakan untuk ruang kosong
        int final[M][M] = 
        { 
            {1, 2, 3}, 
            {5, 8, 6}, 
            {0, 7, 4} 
        }; 

        // Koordinat petak kosong dalam konfigurasi awal
           int a = 1, b = 2; 
            solve(initial, a, b, final); 
                return 0; 
        }
