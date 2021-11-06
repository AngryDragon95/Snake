#include <iostream>
#include <string>
#include <vector>
#include <ctime>
#include <stdlib.h>
#include <conio.h>

using namespace std;

bool gameOver;
const int width = 30;
const int height = 30;
int x, y, fruitX, fruitY, score;
int tailX[100], tailY[100];
int nTail;
enum eDirection { STOP = 0, Left, Right, Up, Down };
eDirection dir;

void Setup() {
    gameOver = false;
    dir = STOP;
    x = width / 2 - 1;
    y = height / 2 - 1;
    fruitX = rand() % width;
    fruitY = rand() % height;
    score = 0;
}

void Draw() {
    system("cls");
    for (int i = 0; i < width + 1; i++)
        cout << "_";
    cout << endl;

    for (int i = 0; i < height; i++) {
        for (int j = 0; j < width; j++) {
            if (j == 0 || j == width - 1) {
                cout << "|";
            }
            if (i == y && j == x) {
                cout << "0";
            }
            else if (i == fruitY && j == fruitX) {
                cout << "O";
            }
            else {
                bool print = false;
                for (int k = 0; k < nTail; k++) {
                    if (tailX[k] == j && tailY[k] == i) {
                        print = true;
                        cout << "o";
                    }
                }
                if (!print)
                cout << " ";
            }
        }
        cout << endl;
    }

    for (int i = 0; i < width + 1; i++)
        cout << "`";
    cout << endl;
    cout << "Score: " << score << endl;
}

void Input() {
    if (_kbhit()) {
        switch (_getch())
        {
        case 'w':
            dir = Up;
            break;
        case 'a':
            dir = Left;
            break;
        case 's':
            dir = Down;
            break;
        case 'd':
            dir = Right;
            break;
        case 'r':
            gameOver = true;
            break;
        }
    }
}

void Logic() {
    int prevX = tailX[0];
    int prevY = tailY[0];
    tailX[0] = x;
    tailY[0] = y;
    int prev2X, prev2Y;

    for (int i = 1; i < nTail; i++) {
        prev2X = tailX[i];
        prev2Y = tailY[i];
        tailX[i] = prevX;
        tailY[i] = prevY;
        prevX = prev2X;
        prevY = prev2Y;
    }

    switch (dir)
    {
    case Left:
        x--;
        break;
    case Right:
        x++;
        break;
    case Up:
        y--;
        break;
    case Down:
        y++;
        break;
    }

    if (x > width || x < 0 || y > height || y < 0) {
        gameOver = true;
    }

    for (int i = 0; i < nTail; i++) { //ізза цього вилітає
        if (tailX[i] == x && tailY[i] == y) {
            gameOver = true;
        }
    }

    if (x == fruitX && y == fruitY) {
        score += 1;
        fruitX = rand() % width;
        fruitY = rand() % height;
    }
    nTail++;
}
int main() {
    Setup();

    while (!gameOver) {
        Draw();
        Input();
        Logic();
    }
    return 0;
}