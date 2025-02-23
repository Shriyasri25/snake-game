# snake-game
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <windows.h>

#define WIDTH 20
#define HEIGHT 20

int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100], nTail;

int gameOver;
enum eDirection { STOP = 0, UP, DOWN, LEFT, RIGHT };
enum eDirection dir;

void setup() {
    gameOver = 0;
    dir = STOP;
    x = WIDTH / 2;
    y = HEIGHT / 2;
    fruitX = rand() % WIDTH;
    fruitY = rand() % HEIGHT;
    score = 0;
    nTail = 0;
}

void draw() {
    system("cls");
    for (int i = 0; i < WIDTH; i++) {
        printf("#");
    }
    printf("\n");
    for (int i = 0; i < HEIGHT; i++) {
        for (int j = 0; j < WIDTH; j++) {
            if (j == 0 || j == WIDTH - 1) {
                printf("#");
            } else if (i == y && j == x) {
                printf("O");
            } else if (i == fruitY && j == fruitX) {
                printf("F");
            } else {
                int print = 0;
                // Snake tail
                for (int k = 0; k < nTail; k++) {
                    if (i == tailY[k] && j == tailX[k]) {
                        printf("o");
                        print = 1;
                    }
                }
                if (!print) {
                    printf(" ");
                }
            }
        }
        printf("\n");
    }
    for (int i = 0; i < WIDTH; i++) {
        printf("#");
    }
    printf("\n");

    printf("\nScore: %d\n", score);
}

void input() {
    if (_kbhit()) {
        switch (_getch()) {
            case 'w':
                dir = UP;
                break;
            case 'a':
                dir = LEFT;
                break;
            case 's':
                dir = DOWN;
                break;
            case 'd':
                dir = RIGHT;
                break;
            default:
                break;
        }
    }
}

void logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    int prev2X, prev2Y;
    tailX[0] = x;
    tailY[0] = y;

    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    switch (dir) {
        case UP:
            y--;
            break;
        case DOWN:
            y++;
            break;
        case LEFT:
            x--;
            break;
        case RIGHT:
            x++;
            break;
        default:
            break;
    }
    if (x < 0 || x >= WIDTH || y < 0 || y >= HEIGHT) {
        gameOver = 1;
    }
    for (int i = 0; i < nTail; i++) {
        if (x == tailX[i] && y == tailY[i]) {
            gameOver = 1;
        }
    }
    if (x == fruitX && y == fruitY) {
        score += 10;
        fruitX = rand() % WIDTH;
        fruitY = rand() % HEIGHT;
        nTail++;
    }
}

int main() {
    srand(time(NULL));
    setup();
    while (!gameOver) {
        draw();
        input();
        logic();
        Sleep(100);
    }
    return 0;
}
