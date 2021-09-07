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

2.k个一组反转链表

3.移除链表元素

4.两两交换链表中的节点

5.删除链表的倒数第N个节点

6.链表相交

7.环形链表

链表总结

### 三、哈希表
1.有效的字母异位词

2.两个数组的交集

3.快乐数

----------

## 三、哈希表

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



