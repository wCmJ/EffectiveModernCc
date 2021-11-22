## 11. 盛最多水的容器
```cpp
/*
  ONE SHOT
  给定n个非负整数，找出其中的两条线，使得它们与x轴共同构成的容器可以容纳最多的水
  思路： 双指针，一个指针从头开始往右走，一个指针从末端开始往左走。
        走动规则：每次取出头指针和尾指针中高度较小的那个，那么以头指针、尾指针和x轴构成的容器可以容纳的水为 height * (end - start)
        更新规则：头指针往右更新，如果下一个高度小于等于当前较小的高度，那么往右更新，直到高度大于当前高度，尾指针以同样条件往左更新
*/
int maxArea(vector<int>& height) {
  int len = height.size(), ans = 0;
  int start = 0, end = len - 1;
  while(start < end){
    int h = min(height[start], height[end]);
    ans = max(ans, h * (end - start));
    while(start < end && height[start] <= h){
      ++start;
    }
    while(start < end && height[end] <= h){
      --end;
    }  
  }
  return ans;
}
```


## 15. 三数之和
```cpp
// 给定一个n个整数的数组，判断数组中是否存在三个元素a, b, c，使得a + b + c = 0，找出所有和为0且不重复的三元组
/*
  思路：排序数组，然后从左往右遍历每一个不同的值（下标为index），从后面的元素中选出满足条件的二元组（由三元组简化为二元组）
  优化：因为已经排好序，当第一个元素>0时，由它及其后面的元素不可能相加为0，可直接结束判断
*/
vector<vector<int>> threeSum(vector<int>& nums) {
  int len = nums.size();
  if(len < 3)return vector<vector<int>>();
  vector<vector<int>> ans;
  // sort
  sort(nums.begin(), nums.end());
  for(int i = 0;i<=len-3 && nums[i] <= 0;){
    int start = i + 1, end = len - 1;
    while(start < end){
      int val = nums[i] + nums[start] + nums[end];
      if(val == 0){
        ans.push_back(vector<int>{nums[i], nums[start], nums[end]});
        while(start + 1 < end && nums[start] == nums[start + 1]){
          ++start;
        }
        ++start;
        while(start < end - 1 && nums[end - 1] == nums[end]){
          --end;
        }
        --end;
      }
      else if(val > 0){
        while(start < end - 1 && nums[end - 1] == nums[end]){
          --end;
        }
        --end;
      }
      else{
        while(start + 1 < end && nums[start] == nums[start + 1]){
          ++start;
        }
        ++start;
      }
    }
    // ignore duplicate nums
    while(i + 1 <= len - 3 && nums[i] == nums[i + 1]){
      ++i;
    }
    ++i;
  }
  return ans;
}
```










