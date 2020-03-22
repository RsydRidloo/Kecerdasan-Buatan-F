 
    https://github.com/TropEX/Kecerdasan-Buatan-F/issues/1#issue-585638079
    
    //Metode ini digunakan untuk mewarnai karakter
    void Color(int col) {  
    HANDLE hConsole = GetStdHandle(STD_OUTPUT_HANDLE);  
    SetConsoleTextAttribute(hConsole, col);  
    }  
    //Metode ini untuk mencetak solusi akhir  
    void printSolution(int board[N][N]) {  
        for (int i = 0; i < N; i++) {  
            for (int j = 0; j < N; j++) {  
                if (board[i][j] == 1) {  
                    Color(2);  
                    printf("%d ", board[i][j]);  
                } else {  
                    Color(15);  
                    printf("%d ", board[i][j]);  
                }  
            }  
            printf("\n");  
        }  
    }  
    // Metode ini memeriksa apakah aman untuk menempatkan ratu di baris dan kolom tertentu.
     // Ini akan return true jika aman untuk menempatkan ratu dan salah jika tidak.
    bool isSafe(int board[N][N], int row, int col) {  
    int i, j;  
    /* Periksa baris di sisi kiri */  
    for (i = 0; i < col; i++)  
        if (board[row][i]) return false;  
    /* Periksa diagonal atas di sisi kiri */  
    for (i = row, j = col; i >= 0 && j >= 0; i--, j--)  
        if (board[i][j]) return false;  
    /* Periksa diagonal bawah di sisi kiri */  
    for (i = row, j = col; j >= 0 && i < N; i++, j--)  
        if (board[i][j]) return false;  
    return true;  
    }  

    //Masalah utilitas rekursif untuk memecahkan masalah N queens.
    bool solveNQUtil(int board[N][N], int col) {  
    /* base case: Jika semua ratu ditempatkan lalu return true */  
    if (col >= N) return true;  
    /* Pertimbangkan kolom  dan coba tempatkan ratu di semua baris satu per satu */  
    for (int i = 0; i < N; i++) {  
        /* Periksa apakah ratu dapat ditempatkan pada board[i][col] */  
        if (isSafe(board, i, col)) {  
            /* Tempatkan ratu pada board[i][col] */  
            board[i][col] = 1;  
            /* berulang untuk menempatkan sisa ratu */  
            if (solveNQUtil(board, col + 1)) return true;  
            /* Jika menempatkan ratu di board[i][col] 
            tidak mengarah ke solusi, lalu hapus ratu dari board[i][col] */  
            board[i][col] = 0; // BACKTRACK  
        }  
    }  
    /* Jika ratu tidak dapat ditempatkan di baris mana pun di kolom col ini, lalu
     return false */  
    return false;  
    }  
    
    /* Fungsi ini memecahkan masalah N Queen menggunakan
    Backtracking.Terutana ini menggunakan solveNQUtil () untuk
    menyelesaikan masalah. Ini return false jika ratu
    tidak dapat ditempatkan, sebaliknya return true dan
    mencetak penempatan ratu dalam bentuk 1s. Harap dicatat bahwa mungkin ada lebih dari satu
    solusi, fungsi ini mencetak salah satu sesi layak
    */  
    bool solveNQ() {  
        int board[N][N] = {  
            {  
                0,  
                0,  
                0,  
                0  
            },  
            {  
                0,  
                0,  
                0,  
                0  
            },  
            {  
                0,  
                0,  
                0,  
                0  
            },  
            {  
                0,  
                0,  
                0,  
                0  
            }  
        };  
        if (solveNQUtil(board, 0) == false) {  
            printf("Solution does not exist");  
            return false;  
        }  
            printSolution(board);  
            return true;  
    }  
        int main() {  
        solveNQ();  
        _getch();  
        return 0;  
    } 
