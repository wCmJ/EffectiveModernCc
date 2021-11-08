## Maximum Sum Subarray of Size K
```cpp
// easy
// given an array of positive numbers and a positive number k, find the maximum sum of any contiguous subarray of size k
/*
input: [2, 1, 5, 1, 3, 2], k = 3
output: 9
*/
int findMaxSumSubarray(vector<int> &nums, int k){
  int len = nums.size();
  if(k < 1 || len < k)return 0;
  int ans = 0, tmp = 0, start = 0, end = 0;
  while(end < len){
    tmp += nums[end++];
    if(end >= k){
      ans = max(ans, tmp);
      tmp -= nums[start++];
    }
  }
  return ans;  
}

```

### Smallest Subarray with a given sum
```cpp
// easy
/*
Given an array of positive numbers and a positive number ‘S’, 
find the length of the smallest contiguous subarray whose sum is greater than or equal to ‘S’. 
Return 0, if no such subarray exists.
*/
int findMinSubArray(vector<int>& nums, int val){
  int len = nums.size(), ans = len + 1;
  int start = 0, end = 0, tmp_sum = 0;
  while(end < len){
    tmp_sum += nums[end++];
    while(tmp_sum >= val){
      ans = min(ans, end - start);
      tmp_sum -= nums[start++];
    }
  }
  return ans > len ? 0 : ans;
}
```

### Longest Substring with K Distinct Characters
```cpp
// medium
/*
  Given a string, find the length of the longest substring in it with no more than K distinct characters
  
  araaic, 2
*/
int findLength(const string& str, int k){
  if(str.empty())return 0;
  if(k < 1)return 0;  
  unordered_map<char, int> cnts;
  int len = str.size();
  int start = 0, end = 0, ans = 0, cnt = 0;
  while(end < len){
    if(++cnts[str[end++]] == 1){
      ++cnt;
    }
    while(cnt > k){
      if(--cnts[str[start++]] == 0){
        --cnt;
      }
    }
    ans = max(ans, end - start);
  }
  return ans;
}
```






















