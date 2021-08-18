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
