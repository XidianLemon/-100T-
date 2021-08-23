# 100大盗，是男人就做100道！
----------
## 目录
### 一、数组
1.二分查找

2.移除元素

3.有序数组的平方

4.长度最小的子数组

5.螺旋矩阵II

数组总结

### 二、链表
1.反转链表

### 三、哈希表
TODO

----------

## 二、链表

## 1.反转链表

反转链表目前只做迭代法，不做递归，这种计算机思想暂时不好理解，跳过。

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



