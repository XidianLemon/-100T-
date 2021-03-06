# 100大盗，是男人就做100道！
----------
## 目录
### 一、数组
1.二分查找

1.1.x的平方根

1.2.搜索插入位置

1.3.在排序数组中查找元素的第一个和最后一个位置

1.4.有效的完全平方数

2.移除元素

2.1.删除有序数组中的重复项

2.2.删除有序数组中的重复项 II

2.3.所有删除重复项的总结

2.4.移动零

2.5.比较含退格的字符串

3.有序数组的平方

4.长度最小的子数组

4.1.水果成篮

4.2.最小覆盖子串

5.螺旋矩阵II

5.1.螺旋矩阵，顺时针打印矩阵

数组总结

### 二、链表
1.反转链表

2.k个一组反转链表

3.移除链表元素

4.两两交换链表中的节点

5.删除链表的倒数第N个节点

6.链表相交

7.环形链表

链表总结

### 三、哈希表
1.有效的字母异位词

1.1.字母异位词分组

1.2.找到字符串中所有字母异位词

2.两个数组的交集

2.1.两个数组的交集 II

3.快乐数

4.两数之和

5.四数相加 II

6.赎金信

7.三数之和

8.四数之和

哈希总结

### 四、字符串
1.反转字符串

2.反转字符串II

3.替换空格

4.翻转字符串里的单词

5.左旋转字符串

6.实现 strStr()

7.重复的子字符串

字符串总结

### 五、栈与队列
1.用栈实现队列

2.用队列实现栈

3.有效的括号

4.删除字符串中的所有相邻重复项

5.逆波兰表达式求值

6.滑动窗口最大值

7.前 K 个高频元素

栈与队列总结

### 六、双指针法
1.移除元素

2.反转字符串

3.替换空格

4.翻转字符串里的单词

5.反转链表

6.删除链表的倒数第 N 个结点

7.链表相交

8.环形链表 II

9.三数之和

10.四数之和

双指针总结

### 七、二叉树
1.二叉树的递归遍历

2.二叉树的迭代遍历

3.验证二叉搜索树

4.二叉树的最近公共祖先

5.二叉搜索树的最近公共祖先

### 八、动态规划
1.斐波那契数

2.爬楼梯

3.使用最小花费爬楼梯

4.不同路径

5.不同路径 II

6.打家劫舍

7.最大子数组和

### 拾遗
1.走迷宫深度优先搜索

2.找连通域

----------

## 拾遗

----------

## 2.找连通域
面小鹏汽车，给定一个二维01数组，问里面有多少连着的1，注意连着的1不包括斜着的，只能上下左右连通，才能叫连通域。

首先两个for循环，当找到第一个1，我从这个1开始深度优先搜索，每搜索到一个1，就将该处的1置0。那么我还得有一个数组，专门记录我走过的路，一开始这个数组和给定的那个迷宫大小一样，但是全0，每走过一格，就令该格置1。在深搜时候首先判断是否越界，没越界就判断是否该处是1，且没有走过。都满足我就再往里搜，搜之前记得将该点置0，且标记该点已经走过。直到所有的都搜完（指的是外面那两个for循环）。当然了，每次找到第一个1，我就将连通域计数+1。

```
#include <iostream>
#include <math.h>
#include<limits.h>
#include <vector>
#include <unordered_map>
#include <algorithm>

using namespace std;

class tiktok2 {

private:
    vector<vector<int>> next_ = {{0,1},//向右走
                                {1,0},//向下走
                                {0,-1},//向左走
                                {-1,0}};//向上走
    vector<vector<int>> cor_vec_ = {{0, 1, 0, 0, 0, 1, 1},
                                    {1, 1, 0, 0, 0, 1, 1},
                                    {0, 0, 0, 1, 0, 0, 0},
                                    {0, 0, 0, 1, 1, 1, 0},
                                    {1, 0, 0, 0, 0, 1, 0},
                                    {1, 0, 0, 0, 0, 1, 0}};
    int record_passed_vec_[6][7] = {{0}}; // 记录走过的路，最开始全是0，走过了就标1

public:
    void dfs(int x, int y, const vector<vector<int>>& maze) {

        int tx, ty, k;

        /*枚举四种走法*/
        for (k = 0; k <= 3; k++) {
            /*计算下一个点的坐标*/
            tx = x + next_[k][0];
            ty = y + next_[k][1];

            //判断是否越界
            if ((tx < 0) || (tx > (maze.size() - 1)) || (ty<0) || (ty > (maze[0].size() - 1)))
                continue;

            // 判断该点是否是1（也就属于连通域内），还判断该点是否已经走过了，
            if ((maze[tx][ty] == 1) && (record_passed_vec_[tx][ty] == 0)) {
                record_passed_vec_[tx][ty] = 1;  // 标记这个点已经走过
                cor_vec_[tx][ty] = 0; // 标记该点为0
                dfs(tx, ty, maze);  // 开始尝试下一个点
            }
        }
        return;
    }

    int FindConnectedDomain() {

        int count_connectdomain_result = 0;

        for (int i = 0; i < cor_vec_.size(); i++) {
            for (int j = 0; j < cor_vec_[0].size(); j++) {

                // 走过了就是1，这个数组专门记录走过的
                record_passed_vec_[i][j] = 1;

                if (cor_vec_[i][j] == 1) { // 当找到了第一个1，则开始寻找此坐标的上下左右为1的值，直到找不下去了
                    dfs(i, j, cor_vec_);

                    // 找完一个连通域，连通域个数+1
                    count_connectdomain_result++;
                }
            }
        }
        return count_connectdomain_result;
    }
};

int main() {
    std::cout << "Hello, World!" << std::endl;

    tiktok2 m_tiktok2;
    int result = m_tiktok2.FindConnectedDomain();
    std::cout << result << std::endl;

    return 0;
}
```

----------

## 1.走迷宫深度优先搜索

面字节跳动原题，是一道图论的深度优先搜索的题目，给定一个二维数组，从左上走到右下，只能走0，不能走1，把路过的路劲坐标打印出来。

深度优先搜索遍历，在进入dfs时，首先判断是否到达了目标位置如果到了，标志位置true，直接返回，此时返回后，往下执行的地方应当再次判断标志位是否达到了，达到了就直接返回。

深度优先搜索还是上下左右四种走法，搜索顺序就是右下左上，当判断越界了，则搜下一次。若该点没有处于之前搜过的，且不是1（也就是该位置不能走）就标记这个点已经搜过了，然后将该点放入最后的vec里。将该点传入进行下次递归dfs，没搜到的话我就将vec的最后放入的那一项pop_back。同时取消这个点的标记因为没搜索到所以该点一定不在路径中。

测试集尝试了几个。

```
// 字节跳动原题
// 0, 0, 1, 0, 0
// 0, 1, 0, 0, 0
// 0, 0, 0, 1, 0

// 0, 0, 1, 0, 0
// 1, 0, 0, 0, 0
// 0, 0, 0, 1, 0

// 0, 1, 1, 0, 0
// 0, 0, 1, 0, 0
// 1, 0, 0, 0, 0

// 0, 1, 0, 0, 0
// 1, 1, 0, 0, 0


class tiktok1 {
public:
    void dfs(int x, int y, const vector<vector<int>>& maze, bool& is_find) {

//        std::cout<<"x = " << x<<"," << "y = "<<y<<std::endl;

        int tx,ty,k;
        if(x==maze.size()-1 && y==maze[0].size()-1)  //判断是否到达小哈的位置
        {
//            cor_vec.push_back({x, y});
            is_find = true;
            return;
        }
        /*枚举四种走法*/
        for(k=0;k<=3;k++)
        {
            /*计算下一个点的坐标*/
            tx=x+next[k][0];
            ty=y+next[k][1];

            if(tx<0 || tx>(maze.size()-1) || ty<0 || ty>(maze[0].size()-1))  //判断是否越界
                continue;
            /*判断该点是否为障碍物或者已经在路径中*/
            if(maze[tx][ty]==0 && book[tx][ty]==0)
            {
                book[tx][ty]=1;  //标记这个点已经走过
                cor_vec.push_back({tx, ty});
                dfs(tx, ty, maze, is_find);  //开始尝试下一个点
                if (is_find) {
                    return;
                }
                cor_vec.pop_back();
                book[tx][ty]=0;  //尝试结束，取消这个点的标记
            }
        }
        return;
    }

    // 输入的maze是个迷宫，里面含有0和1，只能走0，不能走1。从左上，走到右下，把路径坐标打印
    void FindPath(const vector<vector<int>>& maze) {
        int startx = 0;
        int starty = 0;

        book[startx][starty] = 1;
        bool is_find = false;
        cor_vec.push_back({0,0});
        dfs(startx, starty, maze, is_find);
        for (auto& i : cor_vec) {
            std::cout<<i[0]<<" "<< i[1]<<std::endl;
        }
    }

private:
    int book[3][5] = {{0}};
    int next[4][2] = {
            {0,1},//向右走
            {1,0},//向下走
            {0,-1},//向左走
            {-1,0},//向上走
    };
    vector<vector<int>> cor_vec;
};

int main() {
    std::cout << "Hello, World!" << std::endl;

    tiktok1 m_tiktok1;
    vector<vector<int>> maze = {{0, 1, 1, 0, 0}, {0, 0, 1, 0, 0}, {1, 0, 0, 0, 0}};
    m_tiktok1.FindPath(maze);
}
```


----------

## 八、动态规划

----------

## 7.最大子数组和

动归五步，dp表示的含义是：第i项之前，包括了第i项，它的和最大是多少。当然是连续的子序列。
递推公式：由于我dp含义是第i项之前，包括第i项，那么我i-1项就是在i-1项之前包括了第i-1项的和最大是多少（这个值可能为负数！）。
如果dp[i-1]为负数，那么我dp[i]就应该等于nums[i]，而如果dp[i-1]是正的，那么我dp[i]就等于dp[i-1]+nums[i]，所以取二者最大的。
如下所示，最后三步就不列了。

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int result = nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1] + nums[i], nums[i]);
            if (dp[i] > result) result = dp[i];
        }
        return result;
    }
};

// 1.确定dp含义
// dp[i] 表示在第i项之前，包括i项，的最大的那个和
// 2.递推公式：因为第i项之前，不包括i项，他们的和可能是负数，那么加上第i项反而更小了，而i-i又是由i-i之前的项推出来的，因此我要看是第i项大，还是第i项加上i-1项大。
// dp[i] = max(dp[i-1]+nums[i], nums[i]);
// 3.从前到后
// 4.dp[0] = nums[0];
// 5. -2, 1, -3, 4, -1, 2, 1, -5, 4
//    -2, 1, -2, 4,  3, 5, 6,  1, 5
```

----------

## 6.打家劫舍

首先判断数组长度如果是0，直接返回0，是1，则返回的就是第0项。然后dp数组含义就是当前房子最多能偷多少，当然包括之前的。接着确定递推公式：当前房子最多能偷多少，由于不能相邻，因此应当等于上一间房和上上间+当前的，看谁大，就用谁，dp数组初始化的话，就直接根据递推公式基础就是dp0和dp1。最后返回dp的最后一项。

```
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];

        vector<int> dp(nums.size());

        dp[0] = nums[0];
        dp[1] = max(nums[1], nums[0]);

        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i]);
        }

        return dp[nums.size() - 1];
        
    }
};

//1.确定dp数组含义：dp[i] 表示当前房子最多能偷盗多少，当然包括之前的
//2.确定递推公式：dp[i] = max(dp[i - 1], nums[i] + dp[i - 2])
//3.确定dp数组初始化，dp[0] = nums[0], dp[1] = nums[1]
//4.确定遍历顺序：从前向后
//5.举例递推公式：
//2, 7, 9,  3,  1
//2, 7, 11, 11, 12
```

## 5.不同路径 II
这道题和上一题不一样的地方在于初始化dp数组的时候，遇到障碍物后，后面的dp全都置0。在循环中递推的时候，若遇见障碍物，则那一位的dp置0。注意题目给的参数是表示含有障碍物的二维数组。上一题的参数两个int，因为里面没有障碍物。dp数组是dp数组，障碍物数组是障碍物数组。

```
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) {
            if (obstacleGrid[i][0] == 1) {
                break;
            }
            dp[i][0] = 1;
        }
        for (int j = 0; j < n; j++) {
            if (obstacleGrid[0][j] == 1) {
                break;
            }
            dp[0][j] = 1;
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (obstacleGrid[i][j] == 0) {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                } else {
                    dp[i][j] = 0;
                }
            }
        }
        return dp[m-1][n-1];
    }
};

//1.dp数组的含义：dp[i][j]表示第i行第j列走到这里有多少不同的路
//2.递推公式：dp[i][j] = dp[i-1][j] + dp[i][j-1]，若遇见障碍物，则该dp数组为0
//3.初始化dp：dp[i][0]都为1，若遇见障碍物，之后都为0，dp[0][j]都为1，若遇见障碍物，之后都为0
//4.遍历顺序，从左往右，从上到下
//5.列递归
//  1   1   1
//  1   0   1
//  1   1   2
```

----------

## 4.不同路径
本题是个二维的dp数组，不同于一维数组的是，递推公式不同了，初始化时候需要初始化第一行和第一列。

```
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) dp[i][0] = 1;
        for (int j = 0; j < n; j++) dp[0][j] = 1;
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }
};

//1.确定dp数组含义：dp[i][j]表示第i行j列有多少走法
//2.确定递推公式：dp[i][j] = dp[i][j-1] + dp[i-1][j]
//3.初始化dp数组：dp[i][0]都为0，dp[0][j]都为0
//4.确定递推顺序，从左到有，从上到下
//5.举例推导dp数组
//  1   1   1   1   1   1   1
//  1   2   3   4   5   6   7
//  1   3   6   10  15  21  28
```

----------

## 3.使用最小花费爬楼梯
题目非常难懂，大概意思是给一个数组cost[]，数组相当于当前台阶的花费，1就是花费1,100就是花费100。一次要么走一个台阶，要么走2个台阶，cost数组虽然长度是n，但是n+1才是楼顶。楼顶不需要花费。我们一开始在0-1的位置，走到0要花费cost[0]那么多花费，而第一次可以走到1，同样的要花费cost[1]那么多花费。

读懂题目的意思那就是列动归5步：1、确定dp数组及下标含义，本题dp[i]指的是到第i个台阶的最小花费。2、确定递推公式：dp[i]获取就是通过dp[i-1]和dp[i-2]，这两个选谁就是看谁小我就选谁，然后再加上当前的花费，dp[i] = min(dp[i-1], dp[i-2]) + cost[i]。3、dp数组初始化，以前两个台阶的最小花费，那么就初始化dp[0]直接就初始化为cost[0]，而dp[1]初始化为cost[1]，因为这两个花费都是直接走到的。而之后就需要根据这两个来递推。4、确定遍历顺序，从前到后。5、距离推导dp数组，见代码注释

```
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        vector<int> dp(cost.size());
        dp[0] = cost[0];
        dp[1] = cost[1];
        for (int i = 2; i < cost.size(); i++) {
            dp[i] = min(dp[i - 1], dp[i - 2]) + cost[i];
        }
        return min(dp[cost.size() - 1], dp[cost.size() - 2]);
    }
};
//1.确定dp[i]意义：dp[i]表示第i个台阶最小的花费体力值
//2.确定递推公式：dp[i] = min(dp[i-1], dp[i-2]) + cost[i]
//3.确定初始：dp[0]=cost[0], dp[1] = cost[1];
//4.确定顺序：从前往后
//5.
//  i   0   1   2   3   4   5   6   7   8   9
//  dp  1   100 2   3   3   103 4   5   104 6
```

----------

## 2.爬楼梯
动规5步：1、确定dp数组及下标含义，本题dp[i]指的是第i个台阶共有几种跳法。2、确定递推公式，本题递推公式需要列出来前几个，然后发现规律dpi=dpi-1+dpi-2。3、dp数组初始化，题目给定n为正整数，则dp1=1,dp2=2，从1开始。4、确定遍历顺序，从前到后。5、举例推导dp数组，我这里先举例12358没有问题。

```
class Solution {
public:
    int climbStairs(int n) {
        if (n == 1) return 1;
        vector<int> dp(n+1);
        dp[1] = 1;
        dp[2] = 2;
        for (int i = 3; i <= n; i ++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
// 1.确定dp数组及下标含义
// dp[i]表示第i层，有几种方法
// 2.确定递推公式
// 台阶数  1   2   3   4   5
// 方法    1   2   3   5   8
// dp[i] = dp[i - 1] + dp[i - 2]
// 3.dp数组初始化
// dp[1] = 1;
// dp[2] = 2;
// 4.确定遍历顺序
// 从前到后
// 5.举例推导dp数组
// 当n为5，dp数组就是：12358
```

----------

## 1.斐波那契数

1、确定dp数组及下标含义，本题dp数组指的是斐波那契数列的每一项，下标含义就是每一项。动归2、确定递推公式，本题递推公式也给出了就是dp[i]=dp[i-1]+dp[i-2]。3、dp数组初始化，dp[0]=0,dp[1]=1。4、确定遍历顺序，从前到后。5、举例推到dp数组，按照递推公式举例，0112358没有问题。

```
class Solution {
public:
    int fib(int n) {
        if (n <= 1) return n;
        vector<int> dp(n+1);
        dp[0] = 0;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};
```

----------

## 七、二叉树

## 5.二叉搜索树的最近公共祖先

递归法

三部曲：1.确定递归函数的返回值以及参数，返回值是最近的公共祖先，参数就是当前节点和两个节点pq。2.确定终止条件，找到头了就返回。3.确定单层递归逻辑，二叉搜索数的性质就是左边比右边小，那么如果当前的值大于p值，且大于q值，那么就应该往左子树找。如果都小于则往右子树找。而一个大于一个小于，说明当前就是最近公共祖先了！要注意搜索的是一条边，如果这条边没有就找另一条边，而不是搜索整棵树。

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root->val > p->val && root->val > q->val) {
            return lowestCommonAncestor(root->left, p, q);
        } else if (root->val < p->val && root->val < q->val) {
            return lowestCommonAncestor(root->right, p, q);
        } else return root;
    }
};
```

## 4.二叉树的最近公共祖先

这道题从底向上查找，符合后序遍历。几种情况：1.一个节点，它的左子树出现节点p，右子树出现节点q，那么该节点就是最近的公共祖先。情况2：节点本身就是p，他的子孙节点有q，那么他本身就是最近的公共祖先。

递归三步：1.确定递归函数返回值和参数：要返回最近的公共祖先，没有就是null。2.确定终止条件：如果找到了节点qp，或者到头了，就返回。3.确定单层递归逻辑：要遍历整颗树，left和right都遍历完成才可以返回。

```
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == q || root == p || root == NULL) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left != NULL && right != NULL) return root;

        if (left == NULL && right != NULL) return right;
        else if (left != NULL && right == NULL) return left;
        else  { //  (left == NULL && right == NULL)
            return NULL;
        }

    }
};
```

----------

## 3.验证二叉搜索树

递归法

中序遍历的应用，因为要让左节点小于根节点，根节点小于右节点，根节点在中间。
标准的中序遍历的写法，每次都记录下来前一个节点，这样在中序遍历过程中，只要前一个节点比当前根节点大了，则返回false。直到搜索完就返回true。左子树和右子树都找完，就返回两者的交集，有一个是假，就是假。

```
class Solution {
public:
    TreeNode* pre = NULL; // 用来记录前一个节点
    bool isValidBST(TreeNode* root) {
        if (root == NULL) return true;
        bool left = isValidBST(root->left);

        if (pre != NULL && pre->val >= root->val) return false;
        pre = root; // 记录前一个节点

        bool right = isValidBST(root->right);
        return left && right;
    }
};
```

迭代法

同样是中序遍历的应用，每次都记录下来前一个节点，在中序遍历过程中，只要前一个比当前根节点大了根节点大了，则返回false。

```
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        stack<TreeNode*> st;
        TreeNode* cur = root;
        TreeNode* pre = NULL;
        while (cur != NULL || !st.empty()) {
            if (cur != NULL) { // 指针来访问节点，访问到最底层
                st.push(cur); // 将访问的节点放进栈
                cur = cur->left;                // 左
            } else {
                cur = st.top(); // 中
                st.pop();
                if (pre != NULL && cur->val <= pre->val)
                    return false;
                pre = cur;
                cur = cur->right;               // 右
            }
        }
        return true;
    }
};
```

## 2.二叉树的迭代遍历

前序遍历和后序遍历类似，都是操作栈。首先说前序遍历，将头结点放入栈中，然后进入循环，直接将栈顶的值放到最终结果。接着用一个节点存栈顶元素，再栈顶删除，注意此时先判断这个节点（存下来的栈顶）的右是否为空，不为空就入栈，后判断这个节点的左是否为空，不为空就入栈，一定要注意此时的顺序。栈不为空就一直循环。

```
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result;
        }
        stack<TreeNode*> st;
        st.push(root);
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->right) st.push(node->right);
            if (node->left) st.push(node->left);
        }
        return result;
    }
};
```

后序遍历前面过程一样，将头结点放入栈中，进入循环，直接将栈顶的值放入最终结果，接着用一个节点存栈顶元素，再删除栈顶，注意此时先判断这个节点的左，再判断右，因为循环结束后，需要反转整个vector结果才能得到最终结果。

```
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result;
        }
        stack<TreeNode*> st;
        st.push(root);
        
        while (!st.empty()) {
            TreeNode* node = st.top();
            st.pop();
            result.push_back(node->val);
            if (node->left) st.push(node->left);
            if (node->right) st.push(node->right);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

中序遍历需要使用一个指针访问到最底部的元素，首先一个cur结点指向根结点，然后进入循环，如果cur结点不为空或者栈不为空，则一直循环，循环中判断cur是否为空，不为空则将cur结点放入栈中，cur则指向cur左。如果cur为空则使用一个node结点存栈顶结点，然后删除栈顶结点，将结点的值放入结果中，cur指向node结点的右。最终返回结果。思路大体就是从根节点一直往最左下找，每个最左边的都入栈。当找到的为空，则开始删栈顶，之后往右找。

```
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        if (root == nullptr) {
            return result;
        }
        stack<TreeNode*> st;
        TreeNode* cur = root;
        while (cur != nullptr || !st.empty()) {
            if (cur != nullptr) {
                st.push(cur);
                cur = cur->left;
            } else {
                TreeNode* node = st.top();
                st.pop();
                result.push_back(node->val);
                cur = node->right;
            }
        }
        return result;
    }
};
```
----------

## 1.二叉树的递归遍历

递归要注意三点：1.确定参数和返回值；2.确定终止条件；3.确定单层递归逻辑，那么以前序遍历为例，参数就是节点和要返回的那个结果vector，终止条件就是当前遍历节点为空则返回，单层递归逻辑前序就是将节点的值放入。这里背诵的技巧就是前序遍历就是将中节点的值放vec放在前，中序遍历就放中，后序就放后。

```
class Solution {
public:
    void traversal(TreeNode* cur, vector<int>& vec) {
        if (cur == nullptr) {
            return;
        }
        //vec.push_back(cur->val);前
        traversal(cur->left, vec);
        //vec.push_back(cur->val);中
        traversal(cur->right, vec);
        //vec.push_back(cur->val);后
    }
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
};
```

----------

## 六、双指针法

## 双指针总结

数组的双指针，移除元素注意最好不用erase，因为erase的时间复杂度就是n，放到for循环中就是n方。

字符串的双指针交换元素就一个for循环，前后两指针往中间移动交换元素。替换空格要从后往前，一个指针指着原来的，一个指针指着之后的。

翻转链表、链表求环经典的双指针题目。

N数之和双指针往中间靠，去重操作。

----------

## 10.四数之和
先排序，进入循环，先判断该数和上一个是不是一样，去重，然后再进入循环，判断该数是不是和上一个一样，去重。这样固定了两个数，接着找左右指针，一个在固定的那个的后面，一个在最后，和为target就放进去，然后先去重，再收缩，其他的操作和三数之和完全一样。

```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        for (int i = 0; i < nums.size(); i++) {
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            for (int j = i + 1; j < nums.size(); j++) {
                if (j > i + 1 && nums[j] == nums[j - 1]) {
                    continue;
                }
                int left = j + 1;
                int right = nums.size() - 1;
                while (left < right) {
                    if (target - nums[i] - nums[j] > nums[left] + nums[right]) {
                        left++;
                    } else if (target - nums[i] - nums[j] < nums[left] + nums[right]) {
                        right--;
                    } else {
                        result.push_back({nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        left++;
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```

----------

## 9.三数之和
先排序，进入循环，固定一个，先判断这一个是否大于0，如果大于0直接返回结果，然后剩余那两个一个在固定的那个后面，一个在最后。看和不为0就往中间靠，为0就放进去。然后先去重，再收缩，注意去重要用while不是if。收缩完毕接着看和，直到左大于等于右。这一次的就结束。

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > 0) {
                return result;
            }

            if (i > 0 && nums[i - 1] == nums[i]) {
                continue;
            }

            int left = i + 1;
            int right = nums.size() - 1;
            while (left < right) {
                if (0 - nums[i] < nums[left] + nums[right]) {
                    right--;
                } else if (0 - nums[i] > nums[left] + nums[right]) {
                    left++;
                } else {
                    result.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) {
                        left++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right--;
                    }
                    left++;
                    right--;
                }
            }
        }
        return result;
    }
};
```

----------

## 8.环形链表 II
一上来应当先判断链表是否只有一个节点或者一个节点都没有，那么直接返回null。然后定义一个快慢指针，快的指针一次走两个，慢的指针一次走一个，直到相遇，就break，如果没有相遇说明没有环，则返回null。此时快指针在相遇的地方，我让慢指针回到head，然后同时走，相遇的地方就是环的入口。

```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == NULL || head->next == NULL) {
            return NULL;
        }
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast != NULL && fast->next != NULL) {
            slow = slow->next;
            fast = fast->next->next;
            if (slow == fast) {
                break;
            }
        }
        //meetIndex存着相遇的位置

        //没相遇说明没环
        if (slow != fast) {
            return NULL;
        }

        //让slow从头开始走，fast接着走
        slow = head;
        while (slow != fast) {
            slow = slow->next;
            fast = fast->next;
        }

        return fast;
    }
};-
```

----------

## 7.链表相交
首先计算两链表长度，然后让长的那个统一为a链表，长的减去短的，让长的先走这个差距，然后两个一起走，其中一个不为空则就一直走，直到遇见相等的就返回。

```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {

        //求a的长度
        ListNode* tempLNA = headA;
        int lenA = 0;
        while (tempLNA != NULL) {
            lenA++;
            tempLNA = tempLNA->next;
        }
        
        //求b的长度
        ListNode* tempLNB = headB;
        int lenB = 0;
        while (tempLNB != NULL) {
            lenB++;
            tempLNB = tempLNB->next;
        }

        if (lenA < lenB) {
            swap(lenA, lenB);
            swap(headA, headB);
        }
        //保证a始终最长

        ListNode* curA = headA;
        ListNode* curB = headB;

        //让a先走差的那么多步
        int diff = lenA - lenB;
        while (diff != 0) {
            diff--;
            curA = curA->next;
        }

        while (curA != NULL) {
            if (curA == curB) {
                return curA;
            }
            curA = curA->next;
            curB = curB->next;
        }
        return NULL;
    }
};
```


----------


## 6.删除链表的倒数第 N 个结点
本题方法1就是让一个节点走到要删除的前一个，然后让这个节点指向要删除的后一个。那么由于题目给的是倒数第n个节点，因此我们应当首先计算出链表长度，然后再让节点走到前一个。

```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* tempListnode = dummy;
        int size = -1;
        while (tempListnode != nullptr) {
            tempListnode = tempListnode->next;
            size++;
        }
        ListNode* tempLn = dummy;
        int temp2 = size - n;
        while (temp2 != 0) {
            tempLn = tempLn->next;
            temp2--;
        }

        tempLn->next = tempLn->next->next;
        return dummy->next;
    }
};
```

方法2，这种删除的问题都是要定义虚拟头结点，虚拟头结点在这里指向head。fast和slow都等于dummy，然后让fast先走n+1步，也就是走到第n个后面，然后一块走，直到fast走到外面就停止，这时slow正好在倒数第n个前面，删除就可以了。最后返回dummy的next。

```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0);
        dummy->next = head;
        ListNode* fast = dummy;
        ListNode* slow = dummy;
        while (n != 0 && fast != nullptr) {
            n--;
            fast = fast->next;
        }
        fast = fast->next;
        while (fast != nullptr) {
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return dummy->next;
    }
};
```


----------

## 5.反转链表
双指针，一个指向链表的前一个，一个指向当前的，前一个在初始化时候指向null，当前的在初始化时候指向head，然后每次反转，pre和cur就一起往后移动，直到cur移出去了，也就是指向null，就结束，返回pre。

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        ListNode* cur = head;
        while (cur != NULL) {
            ListNode* temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```

----------

## 4.翻转字符串里的单词
不能用erase删除冗余空格，因为erase本身时间复杂度就是n，再放到for循环中时间复杂度就是n的平方。因此移除冗余空格就是一个子问题，首先应该移除开头的空格，然后移除中间的冗余空格，最后移除最后的空格。而且不能使用erase库函数。这样就用到了双指针删除指定元素的那道题的思想，如果等于值就只动快的，不等于就快的赋值给慢的，然后一起动。但是这里有一个问题，当删除完如果最后一个字符是空格则没有删除，那么最后还要判断是否移除了空格，移除了就resize慢指针当前所指的位，没有移除就resize慢指针的前一位。

然后我将所有的字符串reverse，然后再将其中每一个字符串reverse，同样的，不要调库函数，使用swap反转。

然后在整个字符串中找到每一个单词，再让每一个单词反转。

```
class Solution {
public:
    void reverse(string& s, int start, int end) {
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
    void removeSpace(string& s) {
        int fast = 0;
        //删除前面的冗余空格
        for (int i = 0; i < s.size(); i++) {
            if (s[i] != ' ') {
                break;
            }
            fast++;
        }
        //至此fast存着第一个不为空格的字符

        //删除中间冗余空格
        //happy  
        int slow = 0;
        for (; fast < s.size(); fast++) {
            if (fast - 1 > 0 && s[fast] == s[fast - 1] && s[fast] == ' ') {
                continue;
            } else {
                s[slow] = s[fast];
                slow++;
            }
        }
        //至此slow存着最后一个字符的后一项，slow-1存着最后一个字符，此时s只可能在最后一位存在一个空格

        //删除最后的冗余空格
        if (slow - 1 > 0 && s[slow - 1] == ' ') {
            s.resize(slow - 1);
        } else {
            s.resize(slow);
        }
            
    }
    string reverseWords(string s) {
        removeSpace(s);
        reverse(s, 0, s.size() - 1);
        for (int i = 0; i < s.size(); i++) {
            int j = i;
            while (j < s.size() && s[j] != ' ') {
                j++;
            }
            reverse(s, i, j - 1);
            i = j;
        }
        return s;
    }
};
```

----------

## 3.替换空格
首先需要找出字符串里有多少空格。然后计算替换后的总空格数量为原字符串长度+2倍的空格数量。这之后一定要注意的是记得将原空格resize，新的长度，否则就会出现问题。将字符串按照从后往前遍历，同样双指针，一个是从老的字符串长度，一个是从新的字符串长度。如果遇到空格则将这一项替换为0，这一项前一项替换为2，再前一项替换为%，直到第0项。

```
class Solution {
public:
    string replaceSpace(string s) {
        int countSpace = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == ' ') {
                countSpace++;
            }
        }
        int newSize = s.size() + 2 * countSpace;
        int oldSize = s.size();
        cout<<"newsize = " << newSize<<endl;
        cout<<"oldSize = " << oldSize<<endl;
        s.resize(newSize);
        for (int i = oldSize - 1, j = newSize - 1; i >= 0; i--, j--) {
            if (s[i] == ' ') {
                s[j] = '0';
                s[j - 1] = '2';
                s[j - 2] = '%';
                j = j - 2;
            } else {
                s[j] = s[i];
            }
        }
        return s;
    }
};
```

----------

## 2.反转字符串
双指针，一个指向开头，一个指向结尾，同时往中间靠，直到相遇，每次都swap。

```
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }
};
```

----------

## 1.移除元素
首先定义一个慢指针，指向0，快指针也是0开始，如果当前快指针的值等于给的值，则继续移动快指针，慢指针保持不动，如果当前快指针的值不等于给定的值，则将当前的快指针赋值给慢指针，然后慢指针往后移动1位。最后返回慢指针指向的那一位就可以了。

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != val) {
                nums[slow] = nums[i];
                slow++;
            }
        }
        return slow;
    }
};
```

----------

## 五、栈与队列

## 栈与队列总结

面试题：栈里面的元素在内存中是连续分布的吗？

答：栈是容器适配器，底层容器使用不同的容器，导致栈内数据在内存中不是连续分布的。默认情况底层容器是deque，deque也是不连续的。

括号匹配，字符串去重，逆波兰表达式等匹配问题都是使用栈解决的经典问题。

队列的经典问题包括：滑窗最大值，前k个高频元素。其中滑窗最大值引出单调队列，前k个高频元素引出优先级队列，大顶堆小顶堆。

## 7.前 K 个高频元素
首先使用纯哈希的方法，优先级队列暂时我没看懂，等看懂后再用优先级队列。

方法1，纯哈希，首先将数组中的数放入哈希，first应当是数，second应当是频率。然后再将哈希放入一个二维数组中，注意该数组应当pair<int, int>，此时应当注意的是应该频率放到第一个，而数放到第二个。将该数组排序，那么排序的也就是频率。将频率排序完毕后，输出前k个，注意输出的是每一项的second。上述每一个注意都需要重点注意first和second！

```
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> myMap;
        vector<pair<int, int>> myVec;
        for (auto i : nums) {
            myMap[i] ++;
        }
        // myMap此时first存着是数，second存着是频率
        for (auto it = myMap.begin(); it != myMap.end(); it++) {
            myVec.push_back({it->second, it->first});
        }
        // myVec此时first存着是频率，second存的是数
        sort(myVec.rbegin(), myVec.rend());
        // myVec此时由大到小排序好了
        vector<int> result;
        for (int i = 0; i < k; i++) {
            result.push_back(myVec[i].second);
        }
        return result;
    }
};
```


方法2，优先级队列

优先级队列在这里就是用在了大顶堆和小顶堆上，大小顶堆的固定写法就是如下题中的，当push进去后，小顶堆固定的就是将小的排前面，大的放后面，而它可以当做队列来操作。本题首先实现一个小顶堆，然后将所有数的频率放到哈希表里。之后声明一个小顶堆，也就是优先级队列，以更小的数为优先。然后将哈希中的每一项都放入优先级队列中，当放入的时候，该优先级队列会自动的将放入的数以更小的优先放进去。那么当该优先级队列的size超过了k时，我就删除队头的元素，那么遍历完哈希表后，该优先级队列也就存下来了k个大的数。最后将该优先级队列的所有数都放到结果的vector中，就可以了，需要注意的就是放的时候需要先声明vector的长度，即`vector<int> result(k)`。然后放的时候也是result[i] = 队列.top().first。放完就删了队头元素。

```
class Solution {
public:
    class smallT {
    public:
        bool operator() (const pair<int, int>& lh, const pair<int, int>& rh) {
            return lh.second > rh.second;
        }
    };
    vector<int> topKFrequent(vector<int>& nums, int k) {
        unordered_map<int, int> myMap;
        for (int i = 0; i < nums.size(); i++) {
            myMap[nums[i]] ++;
        }
        priority_queue<pair<int, int>, vector<pair<int, int>>, smallT> myQue;
        for (unordered_map<int, int>::iterator it = myMap.begin(); it != myMap.end(); it++) {
            myQue.push(*it);
            if (myQue.size() > k) {
                myQue.pop();
            }
        }
        vector<int> result(k);
        for (int i = 0; i < k; i++) {
            result[i] = myQue.top().first;
            myQue.pop();
        }
        return result;
    }
};
```

----------

## 6.滑动窗口最大值

这道题按照暴力的方法好做，但是提交会超时。降低时间复杂度，就需要想策略。需要一个结构，该结构的头是最大的数，当我往该结构放的时候（也就是滑窗往右移动的时候），如果放的数比前面的数大，就把前面的数都删了，直到放的数比前面的小或者都删干净了为止。然后再放到该结构中。我要删除，也就是当滑窗往右移动的时候，删除也判断，删除的数是不是正好处在该结构的头上，不在该结构的头上，就不删除。因为他是目前已知最大的，而且不出滑窗，则不要删除。

先将前k个依次push到该结构中，然后将此时该结构的头放进结果。然后从第k个开始向后遍历，每一个都是先删除操作，删除的就是第i-k那一项，然后push操作，push就是第i项。然后将该结构的头放进结果中。
最后返回结果就可以了。

```
class Solution {
public:
    deque<int> myDeque;

    //删除的正好是头上那个，我才删
    void pop(int popNum) {
        if (!myDeque.empty() && myDeque.front() == popNum) {
            myDeque.pop_front();
        }
    }

    void push(int pushNum) {
        while (!myDeque.empty() && pushNum > myDeque.back()) {
            myDeque.pop_back();
        }
        myDeque.push_back(pushNum);
    }

    int front() {
        return myDeque.front();
    }

    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        for (int i = 0;i < k;i ++) {
            this->push(nums[i]);
        }
        vector<int> result;
        result.push_back(this->front());

        for (int i = k;i < nums.size();i ++) {
            this->pop(nums[i - k]);
            this->push(nums[i]);
            result.push_back(this->front());
        }
        return result;
    }
};
```

----------

## 5.逆波兰表达式求值

这道题最适合就是栈，给定的字符串，一项一项找，找到数字就放进栈，找到操作符就让栈顶的两个元素根据操作符相加，记得把栈顶元素删除，然后再把计算得到的数字再放回栈中。值得注意的是栈顶和栈顶第二个，哪个是放在操作符前，哪个是操作符后。然后减完记得转int，因为之前是字符转的。栈是int，返回也是int。最终直接返回栈顶的计算结果就行了。

```
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> st;
        for (int i = 0;i < tokens.size();i ++) {
            if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/") {
                int num1 = st.top();
                st.pop();
                int num2 = st.top();
                st.pop();
                if (tokens[i] == "+") {
                    st.push(num2 + num1);
                }
                if (tokens[i] == "-") {
                    st.push(num2 - num1);
                }
                if (tokens[i] == "*") {
                    st.push(num2 * num1);
                }
                if (tokens[i] == "/") {
                    st.push(num2 / num1);
                }
            } else {
                st.push(stoi(tokens[i]));
            }
        }
        return st.top();
    }
};
```

----------

## 4.删除字符串中的所有相邻重复项

使用一个辅助字符串，判断原字符串的第i项 跟辅助串最后一项是否相同，相同就删了，不同就放进辅助串。最后返回辅助串就可以了。

```
class Solution {
public:
    string removeDuplicates(string s) {
        string st;
        for (char S : s) {
            if (S == st.back()) {
                st.pop_back();
            } else {
                st.push_back(S);
            }
        }
        return st;
    }
};
```

----------

## 3.有效的括号

首先用栈来存所有的右括号，只要我在遍历字符串的时候遇见了左括号，我就放一个右括号，对应括号的类型放。如果不是左括号呢，我就判断此时栈 是否为空，空的话就说明不是有效的，直接返回false。如果不是空则判断栈顶是否和字符串的这一位括号相同，相同则删除栈顶元素，不同则返回false。这样一圈遍历下来，如果此时栈是空的，则返回true，不是空的返回false。

```
class Solution {
public:
    bool isValid(string s) {
        stack<int> st;
        for (int i = 0;i < s.size();i ++) {
            if (s[i] == '(') {
                st.push(')');
            } else if (s[i] == '[') {
                st.push(']');
            } else if (s[i] == '{') {
                st.push('}');
            } else if (st.empty() || s[i] != st.top()) {
                return false;
            } else {
                st.pop();
            }
        }
        return st.empty();
    }
};
```

----------

## 2.用队列实现栈

这道题pop比较麻烦，别的都正常用就可以了。同样两个队列，pop实现过程：队列1的元素依次放到队列2中，留下最后一个，然后删除这个，再把队列2中所有的元素放到队列1中，清空队列2。有了思路就好写了。

```
class MyStack {
public:
    queue<int> que1;
    queue<int> que2;
    MyStack() {

    }
    
    void push(int x) {
        que1.push(x);
    }
    
    int pop() {
        int size = que1.size();
        while (size != 1) {
            que2.push(que1.front());
            que1.pop();
            size --;
        }
        int result = que1.front();
        que1.pop();
        while (!que2.empty()) {
            que1.push(que2.front());
            que2.pop();
        }
        return result;
    }
    
    int top() {
        return que1.back();
    }
    
    bool empty() {
        return que1.empty();
    }
};
```

----------

## 1.用栈实现队列


push就是push，删除的话首先判断out栈是否为空，不为空则直接删，为空则先将in栈所有的都依次放到out栈中，再删。peek取也是和pop一样的，只是记得删完再放回去。空则判断这两个栈是否都为空。


```
class MyQueue {
public:
    stack<int> stackOut;
    stack<int> stackIn;
    MyQueue() {

    }
    
    void push(int x) {
        stackIn.push(x);
    }
    
    int pop() {
        if (stackOut.empty()) {
            while (!stackIn.empty()) {
                stackOut.push(stackIn.top());
                stackIn.pop();
            }
        }
        int result = stackOut.top();
        stackOut.pop();
        return result;
    }
    
    int peek() {
        int result = this->pop();
        stackOut.push(result);
        return result;
    }
    
    bool empty() {
        return stackOut.empty() && stackIn.empty();
    }
};
```


----------

## 四、字符串

## 字符串总结

替换空格这类的题，也就是填充，可以先给数组扩容填充后的大小，然后从后往前操作。如果要求时间复杂度，慎用库函数，比如erase的时间复杂度是on。


## 7.重复的子字符串

kmp获取next数组，传入的字符串就是要判断的字符串。该字符串如果存在重复的子字符串，那么一定它的next数组一定会有这个性质：最后一位大于0，且字符串长度 % （字符串长度 - next数组最后一位） == 0，那么获得next数组后，就判断是否有这个性质，有就返回true。

```
class Solution {
public:
    void getNext(int* next, const string& s) {
        int j = 0;
        next[0] = 0;
        for (int i = 1; i < s.size(); i ++) {
            while (j > 0 && s[i] != s[j]) {
                j = next[j - 1];
            }
            if (s[i] == s[j]) {
                j++;
            }
            next[i] = j;
        }
    }
    bool repeatedSubstringPattern(string s) {
        if (s.size() == 0) {
            return false;
        }
        int next[s.size()];
        int len = s.size();
        getNext(next, s);
        if (next[len - 1] > 0 && (len % (len - next[len - 1]) == 0)) {
            return true;
        }
        return false;
    }
};
```

## 6.实现 strStr()

kmp首先获取next数组，然后两个串分别匹配，匹配不上就回退，回退多少就是看next，回退的循环条件是j>0且两个串一直不匹配。然后看下一个是否能匹配上，能的话j++，再判断此时j是否等于模式串的长度，等于就说明找到了，则返回当前项减去模式串长度再加1，就得到了结果。

    class Solution {
    public:
        void getNext(int* next, const string& s) {
            int j = 0;
            next[0] = 0;
            for (int i = 1; i < s.size(); i++) {
                while (j > 0 && s[i] != s[j]) {
                    j = next[j - 1];
                }
                if (s[i] == s[j]) {
                    j++;
                }
                next[i] = j;
            }
        }
        int strStr(string haystack, string needle) {
            if (needle.size() == 0) {
                return 0;
            }
            int j = 0;
            int next[needle.size()];
            getNext(next, needle);
            for (int i = 0; i < haystack.size(); i++) {
                while (j > 0 && haystack[i] != needle[j]) {
                    j = next[j-1];
                }
                if (haystack[i] == needle[j]) {
                    j ++;
                }
                if (j == needle.size()) {
                    return (i - needle.size() + 1);
                }
            }
            return -1;
        }
    };

## 5.左旋转字符串

首先翻转前n个，再翻转后面所有的，再整体翻转。这个技巧确实很厉害。

```
class Solution {
public:
    string reverseLeftWords(string s, int n) {
        reverse(s.begin(), s.begin() + n);
        reverse(s.begin() + n, s.end());
        reverse(s.begin(), s.end());
        return s;
    }
};
```

----------

## 4.翻转字符串里的单词

这道题有几个步骤，注意不要数组越界！这几个步骤感觉不用分顺序，去掉字符串中的重复空格；去掉头部的空格，去掉尾部空格，翻转整个串，翻转每个单词。使用api的时候要特别注意，reverse第一个参数是要翻转的第一位，第二个参数是要翻转的最后一位的下一位哦！！！erase第一个参数是从第几位开始翻转，第二个参数是翻转几位。最后当你删除了第I项的空格，需要记得I--，否则就会少删一位空格哦！

```
class Solution {
public:
    string reverseWords(string s) {

        //首先翻转整个字符串
        reverse(s.begin(), s.end());

        //将这个字符串中每个单词翻转
        for (int i = 0;i < s.length();i ++) {
            //找不是空格那一位
            while (i < s.length() && s[i] == ' ') {
                i ++;
            }
            //当i超过了数组长度时，则break
            if (i == s.length()) {
                break;
            }
            //此时i指向的是那个单词
            int wordStartIndex = i; //start指的是单词开头的索引
            

            //找单词结束的位置
            while (i < s.length() && s[i] != ' ') {
                i ++;
            }
            // if (i == s.length()) {
            //     i 
            // }
            int wordEndIndex = i;   //end指的是单词结束后，第一个空格的索引

            //翻转这个单词
            reverse(s.begin() + wordStartIndex, s.begin() + wordEndIndex);

        }

        //去掉重复空格
        for (int i = 0;i < s.length();i ++) {
            
            if (i > 0 && s[i] == s[i - 1] && s[i] == ' ') {
                s.erase(i - 1, 1);
                i --;
            }
        }

        //去掉开头处空格
        if (s[0] == ' ') {
            s.erase(0, 1);
        }

        //去掉结尾处空格
        if (s[s.length() - 1] == ' '){
            s.erase(s.length() - 1, 1);
        }
        return s;
    }
};
```

----------


## 3.替换空格


首先数空格的个数，然后声明一个新的字符串，给该字符串定长度：原字符串长度+空格个数乘以2。从后往前遍历，若遇到空格则将新字符串的三位替换为%20，不遇到空格则一直将原字符串的字符放到新字符串中。长度最好用length()。

```
class Solution {
public:
    string replaceSpace(string s) {
        string resStr;
        int countSpace = 0;
        for (int i = 0;i < s.length();i ++) {
            if (s[i] == ' ') {
                countSpace ++;
            }
        }
        resStr.resize(countSpace * 2 + s.length());
        int sSize = s.length() - 1;
        int resSize = resStr.length() - 1;
        for (int i = sSize, j = resSize;i >= 0;i --, j --) {
            if (s[i] == ' ') {
                resStr[j] = '0';
                resStr[j - 1] = '2';
                resStr[j - 2] = '%';
                j = j - 2;
            } else {
                resStr[j] = s[i];
            }
        }
        return resStr;
    }
};

```


----------


## 2.反转字符串II


这题的题目表达很差，转换成好理解的话，就是每隔k个，就反转k个，不足k个就都翻转。知道了这个就简单了，每隔k个，那就是for循环的时候+2k，在for循环中判断当前项是否不足k个了，不足k个就当前到最后都翻转，多于k个就翻转k个。

```
class Solution {
public:
    string reverseStr(string s, int k) {
        for (int i = 0;i < s.length();i = i + 2*k) {
            if (s.length() - i < k) {
                reverse(s.begin() + i, s.end());
            } else {
                reverse(s.begin() + i, s.begin() + i + k);
            }
        }
        return s;
    }
};
```

----------

## 1.反转字符串


双指针比较好，也不用引入额外空间，头尾交换，一起收缩，到中间就停。

```
class Solution {
public:
    void reverseString(vector<char>& s) {
        for (int i = 0, j = s.size() - 1;i < s.size() / 2;i ++, j --) {
            swap(s[i], s[j]);
        }
    }
};
```

----------


## 三、哈希表

## 哈希总结

哈希分为三种：1.数组；2.set；3.map。

1.数组。数组适用于包含小写字母，小写字母有26个，每一个对应数组的一位，每遇见一位，就在对应的位置加1。

2.set。没有限制字符的大小的情况，就不能用数组做哈希了。这是由于数组大小是有限的，受到系统栈空间的限制。而如果数组空间足够大，但哈希值比较少，特别分散，跨度特别大，使用数组就造成空间的极大浪费。那么此时做映射就应当用set。

3.map。数组大小收到限制，如果元素很少，但哈希值太大会造成内存空间浪费。set是一个集合，里面放的元素只能是一个key。那么使用set就只能判断是否存在，但不能记录下标。map是一种键值对的结构，那么使用key保存数值，使用value保存数值所在的下标。


## 8.四数之和


这道题和三数之和类似，值得注意的点在于本题的四数之和是一个随机数，而三数之和是0，因此在每次循环开始的地方就不能判断如果超过target就返回了。三数之和只需要定一个数，左右两个数，四数之和定两个数，因此就需要两层for循环，并且每层for循环在最开始的地方去重。其他的就和三数之和一样了，当左小于右，就一直往中间找，看是大了还是小了，就往中间缩，直到左大于等于右。还有需要注意的是四数之和与target比较大小时，不等号左边就保留两个数的和，右边是target - 另外两个数的和，否则就会出现四数之和超过了int的最大值，执行出错！
                                                     
```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i ++) {
            if (i > 0 && nums[i] == nums[i-1]) {
                continue;
            }
            for (int j = i+1; j < nums.size(); j ++) {
                if (j > i+1 && nums[j] == nums[j-1]) {
                    continue;
                }
                int left = j + 1;
                int right = nums.size() - 1;

                while (left < right) {
                    if (nums[i] + nums[j] < target - nums[left] - nums[right]) {
                        left ++;
                    } else if (nums[i] + nums[j] > target - nums[left] - nums[right]) {
                        right --;
                    } else {
                        result.push_back({nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        left ++;
                        right --;
                    }
                }
            }
        }
        return result;
    }
};
```


----------


## 7.三数之和


不想用哈希做容易错还不好理解，就用快慢指针就可以了，首先排序，然后在最外的for循环中去重。相当于固定其中一个，找剩下两数，这两数先从两头找，当找到后一起去重，去重完毕就收缩。

```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int i = 0;i < nums.size();i ++) {
            if (nums[i] > 0) {
                return res;
            }
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            int left = i + 1;
            int right = nums.size() - 1;
            while(left < right) {
                if (nums[i] + nums[left] + nums[right] > 0) {
                    right --;
                } else if (nums[i] + nums[left] + nums[right]) {
                    left ++;
                } else {
                    res.push_back(vector<int>{nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) {
                        left ++;
                    }
                    while (left < right && nums[right] == nums[right - 1]) {
                        right --;
                    }
                    left ++;
                    right --;
                }
            }
        }
        return res;
    }
};
```

----------

## 6.赎金信


这道题跟第一题类似，同样是将每一个字符和'a'相减，得到的数字放到一个int数组中，注意数组长度是26，要在初始化时候指定！将要从中获取的那个string中所有的都放到数组中，放的时候就++，然后取，因为不能重复使用，因此取了就--，如果这一位小于0，说明要从中获取的数组不够，那就直接返回false。

```
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int arr[26] = {0};
        for (int i = 0;i < magazine.size();i ++) {
            arr[magazine[i] - 'a'] ++;
        }
        for (int i = 0;i < ransomNote.size();i ++){
            arr[ransomNote[i] - 'a'] --;
            if(arr[ransomNote[i] - 'a'] < 0){
                return false;
            }
        }
        return true;
    }
};
```

----------

## 5.四数相加 II


首先遍历前两个数，将前两个数字的和放入一个unordered_map中，注意！由于unordered_map不可重复，因此两数的和重复的那一位就会+1！！！然后再遍历后两个数，在这个unordered_map中查找0 - (c+d)，注意此时查找，找的是key，而这个值对应的是有几个这个值，因此需要将这一位的值累加上去，比如unordered_map umap，umap[i]对应的值就是有几个这个数。因此要将这个值累加！

```
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> umap;
        for (int a : nums1) {
            for (int b : nums2) {
                umap[a + b] ++;
            }
        }
        int count = 0;
        for (int c : nums3) {
            for (int d : nums4) {
                if(umap.find(0 - (c + d)) != umap.end()){
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```

----------

## 4.两数之和


这道题在两年前居然做过，使用的暴力解法，就是两个for循环，没啥意思。卡尔老师讲的使用unordered_map很巧妙。使用target减去当前的数，看是否在unordered_map中出现过，如果出现过则直接返回当前下标和那一项的key。如果没出现过则将当前的下标和数都放进unordered_map中。值得注意的就是使用了auto来接find返回的值，并且用这个auto的对象的second来访问这一项的key。这里是比较陌生的，如果想访问这一项的值就是first。

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> numMap;
        for (int i = 0; i < nums.size(); i ++) {
            auto iter = numMap.find(target - nums[i]);
            if (iter != numMap.end()){
                return {i, iter->second};
            }
            numMap.insert(pair<int, int>(nums[i], i));
        }
        return {};
    }
};
```

----------

## 3.快乐数


这道题也要用哈希，原因是如果该数之和一直不为1，则一定会有一个循环的、不为1的数作为所有位相加的和，而只要有和之前一样的数出现了，就不是快乐数。那么使用unordered_set来存和找是否有一样的数。解题的重点部分就在分解这个整数，首先从个位，该数对10取余则得到了个位数，将个位数求平方。然后该数除以10，得到的是不包含个位数的，再对10取余，得到此时的个位数再平方，一直循环到除以10得到0，就将该数每一位都平方后相加了。看得到的数是不是1。然后将该数在unordered_set中找，有的话则说明陷入循环就返回false，没有则insert进去。

```
class Solution {
public:
    int getSum(int n) {
        int resSum = 0;
        while (n != 0) {
            resSum += (n % 10) * (n % 10);
            n = n / 10;
        }
        return resSum;
    }
    bool isHappy(int n) {
        unordered_set<int> resNumSet;
        while (1) {
            int numSum = getSum(n);
            if (numSum == 1) {
                return true;
            }
            if (resNumSet.find(numSum) != resNumSet.end()) {
                return false;
            } else {
                resNumSet.insert(numSum);
            }
            n = numSum;
        }
    }
};
```

----------

## 2.1.两个数组的交集 II

首先使用一个哈希来存那个短的vec中每个字母出现的次数，然后再遍历那个长的vec，每次遍历时候，如果哈希里存在长的vec这一项，则哈希那一项-1，然后把这一项放进结果vec中。

```
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        if (nums1.size() > nums2.size()) {
            return intersect(nums2, nums1);
        }

        unordered_map<int, int> m;
        vector<int> result;
        for (int i : nums1) {
            m[i]++;
        }
        for (int i : nums2) {
            if (m[i] > 0) {
                m[i]--;
                result.push_back(i);
            }
        }
        return result;
    }
};
```

## 2.两个数组的交集


使用的是unordered_set，查找这个数字是否在另一个数组出现过，查找是否出现过就用这个stl。先使用一个unordered_set，存第一个数组，遍历第二个数组的每一项，看是否在前面的unordered_set中出现过这个数，出现过的就把这个数放到另一个unordered_set中，注意unordered_set中的元素是不能重复的！这和题目要求输出的是一致的，最终使用vector来存这个unordered_set。

```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result;
        unordered_set<int> temp(nums1.begin(), nums1.end());
        for(int num : nums2){
            if(temp.find(num) != temp.end()){
                result.insert(num);
            }
        }
        vector<int> resultVec(result.begin(), result.end());
        return resultVec;
    }
};
```

----------


## 1.2.找到字符串中所有字母异位词

搞两个数组，都是26长度，目的是存26个英文字母出现几次，也就是两个哈希。注意在此后，这两个数组先搞在同一个for循环里遍历，for循环的循环次数就是p的长度，也就是说我先在p里面所有出现的字母都先放到p对应的哈希，然后s在前面那些字母出现次数也放在s对应的哈希中。这样我就是相当于完成了一个初始化滑窗的操作，如果此时s和p相等，那么说明s在前p长度就是和p的异位词。把第0个位置直接放入答案里。

接下来的步骤就是操作滑窗了，从0开始到s的长度 - p的长度，每次都让s的哈希在s[i]-'a'这一项减一个，再在s[i+p的长度]-'a'这一项加一个，也就是滑窗往后移动一个，若此时s的哈希和p的哈希相等，则说明此时s的滑窗就是一个异位词，把i+1push到答案里。

```
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int sLen = s.size(), pLen = p.size();

        if (sLen < pLen) {
            return vector<int>();
        }

        vector<int> ans;
        vector<int> sCount(26);
        vector<int> pCount(26);
        for (int i = 0; i < pLen; ++i) {
            ++sCount[s[i] - 'a'];
            ++pCount[p[i] - 'a'];
        }

//        std::cout<<"sCount = ";
//        for(int i = 0; i < sCount.size(); ++i) {
//            std::cout << sCount[i] << ",";
//        }
//        std::cout << std::endl;
//
//        std::cout<<"pCount = ";
//        for(int i = 0; i < pCount.size(); ++i) {
//            std::cout << pCount[i] << ",";
//        }
//        std::cout << std::endl;

        if (sCount == pCount) {
            ans.emplace_back(0);
        }

        for (int i = 0; i < sLen - pLen; ++i) {
            --sCount[s[i] - 'a'];
            ++sCount[s[i + pLen] - 'a'];

            if (sCount == pCount) {
                ans.emplace_back(i + 1);
            }
        }

        return ans;
    }
};
```


## 1.1.字母异位词分组

首先使用一个哈希，哈希类型声明为：键是给定的那个字符串数组中那些包含相同字母的单词，比如说，eta,ate,eat，他们三个我就存eat为键，值就是所有一样的都存到对应键的值里（值是一个vector<string>）。首先遍历给定的string数组，每次遍历时，将那一项的string先赋值给key，key排序，排序后在哈希对应的第key项，将这个string存入。遍历完整个字符串vec后，我取出map的每一项的值，赋值给最终的结果，结果就是一个二维数组。
    
```
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> mp;
        for (string& str: strs) {
            string key = str;
            sort(key.begin(), key.end());
            mp[key].push_back(str);
        }
        vector<vector<string>> ans;
        for (auto it = mp.begin(); it != mp.end(); ++it) {
            ans.push_back(it->second);
        }
        return ans;
    }
};
```

## 1.有效的字母异位词


哈希表，用数组就可以了。数组记录每个字母出现的次数，那么数组长度就是26个。比如'a'出现第一次，那么数组的第0项就是1，之后再出现就再加1。比较的话就是s[i] - 'a'，加入s[i]是b，相减就是1。那么此时将数组的第1项+1，字符串1加上去一个，字符串2每次遇到就减去这一项，直到size结束，看此时数组里有没有不是0的，有的话一定不是异位词。

```
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0;i < s.size();i ++){
            record[s[i] - 'a']++;
        }
        for (int i = 0;i < t.size();i ++){
            record[t[i] - 'a']--;
        }
        for (int i = 0;i < 26;i ++) {
            if (record[i] != 0){
                return false;
            }
        }
        return true;
    }
};
```

----------


## 二、链表


## 链表总结


几个技巧，虚拟头结点，快慢指针等。反转链表多做了前半部分和一部分的反转，都用的递归，好难，暂时不做那个了。删除链表的话，一定记着先将后驱记录，再删除 ，否则之后就找不到了。


## 7.环形链表


方法一：哈希表，这题用哈希表最方便，也最好理解。哈希表里存着每一个节点，当找了一圈之后，第一个重复的节点就是环的入口。方法二暂时不准备做，以后再补。
```
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        unordered_set<ListNode*> uds;
        while(head != NULL){
            if(uds.count(head)){
                return head;
            }
            uds.insert(head);
            head = head->next;
        }
        return NULL;
    }
};
```

----------

## 6.链表相交


首先计算两个链表的长度，看谁长，长多少，长多少就让长的那个先走几步。这里为了省事就swap了一下，统一让heada长。然后虚拟头结点分别指向两个链表头结点。长的那个先走几步，到了之后就一起走。直到两个节点的next相等或者其中一个走到null就停。while循环结束后，说明下一个就是相交的节点，直接返回下一个就可以了。
```
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        int lengthA = 0;
        int lengthB = 0;
        ListNode* tempA = new ListNode(0);
        tempA->next = headA;
        ListNode* tempB = new ListNode(0);
        tempB->next = headB;
        while (tempA->next != NULL) {
            tempA = tempA->next;
            lengthA ++;
        }
        while (tempB->next != NULL) {
            tempB = tempB->next;
            lengthB ++;
        }
        
        //此时(lengthB - lengthA)存的是两个链表之间相差几个
        //这时应当看谁长，谁长我就让谁先走几步，我统一让第一个链表是长的，
        if(lengthA < lengthB){
            swap(lengthA, lengthB);
            swap(headA, headB);
        }
        int diffLengthAB = lengthA - lengthB;
        ListNode* dummyHeadA = new ListNode(0);
        dummyHeadA->next = headA;
        ListNode* dummyHeadB = new ListNode(0);
        dummyHeadB->next = headB;
        //虚拟头结点，让第一个链表先走(lengthB - lengthA)
        while(diffLengthAB != 0){
            dummyHeadA = dummyHeadA->next;
            diffLengthAB --;
        }
        //两链表同时开始走
        while(dummyHeadA->next != dummyHeadB->next && dummyHeadA->next != NULL && dummyHeadB->next != NULL){
            dummyHeadA = dummyHeadA->next;
            dummyHeadB = dummyHeadB->next;
        }
        return dummyHeadB->next;
    }
};
```

----------

## 5.删除链表的倒数第N个节点


快慢指针，虚拟头结点。首先快慢都在dummy那，然后让快指针走n个，然后快慢同时走，直到快的的next为null就停止删除此时slow的下一个，令slow指向下下个即完成。
```
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* fast = dummyHead;
        ListNode* slow = dummyHead;
        while(n != 0){
            fast = fast->next;
            n--;
        }
        while(fast->next != NULL){
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* tmp = slow->next;
        slow->next = slow->next->next;
        delete tmp;
        return dummyHead->next;
    }
};
```

----------

## 4.两两交换链表中的节点


首先画出图，链表问题必须画图，虚拟头结点必须的。将后面几个节点存下来，我这里就是不知道存谁那就都存，存下来后开始交换，按照cur的next先放，next的next后放，next的next的next最后，则依次将现在的都第一遍前两个都放完了就进行下次循环，直到结束。
```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while(cur->next != nullptr && cur->next->next != nullptr){
            ListNode* curNxt = cur->next;
            ListNode* curNxtNxt = cur->next->next;
            ListNode* curNxtNxtNxt = cur->next->next->next;
            cur->next = curNxtNxt;
            cur->next->next = curNxt;
            cur->next->next->next = curNxtNxtNxt;
            cur = cur->next->next;
        }
        return dummyHead->next;
    }
};
```

----------

## 3.移除链表元素


虚拟头结点应该是一个更好的方法：首先虚拟一个头结点，它得next为head，也就是它指向head，然后当前节点cur不要是head，而是head之前的那一节点也就是dummy，这样的话它就可以判断头结点的val了，而不需要额外添加逻辑判断。之后就一直遍历当前节点cur，只要cur的下一个不为val，则往后找，下一个是val则删除再指向之后的。这期间虚拟节点始终都在head的前一节点，虽然head节点删除了，但是整条链表没断，因此虚拟节点此时的next就是没删除的第一项，将其赋值给head，然后返回head，就返回了整条删除val后的链表。

```
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead->next = head;
        ListNode* cur = dummyHead;
        while(cur->next != NULL){
            if(cur->next->val == val){
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            }else{
                cur = cur->next;
            }
        }
        head = dummyHead->next;
        delete dummyHead;
        return head;
    }
};
```


第一点就是c++需要手动delete节点，就是当移除了指定的链表元素后，delete它。第二点要注意就是特殊情况，整个一个链表都是指定的值或者从头开始是指定的值，那么应当注意的是首先一个while，就是让head指向第一个不为给定值得地方（前面删了的元素记得delete）；然后一个while循环,条件是当前节点和下一节点都不为空则进入，如果下一个节点和指定值不同，则当前节点往后移，如果下一个节点和指定值相同，则删了下一个节点，指向下下个，当前还是在原地不能动。
```
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while(head != NULL && head->val == val){
            ListNode* tmp = head;
            head = head->next;
            delete tmp;
        }
        ListNode* cur = head;
        while(cur != NULL && cur->next != NULL){
            if(cur->next->val == val){
                ListNode* tmp = cur->next;
                cur->next = cur->next->next;
                delete tmp;
            }else{
                cur = cur->next;
            }
        }
        return head;
    }
};
```


----------

## 2.k个一组反转链表


这道题首先要想到的是给你两个链表中的节点，让你反转中间这些，比如从a到b反转，那么要注意应当反转的是a到b-1，b不参与反转，因为b就相当于下次的a。

第一个递归函数就是反转从a到a-1，转过来返回的是pre节点，改节点指的是b-1的节点。

第二个递归函数就是k个一组转了，那么首先将b节点转到第k+1个位置，然后调用上面第一个递归输入就是此时的a和b。这是k个一组其中的一组。然后再调用当前递归函数，输入就是b和k，也就是下一组和k个，而返回值应当令a->next指向它即转之后再和下一组转好的连接上。最后返回第一次掉上面第一个递归那个newhead。

```
class Solution {
public:
    ListNode* reverse(ListNode* a, ListNode* b){
        ListNode* pre = NULL;
        ListNode* cur = a;
        ListNode* nxt = a;
        while(cur != b){
            nxt = cur->next;
            cur->next = pre;
            pre = cur;
            cur = nxt;
        }
        return pre;
    }
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(head == NULL){
            return NULL;
        }
        ListNode* a = head;
        ListNode* b = head;
        for(int i = 0;i < k;i ++){
            if(b == NULL){
                return head;
            }
            b = b->next;
        }
        ListNode* newHead = reverse(a, b);
        a->next = reverseKGroup(b, k);
        return newHead;
    }
};
```


----------


## 1.反转链表


反转链表目前只做迭代法，不做递归，这种计算机思想暂时不好理解，跳过。

记得皮城执法官说过的：如果碰壁了，就用力把它碰开。跳过是不可能跳过的，这辈子不可能跳过的。

递归反转它来了！
首先是全部反转，一上来先判断head是否为null，不这样运行不过哦。然后判断head->next是否为null，这是递归的basecase，如果head的下一个为空则返回head本身。接着将除了头的都反转。最后将头反转。

```
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if(head == NULL){
            return NULL;
        }
        if(head->next == NULL){
            return head;
        }
        ListNode* res = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return res;
    }
};
```

再然后是前一部分反转，这部分同样一上来判断head是否为null，然后希望记录下来转之后的后继节点使用nxt记录，当转n-1次后，也就是basecase是转了n-1次则返回此时的头，但是同时必须要记录下来的是后继。接着递归转n-1次，然后再把头反转，同时头的next应当指向后继。


```
    ListNode* nxt = NULL;
    ListNode* reverseN(ListNode* head, int n){
        if(head == NULL){
            return NULL;
        }
        if(n == 1){
            nxt = head->next;
            return head;
        }
        ListNode* last = reverseN(head->next, n - 1);
        head->next->next = head;
        head->next = nxt;
        return last;
    }
```


最后就是部分反转，部分反转的basecase，就是当left等于1的时候，相当于是前一部分反转，那么此时将头到right都反转，返回的是翻转后的头结点。接着做部分反转的递归，递归后，应当让头的next指向翻转后的头结点。此时已经不用管后继是否街上了因为在前一部分反转时，已经接上了。最后返回头结点就可以了！
head的next指向反转


```
class Solution {
public:
    ListNode* nxt = NULL;
    ListNode* reverseN(ListNode* head, int n){
        if(head == NULL){
            return NULL;
        }
        if(n == 1){
            nxt = head->next;
            return head;
        }
        ListNode* last = reverseN(head->next, n - 1);
        head->next->next = head;
        head->next = nxt;
        return last;
    }
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if(head == NULL){
            return NULL;
        }
        if(left == 1){
            return reverseN(head, right);
        }
        head->next = reverseBetween(head->next, left - 1, right - 1);
        return head;
    }
};
```


反转链表就从头开始转，首先将head和head.next打开，打开后，就找不到next了，那么就需要用一个变量存head.next。存下来后，就可以放心的将head.next赋值为前一个节点了。前一个节点就需要新建一个变量存。head.next赋值为前一个节点这个操作就是反转了。反转后就令当前的和前一个都后移，就是将当前的赋值前一个，之前存的head.next赋值给当前的，就完成了一起后移。


    class Solution {
    public:
        ListNode* reverseList(ListNode* head) {
            ListNode* pre = NULL;
            ListNode* temp;
            while(head){
                temp = head->next;
                head->next = pre;
                pre = head;
                head = temp;
            }
            return pre;
        }
    };


部分反转，此题为92. 反转链表 II，这道题按照下面的方法，先让pre移动到要反转区间的前驱，cur移动到反转区间第一个，cur和第三项相连，第二项和前驱相连，这样就让cur变到了第二位。cur再和下下位相连，下一位在和pre相连。也就是每一次将后面一项提到前面，前面往后移。反转次数就是right - left。


```
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* dummy = new ListNode(-1);
        dummy->next = head;
        ListNode* pre = dummy;
        for(int i = 0;i < left - 1;i++){
            pre = pre->next;
        }
        ListNode* cur = pre->next;
        for(int j = left;j < right;j++){
            ListNode* t = cur->next;
            cur->next = t->next;
            t->next = pre->next;
            pre->next = t;
        }
        return dummy->next;
    }

};
```

----------

## 一、数组

## 数组总结
数组是存放在连续内存空间上的相同类型数据的集合。
数组的元素是不能删的，只能覆盖。
数组下标都是从0开始的。
数组内存空间的地址是连续的。
二维数组在内存的空间地址，不是m*n连续，而是m条连续的地址空间。
经典题目二分查找，快慢指针，滑动窗口，模拟行为。

## 5.1.螺旋矩阵，顺时针打印矩阵

首先给上下左右四个边界赋值，四个方向为一圈，即一次循环。当前的上下左右分别为udlr。以从做往右移动为例，一定是搞一个for循环，每循环一次i++，那么i就代表第i列，每次移动后要将给定数组的第u行和第i列那个数放入结果vec中。这个for循环直到i<=r，即走到右边界结束。那么此时上边界的那一行我遍历完了，应当往内圈缩，即u=u+1，若缩进去后，u<d，即当前上边界比下边界还小，则说明遍历完毕，直接break。返回结果。

再以从上到下为例，同样搞一个for循环，每次循环i++，那么i此时代表着第i行，每次移动后要将给定数组的第i行和第r列放入结果vec中。这个for循环同样的，走到i<=d就结束，然后向左缩，若缩进去后r<l，说明当前右边界比左边界还小，则遍历完毕，break，返回结果。

```
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector <int> ans;
        if(matrix.empty()) return ans; //若数组为空，直接返回答案
        int u = 0; //赋值上下左右边界
        int d = matrix.size() - 1;
        int l = 0;
        int r = matrix[0].size() - 1;
        while(true)
        {
            for(int i = l; i <= r; ++i) {
                ans.push_back(matrix[u][i]); //向右移动直到最右
                // std::cout<< "matrix[u][i] = " <<matrix[u][i]<<std::endl;
            }
            // std::cout<<"u = "<<u<<std::endl;
            u = u + 1;
            if(u > d) {
                break;
            } //重新设定上边界，若上边界大于下边界，则遍历遍历完成，下同


            for(int i = u; i <= d; ++i) {
                ans.push_back(matrix[i][r]); //向下
                // std::cout<< "matrix[i][r] = " <<matrix[i][r]<<std::endl;
            }
            r = r - 1;
            if(r < l) break; //重新设定右边界

            for(int i = r; i >= l; --i) {
                ans.push_back(matrix[d][i]);
                // std::cout<< "matrix[d][i] = " <<matrix[d][i]<<std::endl;
            } //向左
            d = d - 1;
            if(d < u) break; //重新设定下边界

            for(int i = d; i >= u; --i) {
                ans.push_back(matrix[i][l]);
                // std::cout<< "matrix[i][l] = " <<matrix[i][l]<<std::endl;
            } //向上
            l = l + 1;
            if(l > r) break; //重新设定左边界
        }
        return ans;
    }
};
```

## 5.螺旋矩阵II
首先注意二维数组初始化是这样哦！

    vector<vector<int>> res(n, vector<int>(n, 0));  //二维数组初始化

这道题主要是看规律，给的n如果是奇数，则中间会有一项，如果是偶数，中间没有这一项，则在最开始判断是奇偶，奇数则给中间的数赋值。由于是顺时针给数组赋值，转一圈就是一次循环，则看要转几圈。这个圈数则是作为循环的次数。而在每次循环中，要分为四种方向上下左右。每种方向都需要对该方向的每一个元素赋值，则该方向本身要作为一个循环。那么每圈四个循环，循环结束条件是当前的x坐标或者y坐标加到了这一行或这一列的倒数第二项尤其注意这个循环条件！在第四个循环结束时，应当将当前指针指向下次循环开始的那一项，且圈数加1，表示转到第几圈了。总共有n/2圈，如果超过了n/2圈则退出循环，返回，结束。

    class Solution {
    public:
        vector<vector<int>> generateMatrix(int n) {
            vector<vector<int>> res(n, vector<int>(n, 0));  //二维数组初始化
            // int cycle = n / 2;  //看总共需要绕几圈
            int cycletemp = 1;  //绕到了第几圈
            int corX = 0;
            int corY = 0;       //这两个是放的那个坐标
            int num = 1;        //指每次放的那个数
    
            if(n % 2 == 1){
                res[n / 2][n / 2] = n * n;
            }
            while(cycletemp <= n / 2){
                while(corY < n - cycletemp){
                    res[corX][corY] = num;
                    corY = corY + 1;
                    num = num + 1;
                }
                while(corX < n - cycletemp){
                    res[corX][corY] = num;
                    corX = corX + 1;
                    num = num + 1;
                }
                while(corY > cycletemp - 1){
                    res[corX][corY] = num;
                    corY = corY - 1;
                    num = num + 1;
                }
                while(corX > cycletemp - 1){
                    res[corX][corY] = num;
                    corX = corX - 1;
                    num = num + 1;
                }
                corX = corX + 1;
                corY = corY + 1;    //移动到下一个圈的起点
                cycletemp = cycletemp + 1;  //下一圈开始
            }
            return res;
        }
    };


----------

## 4.2.最小覆盖子串

两个哈希，ht存着要找的那个字符的哈希（也就是短的），hs存着滑窗里出现ht里所有字母的哈希（也就是说ht中的字母出现的次数）。定义两个指针ij，表示滑窗。从头开始遍历，如果j处元素在ht中有，则说明此时滑窗中应当加一个，那么hs的j对应这一项应当加1，然后判断当前hs（也就是滑窗中）是否完整的包含了ht，如果全部包含，说明此时滑窗已经包含了完整的，那么我应当收缩滑窗，也就是让i加1，在加之前，我应当判断此时的滑窗是否是长度最小，如果长度最小我应当记录i，为了以后将滑窗中的字符串输出。如果长度不是最小，根本不用记录。在加之前，我还应当判断这个i项是否在ht中，如果在的话，应当让hs哈希里对应的这一项减1（也就是滑窗里出现次数减1）。这样就可以让i加1了，也就是缩滑窗。若缩完以后，hs还是包含完整的ht，那么说明我还可以继续缩，直到包含了完整的ht。直到j出去了，就停止遍历，返回最后存下来的那个i，调用substr，就可以了。

```
class Solution {
public:
    //检查hs中是否包含了ht的全部字符以及个数
    bool check(unordered_map<char, int> &hs, unordered_map<char, int> &ht){
        for(unordered_map<char,int>::iterator it=ht.begin();it!=ht.end();it++){
            if(hs[it->first]<it->second){
                return false;
            }
        }
        return true;
    }
    string minWindow(string s, string t) {
        unordered_map<char, int> hs; //用于保存字符串s的滑动窗口里元素出现次数
        unordered_map<char, int> ht; //保存字符串t中各元素出现次数

        int minlen = INT_MAX; //记录最小长度
        cout<<INT_MAX<<endl;
        int ansL = -1; //子串的左边起点

        for(int i=0;i<t.size();i++) ht[t[i]]++; //遍历t，并统计各字符出现次数
        
        //滑动窗口，让k不断往右移动，找到一个满足条件的子串
        for(int i=0,j=0;i<=j&&j<s.size();){
            //如果j处元素在ht中有，那么j处元素也需要保存到hs中
            if(ht.find(s[j])!=ht.end()) hs[s[j]]++; //往hs里面填充有效元素,非必要的不要放进去

            //如果此时hs已经包含了ht中全部字符，就需要让i往右移
            while(check(hs,ht)){
                //如果当前子串长度更小，则更新
                if(j-i+1<minlen){
                    minlen = j-i+1;
                    ansL = i; //记录起点
                }
                //如果i处元素在ht中有，将出现hs中对应出现次数-1
                if(ht.find(s[i])!=ht.end()) hs[s[i]]--; //元素出现次数减1
                i++; //i往右移
            }
            j++; //j继续右移
        }
        if(ansL==-1) return "";
        else{
            return s.substr(ansL, minlen);
        }
    }
};


// A, D, O, B, E, C, O, D, E, B, A, N, C
//                               i      j
//临时最短：4
```

## 4.1.水果成篮

题目的叙述可以理解为：求只包含两种元素的最长连续子序列，给的vector每一项就是各个元素。

我从左开始遍历给的那个vector，每次在循环开始的地方，我将vector的这一项放入哈希中，然后就判断哈希的size是否超过了2，超过了就收缩。

那么收缩一定是从左往右收缩，直到我哈希的size不大于2了我就停止收缩，我需要使用一个对象来记录左，首先收缩的时候应当先将当前的最左边这一项对应的哈希删除，然后判断最左边这一项对应的哈希是否为0，为0说明已经删干净了，那么我把哈希对应的这一键值对删除，如果不为0说明没删干净，还有重复的，就接着删，最后再让记录左的那个对象+1。这时如果哈希的size还大于2就接着收缩，如果不大于2了就停止收缩，看此时是我之前记录的那个滑窗size大，还是当前的这个滑窗size大，谁大就存谁。滑窗size = 当前遍历第i项的i - 左 + 1；

```
class Solution {
public:
    int totalFruit(vector<int>& tree) {
        int res = 0;
        // 水果编号到数量的映射
        unordered_map<int, int> fruit2Num_Umap;
        int n = tree.size();
        // 窗口左边界
        int left = 0;
        // 从左开始遍历，每次都放哈希，超过就收缩
        for (int i = 0; i < n; ++i)
        {
            ++fruit2Num_Umap[tree[i]];
            // 一旦发现超过大小，则不断地从左收缩窗口
            while (fruit2Num_Umap.size() > 2)
            {
                int currTree = tree[left];
                --fruit2Num_Umap[currTree];
                if (fruit2Num_Umap[currTree] == 0)
                {
                    fruit2Num_Umap.erase(currTree);
                }
                ++left;
            }
            // 记录最大的窗口大小
            res = max(res, i - left + 1);
        }
        return res;
    }
};
// 3,3,3,1,2,1,1,2,3,3,4
```

## 4.长度最小的子数组
### 暴力解法：
太耗时了，思路大概是这样的：我要有一个中间变量存当前的子数组长度，还有一个变量存最小的那个长度。然后两个for循环之间呢，要注意将中间变量长度置0，而且求和的那个sum也要在此置零。再一个就是刚开始比较，也就是第一次累和大于target的时候，需要判断res是否为0，为0则将中间变量赋值给res，作为初始的那个来比较。

    class Solution {
    public:
        int minSubArrayLen(int target, vector<int>& nums) {
            int res = 0;
            int sum = 0;
            int restemp = 0;
            for(int i = 0;i < nums.size();i++){
                sum = 0;
                restemp = 0;
                for(int j = i;j < nums.size();j++){
                    sum = sum + nums[j];
                    if(sum >= target){
                        restemp = j - i + 1;
                        if(res > restemp || res == 0){
                            res = restemp;
                        }
                        break;
                    }
                }
            }
            return res;
        }
    };

### 滑窗（双指针）解法：
同样要设置中间变量存当前长度最小的，思路就是两个指针，i和j，i一直往前动，直到比target大了，就停下！记录下当前长度，然后将j指针往前移动一位，再判断是否比target大，还大则接着记录当前target，如果不大了呢，就令i往前移动一个，接着判断是否比target大。同样的，也应当注意第一次的也就是刚开始比较，第一次累和大于target的时候，需要判断res是否为0，为0则将中间变量赋值给res，作为初始的那个来比较。

    class Solution {
    public:
        int minSubArrayLen(int target, vector<int>& nums) {
            int sum = 0;
            int res = 0;
            int restemp = 0;
            int j = 0;
            for(int i = 0;i < nums.size();i ++){
                sum = sum + nums[i];
                while(sum >= target){
                    restemp = i - j + 1;
                    if(res > restemp || res == 0){
                        res = restemp;
                    }
                    sum = sum - nums[j];
                    j++;
                }
            }
            return res;
        }
    };

----------

## 3.有序数组的平方

双指针法，或者暴力解法，暴力解法没什么可写的，双指针法的话，一定要注意声明数组时候，要给他初始化一个大小，否则运行时会出错！

    class Solution {
    public:
        vector<int> sortedSquares(vector<int>& nums) {
            vector<int> res(nums.size());
            int left = 0;
            int right = nums.size() - 1;
            // cout<<"right = "<<right<<endl;
            for(int i = nums.size() - 1;i >= 0;i --){
                //cout<<"nums[left] = "<<nums[left]<<endl;
                //cout<<"nums[right] = "<<nums[right]<<endl;
                if((nums[left] * nums[left]) >= (nums[right] * nums[right])){
                    res[i] = nums[left] * nums[left];
                    // cout<<"res[i]"<<res[i]<<endl;
                    left ++;
                }else{
                    // cout<<"nums[right] = "<<nums[right]<<endl;
                    res[i] = nums[right] * nums[right];
                    // cout<<"res[i]"<<res[i]<<endl;
                    right --;
                }
            }
            return res;
        }
    };

----------

## 2.5.比较含退格的字符串

搞一个字符串tmp，分别遍历给定的两个字符串，从头遍历，当遇到#号，就删除tmp最后的，如果没遇到，就一直往里放。另一个字符串遍历的时候可以搞个tmp1，往里删放，最后就看tmp和tmp1是不是一样的。

```
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }

    string build(string str) {
        string ret;
        for (char ch : str) {
            if (ch != '#') {
                ret.push_back(ch);
            } else if (!ret.empty()) {
                ret.pop_back();
            }
        }
        return ret;
    }
};

// ab#c
// ad#c

```

## 2.4.移动零

这道题我需要将0移动到后面去，一开始同样令fast和slow都在0，然后让快的和0比较，不等于就让快慢交换，然后一起走，等于0就只让快的走。由于0要移动到末尾，而且要保持相对顺序，因此我从头开始遍历，fast遇到的第一个不为0的我让fast和slow交换，保证了交换给slow的不是0，而且是第一个不是0的，然后一起走一个，再判断。

```
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int size = nums.size();
        int slow = 0;
        for (int fast = 0; fast < size; fast++) {
            if (nums[fast] != 0) {
                swap(nums[slow], nums[fast]);
                slow++;
            }
        }
    }
};

//  1, 3, 12, 0, 0
//        s      f
```

## 2.3.所有删除重复项的总结

题目的模板完全按照2.2来，不等于就快的赋值给慢的，然后一起走，等于就只走快的，最后返回慢的。

## 2.2.删除有序数组中的重复项 II

和上一题一样，首先判断如果只有两个元素就返回size。之后令快的和慢的都在第2个，每次迭代首先判断慢的的前两个是不是等于快的，不等于就令快的赋值给现在的慢的，然后一起走，等于的话就只走快的，最终返回慢的就行了。

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() <= 2) {
            return nums.size();
        }
        int slow = 2;

        for (int fast = 2; fast < nums.size(); ++fast) {
            if (nums[slow - 2] != nums[fast]) {
                nums[slow] = nums[fast];
                slow++;
            }
        }

        return slow;
    }
};
```

## 2.1.删除有序数组中的重复项

首先判断如果只有一个元素，就返回size。之后令快的和慢的都在第1个，每次迭代首先判断慢的的前一个是不是等于快的，不等于就令快的赋值给现在的慢的，然后一起走。等于的话就只走快的。最终返回慢的就行了。

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if (nums.size() < 2){
            return nums.size(); //如果只有一个元素，直接返回
        }
        int slow = 1;
        for (int fast = 1; fast < nums.size(); fast++){
            if (nums[slow - 1] != nums[fast]) {
                nums[slow] = nums[fast]; //nums[j]始终为不重复的元素里面最后一个
                slow++;
            }
        }
        return slow;
    }
};

```

## 2.移除元素
暴力解决的方法着重注意的地方就是当集体前移之后，要注意size--，否则后面都是相同的val，不好处理。尤其注意的是for循环，应当小于size，而不是nums.size()!!!!!!!
双指针法重点是思路，看快指针指的元素是否等于val，等于的话，则慢指针不动，快指针后移；不等的话，快指针的值赋值给慢指针，俩人一起后移。
### 暴力解法
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            int size = nums.size();
            for(int i = 0;i < size;i++){
                if(nums[i] == val){
                    for(int j = i;j < size-1;j++){
                        nums[j] = nums[j+1];
                    }
                    i--;
                    size --;
                }
            }
            return size;
        }
    };
### 双指针
    class Solution {
    public:
        int removeElement(vector<int>& nums, int val) {
            int slow = 0;
            for(int fast = 0;fast < nums.size();fast++)
                if(nums[fast] != val)
                    nums[slow++] = nums[fast];
            
            return slow;
        }
    };

----------

## 1.4.有效的完全平方数

同样推出迭代公式，然后上次和这次相减，很小的话就break。然后判断此时的x的平方是不是给的那个数，返回是或者不是。

```
class Solution {
public:
    bool isPerfectSquare(int num) {
        double x0 = num;
        while (true) {
            double x1 = (x0 + num / x0) / 2;
            if (x0 - x1 < 1e-6) {
                break;
            }
            x0 = x1;
        }
        int x = (int) x0;
        return x * x == num;
    }
};
```

## 1.3.在排序数组中查找元素的第一个和最后一个位置

先找出左右边界，我需要找到的是左边界的左边第一个位置和右边界的右边第一个位置！为什么？

因为这道题总共有三种情况，1不在整个数组范围超出了数组范围，在两边，比如  {1，3，5}  我要找0或者6  ；2.不在整个数组范围，在中间，比如{1，3，5}，我要找2  ；3.在数组范围。

我设置初始左右边界都是-2，二分查找是不断地向中间找，那么如果我找的左右边界有一项为-2，就说明超出了数组范围，情况1；如果我找了一圈右边界的右边的第一个位置 减去 左边界左边的第一个位置，大于1，则说明已经找到了，那么我返回的就是左边界的左边第一个位置 + 1（即左边界），右边界右边的第一个位置 - 1（即右边界），情况2；如果右边界右边第一个位置 和  左边界左边第一个位置都不是-2，且差也不大于1，这时候说明左右边界重合了，且没有找到！因为在右边界右边第一个位置 - 左边界左边第一个位置，哪怕有一个元素，这个差也要大于1！

以左边界为例，使用二分查找，终止条件同样是left<=right，和二分查找不同的地方在于，每次迭代时，如果nums[mid]>=target时，我应当记录下来此时的mid-1这个位置，因为在这时，mid-1的右边都找过了，绝对不是左边界的左边一个，而mid-1这一项可能是左边界，因此我此时就记录下来，一旦迭代终止，我就跳出循环，输出左边界。注意：此时等于也算，为啥呢，找到了等于的，它不一定是左边界，这是因为有连续的，因此还需要接着找，直到终止条件left<=right。 所以这里nums[mid]>=target处理方式一样，同样都是将左边的放到mid-1的位置，记录暂时左边界。

右边界同理了，只要将>=改成<=，记录mid+1的位置为临时右边界。

题目首先找到左右边界，然后根据三种情况，先判断是否有一个是-2（情况1），然后判断左右边界的差是否大于1（情况2），否则返回-1，-1（情况3）。

```
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int leftBorder = getLeftBorder(nums, target);
        int rightBorder = getRightBorder(nums, target);
        // 情况一
        if (leftBorder == -2 || rightBorder == -2) return {-1, -1};
        // 情况三
        if (rightBorder - leftBorder > 1) return {leftBorder + 1, rightBorder - 1};
        // 情况二
        return {-1, -1};
    }
private:
     int getRightBorder(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        int rightBorder = -2; // 记录一下rightBorder没有被赋值的情况
        while (left <= right) {
            int middle = left + ((right - left) / 2);
            if (nums[middle] > target) {
                right = middle - 1;
            } else { // 寻找右边界，nums[middle] == target的时候更新left
                left = middle + 1;
                rightBorder = left;
            }
        }
        return rightBorder;
    }
    int getLeftBorder(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        int leftBorder = -2; // 记录一下leftBorder没有被赋值的情况
        while (left <= right) {
            int middle = left + ((right - left) / 2);
            if (nums[middle] >= target) { // 寻找左边界，nums[middle] == target的时候更新right
                right = middle - 1;
                leftBorder = right;
            } else {
                left = middle + 1;
            }
        }
        return leftBorder;
    }
};
```

## 1.2.搜索插入位置

这题和二分查找类似，在最后返回right + 1，如果size为0，则直接返回-1。在二分查找完毕后，如果找到了则直接返回middle，如果没找到则此时的right正好在middle-1的位置，返回right+1就可以了。

```
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;
        while (left <= right) {
            int middle = left + ((right - left) / 2);
            if (nums[middle] > target) {
                right = middle - 1;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }
        return right + 1;
    }
};
```


## 1.1.x的平方根

这题首先要推公式，求出迭代公式然后在迭代后，判断是否超过了一个很小的数比如1e-7，超过了就停止。迭代公式就是根据直线方程求得的，直线方程的斜率就是x方的导数，也就是2x。值得注意的是这只是整数的平方根，如果要求小数比如0~1之间的数的平方根就是另一种做法了，暂未研究。
首先要做出一条抛物线，抛物线的方程式y=x^2 - c，之所以这样做是因为我要令这条抛物线在x>0的时候和x轴相交，这样交点即给定的那个数。

然后我要在这条抛物线上做切线，每个切线的斜率就是2x，第一个切线的切点就是x=给定的那个数，y就是x^2 - c，而这条切线和x轴的交点就是(x1,0)，这条切线目前就有了两个点(x0,x0^2 - c),(x1,0)，斜率是2x0，那么这条线就可以确定了，之后推导出x1=0.5*(x0+C/X0)，这个就是迭代公式，那么x2也就由x1推导出来，依次找下去，直到相邻两次足够小，就返回。

```
class Solution {
public:
    int mySqrtInt(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (fabs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return int(x0);
    }
    double mySqrtDouble(int x) {
        if (x == 0) {
            return 0;
        }

        double C = x, x0 = x;
        while (true) {
            double xi = 0.5 * (x0 + C / x0);
            if (fabs(x0 - xi) < 1e-7) {
                break;
            }
            x0 = xi;
        }
        return x0;
    }
};
```

## 1.二分查找

二分查找前提是数组有序，无重复元素。
难点是边界条件，区间的定义要想清楚，区间的定义就是不变量，要在二分查找过程中保持不变。
左闭右闭，那么边界条件就是left==right,right初始应当取size-1。

    class Solution {
    public:
        int search(vector<int>& nums, int target) {
            int left = 0;
            int right = nums.size() - 1;
            int middle;
            while(left <= right){
                middle = left + (right - left) / 2;
                if(nums[middle] < target){
                    left = middle + 1;
                }else if(nums[middle] > target){
                    right = middle - 1;
                }else{
                    return middle;
                }
            }
            return -1;
        }
    };




