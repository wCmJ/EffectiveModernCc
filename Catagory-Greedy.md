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

## 121. 买卖股票 1次
```cpp

int maxProfit(vector<int> &nums){
  int len = nums.size(), ans = 0;
  for(int i = 1, min_price = nums[0];i<len;++i){
    ans = max(ans, nums[i] - min_price);
    min_price = min(min_price, nums[i]);
  }
  return ans;
}

```

## 122. 买卖股票 任意多次
```cpp
int maxProfit(vector<int> &nums){
  int len = nums.size(), ans = 0;
  for(int i = 1;i<len;++i){
    ans += max(0, nums[i] - nums[i - 1]);
  }
  return ans;
}

```

## 123. 买卖股票 最多两次交易
```cpp
int maxProfit(vector<int> &nums){
  int len = nums.size();
  vector<vector<vector<int>>> dp(len, vector<vector<int>>(3, vector<int>(2, 0)));
  dp[0][1][1] = -nums[0];
  dp[0][2][1] = -nums[0];
  for(int i = 1;i<len;++i){
    for(int j = 1;j<3;++j){    
      dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + nums[i]);
      dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - nums[i]);
    }  
  }
  return max(dp[len-1][0][0], max(dp[len-1][1][0], dp[len-1][2][0]));
}
```


## 188, 买卖股票 最多k次
```cpp
int maxProfit(int k, vector<int>& nums){
  int len = nums.size();
  vector<vector<vector<int>>> dp(len, vector<vector<int>>(k + 1, vector<int>(2, 0)));
  for(int j = 1;j<=k;++j){
    dp[0][j][1] = -nums[0];
  }
  for(int i = 1;i<len;++i){
    for(int j = 1;j<=k;++j){
      dp[i][j][0] = max(dp[i-1][j][0], dp[i-1][j][1] + nums[i]);
      dp[i][j][1] = max(dp[i-1][j][1], dp[i-1][j-1][0] - nums[i]);
    }
  }
  int ans = dp[len - 1][0][0];
  for(int j = 1;j<=k;++j){
    ans = max(ans, dp[len - 1][j][0]);
  }
  return ans;
}
```

## 309. 买卖股票 带冷冻期
```cpp
// dp[i][j]: 表示第i天的第j中状态所能获得的最大利润。j表示的状态有：0（不持有股票且不在当天卖出股票），1（持有股票），2（不持有股票且在当天卖出）
/*
  最终结果必在某一种状态中
*/
int maxProfit(vector<int> &nums){
  int len = nums.size();
  vector<vector<int>> dp(len, vector<int>(3, 0));
  dp[0][1] = -nums[0];
  for(int i = 1;i<len;++i){
    dp[i][0] = max(dp[i-1][0], dp[i-1][2]);
    dp[i][1] = max(dp[i-1][1], dp[i-1][0] - nums[i]);
    dp[i][2] = dp[i-1][1] + nums[i];
  }
  return max(dp[len-1][0], dp[len - 1][2]);
}




```





















