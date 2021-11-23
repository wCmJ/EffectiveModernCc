## 45. 跳跃游戏②
```cpp

int jump(vector<int>& nums){
  int len = nums.size();
  int max_jump = 0, ans = 0, index = 0;
  while(max_jump < len - 1){
    ++ans;
    int temp_max_jump = max_jump;
    while(index <= temp_max_jump){
      max_jump = max(max_jump, index + nums[index]);
      ++index;
    }
  }
  return ans;
}
```

## 55. 跳跃游戏，能否跳至最后
```cpp
bool canJump(vector<int> &nums){
  int len = nums.size();
  int max_jump = 0, index = 0;
  while(max_jump < len - 1){
    int temp_max_jump = max_jump;
    while(index <= temp_max_jump){
      max_jump = max(max_jump, index + nums[index]);
      ++index;
    }
    if(max_jump == temp_max_jump){
      return false;
    }
  }
  return true;
}


```

## 122. 
















