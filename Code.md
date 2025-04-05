# viralregenerationsimulation
My code for dead and alive viruses.

#include <stdio.h>
#include <stdlib.h>
#include <unistd.h> // For usleep()

int size = 20;
int grid[20][20];
int newGrid[20][20];

// Function to count neighbors
int countNeighbors(int x, int y) {
    int count = 0;
    for (int i = -1; i <= 1; i++) {
        for (int j = -1; j <= 1; j++) {
            if (i == 0 && j == 0) continue; // Skip itself
            int nx = x + i, ny = y + j;
            if (nx >= 0 && nx < size && ny >= 0 && ny < size) {
                count += grid[nx][ny];
            }
        }
    }
    return count;
}

// Function to update the grid
void updateGrid() {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            int neighbors = countNeighbors(i, j);
            if (grid[i][j] == 1) { // Alive
                if (neighbors < 2 || neighbors > 3)
                    newGrid[i][j] = 0; // Dies
                else
                    newGrid[i][j] = 1; // Stays alive
            } else { // Dead
                if (neighbors == 3)
                    newGrid[i][j] = 1; // Revives
                else
                    newGrid[i][j] = 0; // Stays dead
            }
        }
    }

    // Copy newGrid to grid
    for (int i = 0; i < size; i++)
        for (int j = 0; j < size; j++)
            grid[i][j] = newGrid[i][j];
}

// Function to display the grid
void displayGrid() {
    printf("\033[H"); // Moves cursor to top-left (for smoother animation)
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            printf("%c ", grid[i][j] ? 'M' : '.'); // 'X' for alive, '.' for dead
        }
        printf("\n");
    }
}

// Function to initialize the grid randomly
void initializeGrid() {
    for (int i = 0; i < size; i++) {
        for (int j = 0; j < size; j++) {
            grid[i][j] = rand() % 2; // Randomly alive (1) or dead (0)
        }
    }
}

int main() {
    initializeGrid();
    
    while (1) { // Infinite loop to keep running
        displayGrid();
        updateGrid();
        usleep(200000); // Delay for 0.2 seconds (200,000 microseconds)
    }

    return 0;
}
