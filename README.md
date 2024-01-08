```c
#include <stdio.h>
#include <stdbool.h>

#define NUM_LAMPS 8

// 函数声明
void printLamps(bool lamps[]);
void toggleLamps(bool lamps[], int n);
bool checkWin(bool lamps[]);

int main() {
    bool lamps[NUM_LAMPS] = {false}; // 所有灯泡初始状态为关闭
    int n;

    // 游戏介绍只显示一次
    printf("|------------------------------------------------------|\n");
    printf("|              Lamp Toggle Game by tan91               |\n");
    printf("|------------------------------------------------------|\n");
    printf("The n is the serial number of the lamp, and m is the state of the lamp.\n");
    printf("If m of the Nth lamp is 1, it's on; if not, it's off.\n");
    printf("At first, all the lights are off.\n");
    printf("You can input n to change its state.\n");
    printf("But pay attention: if you change the state of the Nth lamp,\n");
    printf("the state of (N-1)th and (N+1)th will be changed too.\n");
    printf("When all lamps are on, the flag will appear.\n");
    printf("Input n (1-8) to toggle a lamp, 0 to restart.\n");
    printf("|******************************************************|\n");

    while (true) {
        printLamps(lamps); // 显示当前灯泡状态
        printf("Input n: ");
        scanf("%d", &n);

        if (n == 0) {
            // 重置游戏
            for (int i = 0; i < NUM_LAMPS; i++) {
                lamps[i] = false;
            }
            printf("Game restarted.\n");
        } else if (n >= 1 && n <= NUM_LAMPS) {
            // 切换灯泡状态
            toggleLamps(lamps, n - 1); // 数组索引从0开始
        } else {
            printf("Invalid input. Please enter a number between 1 and 8, or 0 to restart.\n");
            continue;
        }

        // 检查是否赢得游戏
        if (checkWin(lamps)) {
            printf("Congratulations! You've won the game!\nThe flag is flag{bdc2421f-9565-1e61-9263-29bda002ba5d}\n");
            break;
        }
    }

    // 防止程序自动关闭
    printf("Press enter to exit...\n");
    getchar(); // 消耗之前的换行符
    getchar(); // 等待用户输入

    return 0;
}

// 打印灯泡状态
void printLamps(bool lamps[]) {
    char *symbols[NUM_LAMPS] = {"△", "○", "◇", "□", "☆", "▽", "(￣▽￣)／", "(;°Д°)"};
    char *onSymbols[NUM_LAMPS] = {"▲", "●", "◆", "■", "★", "▼", "(°Д°)", "(*°▽°)=3"};
    char *switchSymbol = "／"; // 默认开关符号
    char *onSwitchSymbol = "-"; // 灯泡亮起时的开关符号

    for (int i = 0; i < NUM_LAMPS; i++) {
        if (lamps[i]) {
            printf("|------------%s --------%s--------|\n", onSwitchSymbol, onSymbols[i]);
        } else {
            printf("|------------%s --------%s--------|\n", switchSymbol, symbols[i]);
        }
    }
    // printf("|------------------------------------------------------|\n");
}

// 切换灯泡状态
void toggleLamps(bool lamps[], int n) {
    // 切换当前灯泡状态
    lamps[n] = !lamps[n];

    // 切换前一个灯泡状态
    int prev = (n - 1 + NUM_LAMPS) % NUM_LAMPS; // 使用模运算来实现环形切换
    lamps[prev] = !lamps[prev];

    // 切换后一个灯泡状态
    int next = (n + 1) % NUM_LAMPS; // 使用模运算来实现环形切换
    lamps[next] = !lamps[next];
}

// 检查是否所有灯泡都亮了
bool checkWin(bool lamps[]) {
    for (int i = 0; i < NUM_LAMPS; i++) {
        if (!lamps[i]) {
            return false;
        }
    }
    return true;
}

```

