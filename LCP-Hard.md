## 11.3
### 4

### 10

### 23

### 25

## 11.4
### 30
```cpp
// 串联所有单词的子串
// time: O(n*m), space: O(m)
void CheckIndexValid(const string &s, int i,
                     unordered_map<string, int> word_cnt, vector<int> &ans,
                     int word_len, int word_len_sum) {
  int start = i, end = i, count = word_cnt.size();
  while (start + word_len_sum <= s.size()) {
    if (end - start == word_len_sum) {
      if (count == 0) {
        ans.push_back(start);
      }
      string st = s.substr(start, word_len);
      start += word_len;
      if (word_cnt.count(st) && ++word_cnt[st] == 1) {
        count++;
      }
    } else {
      while (end - start < word_len_sum) {
        string tmp = s.substr(end, word_len);
        end += word_len;
        if (word_cnt.count(tmp) == 0) {
          while (start != end) {
            string st = s.substr(start, word_len);
            start += word_len;
            if (word_cnt.count(st) && ++word_cnt[st] == 1) {
              ++count;
            }
          }
          break;
        } else {
          while (word_cnt[tmp] == 0) {
            string st = s.substr(start, word_len);
            start += word_len;
            if (word_cnt.count(st) && ++word_cnt[st] == 1) {
              ++count;
            }
            if (st == tmp) {
              break;
            }
          }
          if (--word_cnt[tmp] == 0) {
            --count;
          }
        }
      }
    }
  }
}

vector<int> findSubstring(string s, vector<string> &words) {
  unordered_map<string, int> word_cnt;
  int word_len = 0, word_len_sum = 0;
  vector<int> ans;
  for (auto &word : words) {
    word_cnt[word]++;
    word_len = word.size();
    word_len_sum += word_len;
  }
  for (int i = 0; i < word_len; ++i) {
    CheckIndexValid(s, i, word_cnt, ans, word_len, word_len_sum);
  }
  return ans;
}
```
### 32
```cpp
// 最长有效括号
// time: O(n), space: O(n)
int lc_32_longestValidParentheses(string s) {
  int ans = 0, len = s.size();
  vector<int> dp(len, 0);
  for (int i = 1; i < len; ++i) {
    if (s[i] == ')') {
      int last = i - 1 - dp[i - 1];
      if (last >= 0 && s[last] == '(') {
        dp[i] = 2 + dp[i - 1] + (last - 1 >= 0 ? dp[last - 1] : 0);
      }
      ans = max(ans, dp[i]);
    }
  }
  return ans;
}
```

### 37
```cpp
// 解数独
// time: 9^n space: O(1)
bool rowValid(vector<vector<char>> &board, int i, int j) {
  for (int k = 0; k < 9; ++k) {
    if (k != j && board[i][k] == board[i][j]) {
      return false;
    }
  }
  return true;
}

bool colValid(vector<vector<char>> &board, int i, int j) {
  for (int k = 0; k < 9; ++k) {
    if (k != i && board[k][j] == board[i][j]) {
      return false;
    }
  }
  return true;
}

bool boxValid(vector<vector<char>> &board, int i, int j) {
  int si = 3 * (i / 3), sj = 3 * (j / 3);
  for (int r = si; r < si + 3; ++r) {
    for (int c = sj; c < sj + 3; ++c) {
      if (r != i && c != j && board[r][c] == board[i][j]) {
        return false;
      }
    }
  }
  return true;
}

std::pair<int, int> solveSudokuNext(vector<vector<char>> &board, int i, int j) {
  std::pair<int, int> ans{0, 0};
  for (int r = i; r < 9; ++r) {
    for (int c = 0; c < 9; ++c) {
      if (board[r][c] == '.') {
        ans.first = r;
        ans.second = c;
        return ans;
      }
    }
  }
  return ans;
}

bool solveSudokuImpl(vector<vector<char>> &board, int i, int j) {
  for (char c = '1'; c <= '9'; ++c) {
    board[i][j] = c;
    if (rowValid(board, i, j) && colValid(board, i, j) &&
        boxValid(board, i, j)) {
      auto next = solveSudokuNext(board, i, j);
      if ((next.first == 0 && next.second == 0) ||
          solveSudokuImpl(board, next.first, next.second)) {
        return true;
      }
    }
  }
  board[i][j] = '.';
  return false;
}

void lc_37_solveSudoku(vector<vector<char>> &board) {
  // 9 x 9
  for (int i = 0; i < 9; ++i) {
    for (int j = 0; j < 9; ++j) {
      if (board[i][j] == '.') {
        solveSudokuImpl(board, i, j);
        break;
      }
    }
  }
}
```
### 41
```cpp
// 第一个缺失的正数
// time: O(n), space: O(1)
int lc_41_firstMissingPositive(vector<int> &nums) {
  int len = nums.size();
  for (int i = 0; i < len;) {
    if (nums[i] != i + 1 && nums[i] >= 1 && nums[i] <= len &&
        nums[nums[i] - 1] != nums[i]) {
      swap(nums[i], nums[nums[i] - 1]);
    } else {
      ++i;
    }
  }
  int ans = len + 1;
  for (int i = 0; i < len; ++i) {
    if (nums[i] != i + 1) {
      ans = i + 1;
      break;
    }
  }
  return ans;
}
```

### 44
```cpp
// 通配符匹配：？any single char，* any string
// time: O(n*m) space: O(n*m)
bool lc_44_isMatch(string s, string p) {
  if (p.empty())
    return s.empty();
  int ls = s.size(), lp = p.size();
  vector<vector<int>> dp(ls + 1, vector<int>(lp + 1, 0));
  dp[0][0] = true;
  for (int i = 0; i < lp; ++i) {
    if (p[i] == '*') {
      dp[0][i + 1] = dp[0][i];
    } else {
      break;
    }
  }

  for (int i = 0; i < ls; ++i) {
    for (int j = 0; j < lp; ++j) {
      if (s[i] == p[j] || '?' == p[j]) {
        dp[i + 1][j + 1] = dp[i][j];
      } else {
        if ('*' == p[j]) {
          dp[i + 1][j + 1] = dp[i + 1][j] || dp[i][j] || dp[i][j + 1];
        }
      }
    }
  }
  return dp[ls][lp];
}
```
### 51
```cpp
// N皇后
// time: O(n^n) space: O(n)
int solveNQueensIndex(const string &s) {
  int ans = -1;
  for (int i = 0; i < s.size(); ++i) {
    if (s[i] == 'Q') {
      ans = i;
      break;
    }
  }
  return ans;
}

bool solveNQueensValid(vector<string> &loc, int i) {
  int li = solveNQueensIndex(loc[i]);
  for (int s = 0; s < i; ++s) {
    int ls = solveNQueensIndex(loc[s]);
    if (ls == li || (abs(ls - li) == abs(i - s))) {
      return false;
    }
  }
  return true;
}

void solveNQueensImpl(vector<string> &loc, int i, vector<vector<string>> &ans) {
  if (i >= loc.size()) {
    ans.push_back(loc);
    return;
  }
  for (int l = 0; l < loc[i].size(); ++l) {
    loc[i][l] = 'Q';
    if (solveNQueensValid(loc, i)) {
      solveNQueensImpl(loc, i + 1, ans);
    }
    loc[i][l] = '.';
  }
}

vector<vector<string>> lc_51_solveNQueens(int n) {
  vector<vector<string>> ans;
  vector<string> loc(n, string(n, '.'));
  solveNQueensImpl(loc, 0, ans);
  return ans;
}
```
