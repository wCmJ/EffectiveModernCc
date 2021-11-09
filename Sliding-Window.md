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

### Fruits into Baskets
```cpp
// medium
/*
Given an array of characters where each character represents a fruit tree, you are given two baskets and your goal is to put maximum number of fruits in each basket. The only restriction is that each basket can have only one type of fruit
ABCAC => 3
ABCBBC => 5
*/

int maxFruitCountOfTwoTypes(const string &str){  
  if(str.size() < 2)return str.size();
  unordered_map<char, int> cnts;
  int len = str.size(), ans = 0, cnt = 0;
  int start = 0, end = 0;
  while(end < len){
    if(++cnts[str[end++]] == 1){
      ++cnt;
    }
    while(cnt > 2){
      if(--cnts[str[start++]] == 0){
        --cnt;
      }
    }
    ans = max(ans, end - start);
  }
  return ans;
}

```

### No-repeat Substring
```cpp
// Given a string, find the length of the longest substring which has no repeating characters
/*
aabccbb => 3
abbbb => 2
abccde => 3
*/

int noRepeatSubstring(const string &str){
  if(str.size() < 2)return str.size();
  int len = str.size(), ans = 0, start = 0, end = 0, cnt = 0;
  unordered_map<char, int> cnts;
  while(end < len){
    if(++cnts[str[end++]] == 1){
      ++cnt;
    }
    while(cnt < (end - start)){
      if(--cnts[str[start++]] == 0){
        --cnt;
      }
    }
    ans = max(ans, cnt);
  }
  return ans;
}

```

### Longest Substring with Same Letters after Replacement 
```cpp
/*
Given a string with lowercase letters only, if you are allowed to replace no more than ‘k’ letters with any letter, find the length of the longest substring having the same letters after replacement
aabccbb, 2 => 5
abbcb, 1 => 4
abccde, 1 => 3

*/

int characterReplacement(const string &str, int k){
  if(str.empty())return 0;
  {
    int ans = 0, start = 0, len = str.size();
    for(;start < len;){
      int end = start, tk = k;
      while(end < len){
        if(str[end] != str[start] && --tk < 0){
          break;          
        }
        ++end;
      }
      ans = max(ans, end - start);
      while(start + 1 < len && str[start + 1] == str[start]){
        ++start;
      }
      ++start;
    }
    return ans;
  }
}


```

### Longest Subarray with Ones after Replacement
```cpp
/*
Given an array containing 0s and 1s, if you are allowed to replace no more than ‘k’ 0s with 1s, find the length of the longest contiguous subarray having all 1s
[0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1], k=2 => 6
[0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1], k=3 => 9
*/

int replacingOnes(vector<int> &nums, int k){
  if(nums.empty())return 0;
  int len = nums.size(), start = 0, end = 0, cnt = 0, ans = 0;
  while(end < len){
    if(nums[end++] == 0){
      ++cnt;
    }
    while(cnt > k){
      if(nums[start++] == 0){
        --cnt;
      }
    }
    ans = max(ans, end - start);
  }
  return ans;
}


```

### Problem Challenge 1
```cpp
// Given a string and a pattern, find out if the string contains any permutation of the pattern
/*
oidbcaf, abc => true
odicf, dc => false
bcdxabcdy, bcdyabcdx => true
aaacb, abc => true

*/
bool stringPermutation(const string& str, const string& pattern){
  if(pattern.empty())return true;
  if(str.empty())return pattern.empty();
  unordered_map<char, int> cnts;
  for(auto c: pattern){
    cnts[c]++;
  }
  int len = str.size(), start = 0, end = 0, cnt = cnts.size();
  while(end < len){
    if(cnts.count(str[end]) && --cnts[str[end]] == 0){
      --cnt;
    }
    ++end;
    if(end - start == pattern.size() && cnt == 0){
      return true;
    }
    if(end >= pattern.size()){
      if(cnts.count(str[start]) && ++cnts[str[start]] == 1){
        ++cnt;
      }
      ++start;
    }    
  }
  return false;  
}

```









































