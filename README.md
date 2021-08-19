力扣刷题

1.二分查找

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

2.移除元素
暴力解决的方法着重注意的地方就是当集体前移之后，要注意size--，否则后面都是相同的val，不好处理。尤其注意的是for循环，应当小于size，而不是nums.size()!!!!!!!

双指针法重点是思路，看快指针指的元素是否等于val，等于的话，则慢指针不动，快指针后移；不等的话，快指针的值赋值给慢指针，俩人一起后移。

暴力解法
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
双指针
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

                                                   
                                                   
                                                   
                                                   
3.有序数组的平方
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

    
4.长度最小的子数组

暴力解法：太耗时了，思路大概是这样的：我要有一个中间变量存当前的子数组长度，还有一个变量存最小的那个长度。然后两个for循环之间呢，要注意将中间变量长度置0，而且求和的那个sum也要在此置零。再一个就是刚开始比较，也就是第一次累和大于target的时候，需要判断res是否为0，为0则将中间变量赋值给res，作为初始的那个来比较。
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
    

滑窗（双指针）解法：同样要设置中间变量存当前长度最小的，思路就是两个指针，i和j，i一直往前动，直到比target大了，就停下！记录下当前长度，然后将j指针往前移动一位，再判断是否比target大，还大则接着记录当前target，如果不大了呢，就令i往前移动一个，接着判断是否比target大。同样的，也应当注意第一次的也就是刚开始比较，第一次累和大于target的时候，需要判断res是否为0，为0则将中间变量赋值给res，作为初始的那个来比较。
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
    
