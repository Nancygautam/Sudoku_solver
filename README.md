# Sudoku_solver

#include <iostream>
#define UNASSIGNED 0
#define N 9

class SudokuSolver {
public:
    bool solveSudoku(int grid[N][N]) {
        int row, col;

        // If there is no unassigned location, we are done
        if (!findUnassignedLocation(grid, row, col))
            return true;

        // Consider digits 1 to 9
        for (int num = 1; num <= 9; num++) {
            // If looks promising
            if (isSafe(grid, row, col, num)) {
                // Make tentative assignment
                grid[row][col] = num;

                // Return, if success
                if (solveSudoku(grid))
                    return true;

                // Failure, unmake & try again
                grid[row][col] = UNASSIGNED;
            }
        }
        return false; // This triggers backtracking
    }

    void printGrid(int grid[N][N]) {
        for (int row = 0; row < N; row++) {
            for (int col = 0; col < N; col++)
                std::cout << grid[row][col] << " ";
            std::cout << std::endl;
        }
    }

private:
    bool findUnassignedLocation(int grid[N][N], int &row, int &col) {
        for (row = 0; row < N; row++)
            for (col = 0; col < N; col++)
                if (grid[row][col] == UNASSIGNED)
                    return true;
        return false;
    }

    bool isUsedInRow(int grid[N][N], int row, int num) {
        for (int col = 0; col < N; col++)
            if (grid[row][col] == num)
                return true;
        return false;
    }

    bool isUsedInCol(int grid[N][N], int col, int num) {
        for (int row = 0; row < N; row++)
            if (grid[row][col] == num)
                return true;
        return false;
    }

    bool isUsedInBox(int grid[N][N], int boxStartRow, int boxStartCol, int num) {
        for (int row = 0; row < 3; row++)
            for (int col = 0; col < 3; col++)
                if (grid[row + boxStartRow][col + boxStartCol] == num)
                    return true;
        return false;
    }

    bool isSafe(int grid[N][N], int row, int col, int num) {
        return !isUsedInRow(grid, row, num) &&
               !isUsedInCol(grid, col, num) &&
               !isUsedInBox(grid, row - row % 3, col - col % 3, num) &&
               grid[row][col] == UNASSIGNED;
    }
};

int main() {
    int grid[N][N] = {
        {5, 3, 0, 0, 7, 0, 0, 0, 0},
        {6, 0, 0, 1, 9, 5, 0, 0, 0},
        {0, 9, 8, 0, 0, 0, 0, 6, 0},
        {8, 0, 0, 0, 6, 0, 0, 0, 3},
        {4, 0, 0, 8, 0, 3, 0, 0, 1},
        {7, 0, 0, 0, 2, 0, 0, 0, 6},
        {0, 6, 0, 0, 0, 0, 2, 8, 0},
        {0, 0, 0, 4, 1, 9, 0, 0, 5},
        {0, 0, 0, 0, 8, 0, 0, 7, 9}
    };

    SudokuSolver solver;
    if (solver.solveSudoku(grid))
        solver.printGrid(grid);
    else
        std::cout << "No solution exists" << std::endl;

    return 0;
}
