## 215. 数组中第K个最大元素
```cpp
int findKthLargest(vector<int>& nums, int k) {
  // method1: 数组增序排序，倒数第k个即为答案
  {
    sort(nums.begin(), nums.end());
    return nums[nums.size() - k];
  }
  // method2: 快速排序思想，不断迭代至下标为len-k
  {
    int start = 0, end = nums.size() - 1, ans = 0, len = nums.size();
    while(start <= end){
      // 
      int index = start - 1;
      for(int i = start;i<end;++i){
        if(nums[i] < nums[end]){
          ++index;
          int temp = nums[index];
          nums[index] = nums[i];
          nums[i] = temp;
        }
      }
      ++index;
      int temp = nums[index];
      nums[index] = nums[end];
      nums[end] = temp;
      
      if(index == len - k){
        ans = nums[index];
        break;
      }
      else if(index < len - k){
        start = index + 1;      
      }
      else{
        end = index - 1;
      }    
    }
    return ans;
  }
  
  // method3 堆排序
  {
    priority_queue<int, vector<int>, Less> min_heap;
    for(auto n: nums){
      if(min_heap.size() < k){
        min_heap.push(n);
      }
      else{
        if(n > min_heap.top()){
          min_heap.pop();
          min_heap.push(n);
        }      
      }
    }
    return min_heap.top();
  }

}
```

## 206. 反转链表

## 236. 二叉树的最近公共祖先

## 124. 二叉树的最大路径和

## 48. 旋转图像

## 297. 二叉树的序列化及反序列化

## 146. LRU缓存机制

## 53. 最大子序和

## 224. 基本计算器

## 151. 翻转字符串里的单词

## 543. 二叉树的直径

## 91. 解码方法

## 32. 最长有效括号


## 468. 验证IP地址

## 3. 无重复的最长子串


## 141. 环形链表

## 103. 二叉树的锯齿形遍历


## 39. 组合总和


## 207. 课程表

## 450. 删除二叉搜索树中的节点







