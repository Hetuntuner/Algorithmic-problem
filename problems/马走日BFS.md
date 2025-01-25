这是一个特别经典的问题

**题目描述**
***************
输入四个数n， m， x， y分别表示棋盘大小，以及最开始马的初始位置，现在叫你求出马跳到其他所有格子所需要的最少步数，如果不能跳到某个格子则在该格子的位置输出-1

**样例**：
input ：
3 3 1 1
output：
0       3       2
3      -1      1
2       1       4
******

经典的问题就要使用经典的模版：**BFS**

BFS通常需要由这些一起构成：
1. 一个记录某个格子是否走过的st【】【】数组
2. 一个队列来存储新的方案数

细节地方：
因为我们是一圈一圈的在搜索，所以我们搜到后面的时候会不好记录后面的步数，这个时候我们的队列就派上用场了，

```C++
#include <iostream>
#include <queue>
#include <cstring>
using namespace std;

const int N = 404; // 棋盘最大范围是400
int n, m, x, y;

int ans[N][N], st[N][N];

void bfs(int x, int y)
{
    ans[x][y] = 0, st[x][y] = 1;
    queue<int> qx, qy;
    int dx[8] = {2, 2, 1, 1, -2, -2, -1, -1};
    int dy[8] = {1, -1, 2, -2, 1, -1, 2, -2};

    qx.push(x), qy.push(y);
    while (!qx.empty())
    {
        int xx = qx.front();
        int yy = qy.front();
        for (int i = 0; i < 8; i++)
        {
            int a = xx + dx[i];
            int b = yy + dy[i];
            if (a >= 0 && a < n && b >= 0 && b < m && !st[a][b])
            {
                st[a][b] = 1;
                ans[a][b] = ans[qx.front()][qy.front()] + 1; // 这是我觉得最巧秒的地方！！！！！！！！！！！！！
                qx.push(a), qy.push(b);
            }
        }
        qx.pop();
        qy.pop();
    }
    return;
}

int main()
{
    cin >> n >> m >> x >> y;
    x--, y--; // 因为我们后面的数组是从0 0开始定义的，我们这里直接将x y向左上移动一个单位即可
    
    memset(ans, -1, sizeof ans);
    bfs(x, y);
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            cout << ans[i][j] << '\t';
        }
        if (i != n - 1)
            cout << '\n'; // 只要不是最后一排，每当内部循环结束了就需要输出换行
    }

    return 0;
}
```