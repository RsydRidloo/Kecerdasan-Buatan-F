1. BFS
* Breadth-first search adalah algoritma yang melakukan pencarian secara melebar yang mengunjungi simpul secara preorder yaitu 
mengunjungi suatu simpul kemudian mengunjungi semua simpul yang bertetangga dengan simpul tersebut terlebih dahulu. 
Selanjutnya, simpul yang belum dikunjungi dan bertetangga dengan simpulsimpul yang tadi dikunjungi , demikian seterusnya. 
Jika graf berbentuk pohon berakar, maka semua simpul pada aras d dikunjungi lebih dahulu sebelum simpul-simpul pad aras d+1.
Algoritma ini memerlukan sebuah antrian q untuk menyimpan simpul yang telah dikunjungi. 
Simpulsimpul ini diperlukan sebagai acuan untuk mengunjungi simpul-simpul yang bertetanggaan dengannya. 
Tiap simpul yang telah dikunjungu masuk ke dalam antrian hanya satu kali. Algoritma ini juga membutuhkan 
table Boolean untuk menyimpan simpul yang te lah dikunjungi sehingga tidak ada simpul yang dikunjungi lebih dari satu kali.

Gambar compile & run https://github.com/TropEX/Kecerdasan-Buatan-F/issues/5#issue-590128645


        /*Menggunakan branch and bound yang merupakan algoritma
          Breath First Search yang berjalan layaknya penggunaan queue */

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

	bool testing (int mat[M][M],int final[M][M]){
 	for (int i = 0; i < M; i++) 
	{ 
		for (int j = 0; j < M; j++) {
			if (mat [i] [j] != final [i] [j]){
				return false;
				} 
			}
		}
 		return true;
 	}

 	/* Berfungsi untuk memecahkan algoritma puzzle M * M - 1 menggunakan
           Branch dan Bound. a dan b adalah koordinat kotak kosong
           dalam kondisi awal */
          void solve(int initial[M][M], int a, int b, 
          int final[M][M]) 
          { 
        // Buat prioritas queue untuk menyimpan live nodes dari search tree  
            queue <Node*> pq;

        // buat root node and menghitung biayanya
           Node* root = newNode(initial, a, b, a, b, 0, NULL); 

        // Tambahkan root ke daftar live node 
            pq.push(root); 

        /* Menemukan live node dengan biaya paling sedikit,
          tambahkan childerns ke daftar live node dan
          menghapusnya dari daftar */
          
        while (!pq.empty()) 
        { 
        // Temukan live node dengan perkiraan biaya terendah
            Node* min = pq.front(); 

        // Node yang ditemukan dihapus dari daftar node hidup
        pq.pop(); 

        // jika min adalah answer node;
        if (testing (min->mat, final)) 
        { 
        // cetak path dari root ke destination; 
            	printPath(min); 
			printf ("Move: %d",min->level);
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

2.DFS
* Algoritma DFS (Depth First Search) adalah salah satu algoritma yang digunakan untuk pencarian jalur. Contoh yang dibahas kali
ini adalah mengenai pencarian jalur yang melalui semua titik.
Algoritma ini mirip dengan Algoritma BFS (Breadth First Search) yang sudah dijelaskan sebelumnya. Jika Algoritma BFS (Breadth
First Search) melakukan perhitungan secara terurut dari urutan pertama sampai urutan terakhir, maka algoritma ini melakukan
kebalikannya, yaitu melakukan perhitungan secara terurut dari urutan terakhir. Setelah menghabiskan semua kemungkinan dari titik
terakhir, barulah mundur ke titik-titik sebelumnya sampai pada titik pertama.

		
	/*Menggunakan branch and bound yang merupakan algoritma
          Breath First Search yang berjalan layaknya penggunaan stack*/

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

	bool testing (int mat[M][M],int final[M][M]){
 	for (int i = 0; i < M; i++) 
	{ 
		for (int j = 0; j < M; j++) {
			if (mat [i] [j] != final [i] [j]){
				return false;
				} 
			}
		}
 		return true;
 	}

 	/* Berfungsi untuk memecahkan algoritma puzzle M * M - 1 menggunakan
           Branch dan Bound. a dan b adalah koordinat kotak kosong
           dalam kondisi awal */
          void solve(int initial[M][M], int a, int b, 
          int final[M][M]) 
          { 
        // Buat prioritas stack untuk menyimpan live nodes dari search tree  
            stack<Node*> pq;

        // buat root node and menghitung biayanya
           Node* root = newNode(initial, a, b, a, b, 0, NULL); 

        // Tambahkan root ke daftar live node 
            pq.push(root); 

        /* Menemukan live node dengan biaya paling sedikit,
          tambahkan childerns ke daftar live node dan
          menghapusnya dari daftar */
          
        while (!pq.empty()) 
        { 
        // Temukan live node dengan perkiraan biaya terendah
            Node* min = pq.front(); 

        // Node yang ditemukan dihapus dari daftar node hidup
        pq.pop(); 

        // jika min adalah answer node;
        if (testing (min->mat, final)) 
        { 
        // cetak path dari root ke destination; 
            	printPath(min); 
			printf ("Move: %d",min->level);
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
		
		
	
