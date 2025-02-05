# Sudoku-Solver
#include <iostream>
#include <vector>
using namespace std;
// It is use to Print Sample Sudoku
void printSudoku(int arr[9][9]) {
    cout<<"----------------------" << endl;
    for (int r = 0; r < 9; r++) {
        for (int c = 0; c < 9; c++) {
            cout<<arr[r][c] << " ";
        }
        cout<<endl;
    }
    cout<<"----------------------" << endl;
}
bool canPlace(int arr[9][9], int row, int col, int n) {
// Here we Checks the row
    for (int i = 0; i < 9; i++) {
        if (arr[row][i] == n) {
            return false;
        }
    }
//Here we Checks column
    for (int i = 0; i < 9; i++) {
        if (arr[i][col] == n) {
            return false;
        }
    }
//Here we  Checks that 3x3 subgrid
    int startRow = (row / 3) * 3;
    int startCol = (col / 3) * 3;
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            if (arr[startRow + i][startCol + j] == n) {
                return false;
            }
        }
    }
    return true;
}
vector<int> findPlaceble(int arr[9][9], int r, int c) {
    vector<int> cps = {};
    for (int i = 1; i <= 9; i++) {
        if (canPlace(arr, r, c, i)) {
            cps.push_back(i);
        }
    }
    return cps;
}
bool findNextEmptyCell(int arr[9][9], int &row, int &col) {
    for (row = 0; row < 9; row++) {
        for (col = 0; col < 9; col++) {
            if (arr[row][col] == 0) {
                return true; //If Found an empty cell
            }
        }
    }
    return false; //If No more empty cells found 
}
bool solveSudoku(int arr[9][9]) {
    int row, col;
//Here we Find the next empty cell
    if (!findNextEmptyCell(arr, row, col)) {
        return true; //If No empty cells, puzzle is solved
    }
    vector<int> placeables = findPlaceble(arr, row, col);
    for (int n : placeables) {
        arr[row][col] = n;
        if (solveSudoku(arr)) {
            return true; //If Solution found
        }
        arr[row][col] = 0; // Backtrack
    }
    return false; //If No solution found
}
int main() {
    int board[9][9] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 0},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };
    printSudoku(board);
    if (solveSudoku(board)) {
        cout<<"Sudoku solved!"<<endl;
        printSudoku(board);
    } else {
        cout<<"No solution found."<<endl;
    }
    return 0;
}
