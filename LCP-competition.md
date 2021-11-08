## 11.8
### 2062
```cpp
// 子字符串的个数
/*
  aeiouu
  aeiou
  aeiouu

  example2: cu aieuo uac
  from u：uaieuo u a
  from a: aieuo u a
  from i: ieuoua c

  2 * 3 = 6
*/

int lt_2062_cal(const string &word, int index) {
  unordered_map<char, int> cnts{
      {'a', 1}, {'e', 1}, {'i', 1}, {'o', 1}, {'u', 1},
  };
  int cnt = 5, i = index;
  for (i = index; i < word.size(); ++i) {
    if (word[i] == 'a' || word[i] == 'e' || word[i] == 'i' || word[i] == 'o' ||
        word[i] == 'u') {
      if (--cnts[word[i]] == 0) {
        --cnt;
      }
    } else {
      break;
    }
    if (cnt == 0) {
      break;
    }
  }
  if (cnt != 0) {
    return 0;
  }
  for (index = i; index < word.size();) {
    if (word[index] == 'a' || word[index] == 'e' || word[index] == 'i' ||
        word[index] == 'o' || word[index] == 'u') {
      ++index;
    } else {
      break;
    }
  }
  // std::cout << "index: " << index << ",i: " << i << std::endl;
  return index - i;
}

int lt_2062_countVowelSubstrings(string word) {
  int ans = 0;
  //
  for (int i = 0; i < word.size(); ++i) {
    if (word[i] == 'a' || word[i] == 'e' || word[i] == 'i' || word[i] == 'o' ||
        word[i] == 'u') {
      ans += lt_2062_cal(word, i);
    }
  }
  return ans;
}
```

### 2064
```cpp
// 分配给商店的最多商品的最小值
// 使用二分法
bool lt_2064_valid(int val, int n, vector<int> &quantities) {
  int count = 0;
  for (auto q : quantities) {
    count += (q - 1) / val + 1;
  }
  return count <= n;
}

int lt_2064_minimizedMaximum(int n, vector<int> &quantities) {
  int m = quantities.size(), max_val = quantities[0];
  for (auto q : quantities) {
    max_val = std::max(max_val, q);
  }
  if (m == n)
    return max_val;
  int start = 1, end = max_val, ans = 0;
  while (start <= end) {
    int mid = start + ((end - start) >> 1);
    if (lt_2064_valid(mid, n, quantities)) {
      end = mid - 1;
      ans = mid;
    } else {
      start = mid + 1;
    }
  }
  return ans;
}



```



