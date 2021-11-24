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

























