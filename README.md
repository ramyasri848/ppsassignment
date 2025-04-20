#include <stdio.h>
#include <stdbool.h>

#define N 8

// All 8 possible moves a Knight can make
int dx[8] = { 2, 1, -1, -2, -2, -1, 1, 2 };
int dy[8] = { 1, 2,  2,  1, -1, -2, -2, -1 };

// Check if the move is valid
bool isValid(int x, int y, int board[N][N]) {
    return (x >= 0 && x < N && y >= 0 && y < N && board[x][y] == 0);
}

// Print the chessboard
void printBoard(int board[N][N]) {
    for (int x = 0; x < N; x++) {
        for (int y = 0; y < N; y++)
            printf("%2d ", board[x][y]);
        printf("\n");
    }
}

// Recursive function to solve Knight's Tour
bool solveKnightTour(int x, int y, int moveCount, int board[N][N]) {
    if (moveCount == N * N + 1)
        return true;

    for (int i = 0; i < 8; i++) {
        int nextX = x + dx[i];
        int nextY = y + dy[i];
        if (isValid(nextX, nextY, board)) {
            board[nextX][nextY] = moveCount;
            if (solveKnightTour(nextX, nextY, moveCount + 1, board))
                return true;
            board[nextX][nextY] = 0; // Backtrack
        }
    }

    return false;
}

// Function to get validated input from user
void getValidInput(int* x, int* y) {
    while (1) {
        printf("Enter starting X coordinate (0-7): ");
        scanf("%d", x);
        printf("Enter starting Y coordinate (0-7): ");
        scanf("%d", y);

        if (*x >= 0 && *x < N && *y >= 0 && *y < N) {
            break; // Valid input
        } else {
            printf("Invalid input. Please enter values between 0 and 7.\n\n");
        }
    }
}

int main() {
    int board[N][N] = {0};
    int startX, startY;

    getValidInput(&startX, &startY);

    board[startX][startY] = 1; // Mark starting position

    if (solveKnightTour(startX, startY, 2, board)) {
        printf("\nKnight's Tour solution:\n");
        printBoard(board);
    } else {
        printf("No solution exists from the given starting position.\n");
    }

    return 0;
}
