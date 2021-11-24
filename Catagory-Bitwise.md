## 78. 子集
```cpp
vector<vector<int>> subsets(vector<int> &nums){
  vector<vector<int>> ans{vector<int>()};
  for(auto n: nums){
    int len = ans.size();
    for(int i = 0;i<len;++i){
      vector<int> temp = ans[i];
      temp.push_back(n);
      ans.push_back(temp);
    }
  }
  return ans;
}

```

## 89. 格雷编码
```cpp
// 2: [0,1] => [00, 01] + [11, 10] == [00, 01, 11, 10] == [0, 1, 3, 2]
// 3: => [000, 001, 011, 010] + [110, 111, 101, 100] == [0, 1, 3, 2, 6, 7, 5, 4]

vector<int> grayCode(int n) {
  vector<int> ans{0, 1};
  for(int i = 2, val = 2;i<=n;++i){
     int len = ans.size();
     for(int j = len - 1;j>=0;--j){
      ans.push_back(ans[j] + val);
     }
     val *= 2;
  }
  return ans;
}



```


## 90. 子集II
```cpp
vector<vector<int>> subsetsWithDup(vector<int> &nums) {
  vector<vector<int>> ans{vector<int>()};
  unordered_map<int, int> last_index;
  sort(nums.begin(), nums.end());
  for(auto n: nums){
    int start_index = last_index[n];
    int end_index = ans.size();
    last_index[n] = end_index;
    while(start_index < end_index){
      auto temp = ans[start_index];
      temp.push_back(n);
      ans.push_back(temp);
      ++start_index;
    }
  }
  return ans;
}

```


## 137. 只出现一次的数字
```cpp
int singleNumber(vector<int> &nums){
  int len = sizeof(int) * 8;
  vector<int> bit_times(len, 0);
  for(auto n: nums){
    for(int i = 0;i<len;++i){
      int val = 1 << i;
      if((val & n) != 0){
        bit_times[i]++;
      }
    }
  }
  int ans = 0;
  for(int i = 0;i<len;++i){
    if(bit_times[i] % 3 != 0){
      ans += (1 << i);
    }
  }
  return ans;
}

```

## 187. 重复的DNA序列


## 201. 数字范围按位与


## 260. 只出现一次的数字III


## 287. 寻找重复数
```cpp
int findDuplicate(vector<int> &nums){
  int ans = -1;
  for(int i = 0;i<nums.size();){
    int index_1 = i, index_2 = nums[i] - 1;
    if(nums[index_1] != index_1 + 1){
      // do swap or not
      if(nums[index_2] != index_2 + 1){
        int temp = nums[index_1];
        nums[index_1] = nums[index_2];
        nums[index_2] = temp;
      }
      else{
        ans = nums[i];
        break;
      }      
    }
    else{
      ++i;
    }  
  }
  return ans;
}




```

## 318. 最大单次长度乘积


```cpp

bool compare(vector<string> &words, int i, int j){
  int ch[26] = {0};
  for(auto c: words[i]){
    ch[(int)(c - 'a')]++;
  }
  for(auto c: words[j]){
    if(ch[(int)(c - 'a')] > 0){
      return false;
    }
  }
  return true;
}

int maxProduct(vector<string> &words){
  // method2
  {
    unordered_map<int, int> hash_count;
    int ans = 0;
    for(auto word: words){
      int mask = 0;
      for(auto c: word){
        mask |= (1 << (c - 'a'));
      }
      hash_count[mask] = max(hash_count[mask], (int)word.size());
      for(auto &node: hash_count){
        if(0 == (node.first & mask)){
          ans = max(ans, node.second * hash_count[mask]);
        }
      }    
    }
    return ans;  
  }


  // beat 5%
  int ans = 0;
  for(int i = 0;i<words.size();++i){
    for(int j = 0;j<words.size();++j){
      if(compare(words, i, j)){
        ans = max(ans, words[i].size() * words[j].size());
      }
    }    
  }
  return ans;
}


```


## 371. 两整数之和，不使用+和-


## 393. utf-8编码验证


## 397. 整数替换




## 421. 数组中两个数的最大异或值


































