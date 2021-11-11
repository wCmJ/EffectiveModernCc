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

## 11.9
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

### Problem Challenge 2
```cpp
// Given a string and a pattern, find all anagrams of the pattern in the given string
/*
ppqp, pq => [1, 2]
abbcabc, abc => [2, 3, 4]
*/

vector<int> stringAnagrams(const string &str, const string &pattern){
  vector<int> ans;
  int ls = str.size(), lp = pattern.size();
  if(lp == 0 || ls == 0 || lp > ls)return ans;
  unordered_map<char, int> cnts;
  for(auto p: pattern){
    cnts[p]++;
  }
  
  int start = 0, end = 0, cnt = cnts.size();
  while(end < ls){
    if(cnts.count(str[end]) && --cnts[str[end]] == 0){
      --cnt;
    }
    ++end;
    if(end - start == lp && cnt == 0){
      ans.push_back(start);
    }
    if(end >= lp){
      if(cnts.count(str[start]) && ++cnts[str[start]] == 1){
        ++cnt;
      }
      ++start;
    }    
  }
  return ans;  
}
```

### Problem Challenge 3
```cpp
/*
Smallest Window containing Substring
aabdec, abc => abdec
abdabca, abc => abc
adcad, abc => ""
*/

string minimumWindowSubstring(const string& str, const string& pattern){
  int ls = str.size(), lp = pattern.size();
  if(lp > ls || lp == 0 || ls == 0)return "";
  unordered_map<char, int> cnts;
  for(auto p: pattern){
    cnts[p]++;
  }
  int start = 0, end = 0, cnt = cnts.size();
  int ans_start = -1, ans_len = -1;
  while(end < ls){
    if(cnts.count(str[end]) && --cnts[str[end]] == 0){
      --cnt;
    }
    ++end;
    while(cnt == 0){
      if(ans_start == -1 || ans_len > (end - start)){
        ans_start = start;
        ans_len = end - start;
      }
      if(cnts.count(str[start]) && ++cnts[str[start]] == 1){
        ++cnt;
      }
      ++start;
    }    
  }
  return ans_start == -1 ? "" : str.substr(ans_start, ans_len);  
}


```
### Problem Challenge 4
```cpp
// Words Concatenation
/*
Given
catfoxcat, [cat, fox] => [0, 3]
catcatfoxfox, [cat, fox] => [3]
*/

vector<int> wordConcatenation(const string& str, vecor<string> &words){
  // not empty
  int ls = str.size(), lw = words.size(), lsw = words[0].size(), total_len = lsw * lw;
  unordered_map<string, int> cnts;
  for(auto &w: words){
    cnts[w]++;
  }
  vector<int> ans;
  for(int i = 0;i<lsw;++i){
    int start = i, end = i, cnt = cnts.size();
    unordered_map<string, int> tmp_cnt = cnts;
    while(end + lsw <= ls){
      string tmp = str.substr(end, lsw);
      if(tmp_cnt.count(tmp) && --tmp_cnt[tmp] == 0){
        --cnt;
      }
      end += lsw;
      if(end - start == total_len && cnt == 0){
        ans.push_back(start);
      }
      if(end - i >= total_len){
        string tmp_start = str.substr(start, lsw);
        if(tmp_cnt.count(tmp_start) && ++tmp_cnt[tmp_start] == 1){
          ++cnt;
        }
        start += lsw;
      }
    }
  }  
  return ans;
}



```

### Pair with Target Sum
```cpp
/*
1,2,3,4,6  target = 6 => [1,3]
2,5,9,11 target = 11 => [0,2]

*/

vector<int> pairWithTargetSum(vector<int> &nums, int target){
  int len = nums.size();
  if(len < 2)return vector<int>();
  vector<int> ans(2, 0);
  int start = 0, end = len - 1;
  while(start < end){
    int val = nums[start] + nums[end];
    if(val == target){
      ans[0] = start;
      ans[1] = end;
      break;
    }
    else if(val < target){
      ++start;
    }
    else{
      --end;
    }  
  }
  return ans;  
}

```

### Remove Duplicates
```cpp
int removeDuplicates(vector<int> &nums){
  int len = nums.size();
  int last = -1, start = 0;
  for(int start = 0;start < len;){
    int end = start;
    while(end < len && nums[end] == nums[start]){
      ++end;
    }
    nums[++last] = nums[start];
    start = end;
  }
  return last + 1;  
}


```

### Squaring a Sorted Array
```cpp
vector<int> sortedArraySquares(vector<int>& nums){
  int len = nums.size();
  vector<int> ans(len, 0);
  int index = len - 1;
  int start = 0, end = len - 1;
  while(index >= 0){
    if(start > end){
      break;
    }
    if(nums[start] * nums[start] >= nums[end] * nums[end]){
      ans[index--] = nums[start] * nums[start];
      ++start;
    }
    else{
      ans[index--] = nums[end] * nums[end];
      --end;
    }    
  }
  return ans;
}

```

### Triplet Sum to Zero
```cpp
/*
find all unique triplets in it that add up to zero
-3, 0, 1, 2, -1, 1, -2 => [-3, 1, 2], [-2, 0, 2], [-2, 1, 1], [-1, 0, 1]
-5, 2, -1, -2, 3 => [-5, 2, 3], [-2, -1, 3]
*/

vector<vector<int>> tripletSumToZero(vector<int> &nums){
  int len = nums.size();
  vector<vector<int>> ans;
  if(len < 3)return ans;
  sort(nums.begin(), nums.end());
  for(int i = 0;i<= len - 3;){
    int val = 0 - nums[i];
    int start = i + 1, end = len - 1;
    while(start < end){
      int tv = nums[start] + nums[end];
      if(tv == val){
        ans.push_back(vector<int>{nums[i], nums[start], nums[end]});
        int bs = start, be = end;
        while(start < end && nums[start] == nums[bs]){
          ++start;
        }
        while(start < end && nums[end] == nums[be]){
          --end;
        }
      }
      else if(tv < val){
        ++start;
      }
      else{
        --end;
      }      
    }  
    int v = nums[i];
    while(i <= len - 3 && nums[i] == v){
      ++i;
    }
  }
  return ans;
}

```

### 四数之和
```cpp
vector<vector<int>> quadrupleSumToTarget(vector<int> &nums, int target) {
  vector<vector<int>> ans;
  int len = nums.size();
  if (len < 4)
    return ans;
  sort(nums.begin(), nums.end());
  for (int i = 0; i <= len - 4;) {
    if (nums[i] + nums[i + 1] + nums[i + 2] + nums[i + 3] > target) {
      break;
    }
    if (nums[i] + nums[len - 3] + nums[len - 2] + nums[len - 1] < target) {
      // do nothing
    } else {
      int val = target - nums[i];
      for (int j = i + 1; j <= len - 3;) {
        int tmp_val = val - nums[j];
        int start = j + 1, end = len - 1;
        while (start < end) {
          int cur_val = nums[start] + nums[end];
          if (cur_val == tmp_val) {
            ans.push_back(
                vector<int>{nums[i], nums[j], nums[start], nums[end]});
            int bs = start, be = end;
            while (start < end && nums[start] == nums[bs]) {
              ++start;
            }
            while (start < end && nums[end] == nums[be]) {
              --end;
            }
          } else if (cur_val < tmp_val) {
            ++start;
          } else {
            --end;
          }
        }
        int bj = j;
        while (j <= len - 3 && nums[bj] == nums[j]) {
          ++j;
        }
      }
    }
    // skip duplicates
    int base = i;
    while (i <= len - 4 && nums[i] == nums[base]) {
      ++i;
    }
  }
  return ans;
}


```


## 11.10
### Problem Challenge 2
```cpp
/*
Comparing Strings containing Backspaces
Given two strings containing backspaces(identified by the character #), check if the two strins are equal

xy#z, xzz# => true
xy#z, zyz# => false
xp#, xyz## => true
xywrrmp, xywrrmu#p => true
*/

void getThrough(const string& s, string &ns){
  int backspace_cnt = 0, len = s.size();
  for(int i = len - 1;i >= 0;--i){
    if(s[i] == '#'){
      ++backspace_cnt;
    }
    else{
      if(backspace_cnt > 0){
        --backspace_cnt;
      }
      else{
        ns = s[i] + ns;
      }
    }
  }
}

bool backspaceCompare(const string& s1, const string& s2){
  string ans1, ans2;
  getThrough(s1, ans1);
  getThrough(s2, ans2);
  return ans1 == ans2;
}




```
### Problem Challenge 3
```cpp
/*
Minimum Window Sort
1, 2, 5, 3, 7, 10, 9, 12 => 5(5,3,7,10,9)
1, 3, 2, 0, -1, 7, 10 => 5(1, 3, 2, 0, -1)
1, 2, 3 => 0
3, 2, 1 => 3
*/

int shortestWindowSort(vector<int> &nums){
  int len = nums.size();
  if (len < 2)
    return 0;
  vector<int> copy = nums;
  sort(copy.begin(), copy.end());
  int start = 0, end = len - 1;
  while (start < len) {
    if (nums[start] != copy[start]) {
      break;
    }
    ++start;
  }
  while (-1 < end) {
    if (nums[end] != copy[end]) {
      break;
    }
    --end;
  }
  return end >= start ? end - start + 1 : 0;
}

// method2
/*
1. 从左往右找出第一个值index1，该值大于它后面第一个值
2. 从右往左找出第一个值index2，该值小于它前面第一个值
3. 找出index1和index2中的最大值max和最小值min
4. 在0~index1-1之间，找出第一个小于min的值，将index1更新为该下标
5. 在len-1~index2 + 1之间，找出第一个大于max的值，将index2更新为该下标
*/

void getInvalidIndex(vector<int> &nums, int &start, int &end) {
  int len = nums.size();
  while (start + 1 < len) {
    if (nums[start] > nums[start + 1]) {
      break;
    }
    ++start;
  }
  // last invalid
  while (end - 1 >= 0) {
    if (nums[end] < nums[end - 1]) {
      break;
    }
    --end;
  }
}

void updateInvalidIndex(vector<int> &nums, int &start, int &end) {
  // find min and max in start and end
  int len = nums.size();
  int min_val = nums[start], max_val = nums[end];
  for (int i = start; i <= end; ++i) {
    min_val = min(nums[i], min_val);
    max_val = max(nums[i], max_val);
  }
  for (int i = 0; i < start; ++i) {
    if (nums[i] > min_val) {
      start = i;
      break;
    }
  }
  for (int i = len - 1; i > end; --i) {
    if (nums[i] < max_val) {
      end = i;
      break;
    }
  }
}
int shortestWindowSort(vector<int> &nums){
    int len = nums.size();
    if (len < 2)
      return 0;
    int start = 0, end = len - 1;
    // first invalid
    getInvalidIndex(nums, start, end);
    if (end <= start) {
      return 0;
    }
    updateInvalidIndex(nums, start, end);
    return end - start + 1;
}


```
#############################################################################
### LinkedList Cycle
```cpp
struct ListNode{
  int val;
  ListNode *next;
  ListNode(int v):val(v), next(nullptr){}
};

bool hasCycle(ListNode *head){
  if(head == nullptr || head->next == nullptr)return false;
  ListNode *slow = head, *fast = head;
  while(fast && fast->next){
    slow = slow->next;
    fast = fast->next->next;
    if(slow == fast){
      return true;
    }
  }
  return false;
}


```

### Cycle start
```cpp
ListNode* findCycleStart(ListNode *head) {
  auto slow = head, fast = head;
  while (fast && fast->next) {
    slow = slow->next;
    fast = fast->next->next;
    if (slow == fast) {
      break;
    }
  }
  auto ln1 = head, ln2 = slow;
  while (ln1 != ln2) {
    ln1 = ln1->next;
    ln2 = ln2->next;
  }
  return ln1;
}


```

### Cycle Array
```cpp
bool isCircularArrayCheck2(vector<int> &nums, int index) {
  bool direction = nums[index] > 0;
  int slow = index, fast = index;
  while (true) {
    slow = (slow + nums[slow]) % nums.size();
    if ((nums[slow] > 0) != direction) {
      return false;
    }
    fast = (fast + nums[fast]) % nums.size();
    if ((nums[fast] > 0) != direction) {
      return false;
    }
    fast = (fast + nums[fast]) % nums.size();
    if ((nums[fast] > 0) != direction) {
      return false;
    }
    if (slow == fast) {
      return true;
    }
  }
  return false;
}

bool isCircularArray(vector<int> &nums) {
  /*
  1, 2, -1, 2, 2
  2, 1, -1, -2
  */
  // method2
  {
    // cycle => slow & fast
    int len = nums.size();
    if (len < 2)
      return false;
    for (int i = 0; i < len; ++i) {
      if (isCircularArrayCheck2(nums, i)) {
        return true;
      }
    }
    return false;
  }
}

```

## 11.11
### Minimum Meeting Rooms
```cpp
/*
最少使用多少个会议室，可以完成所有会议
{2,3},{2,4},{3,5},{4,5} => 2
{1,4},{2,3},{3,6} => 2
*/
struct lessEndTime{
  bool operator()(vector<int> &v1, vector<int> &v2){
    return v1[1] > v2[1];
  }
};


int minimumMeetingRooms(vector<vector<int>>& meetings){
  sort(meetings.begin(), meetings.end(), [](vector<int> &v1, vector<int> &v2){
    return v1[0] < v2[0]; // 需要按照start time排序，确保被弹出去的不会影响后续的计算
  });
  size_t min_rooms = 0;
  priority_queue<vector<int>, vector<vector<int>>, lessEndTime> min_heap;
  for(auto &meeting& meetings){
    while(!min_heap.empty() && meeting[0] >= min_heap.top()[1]){
      min_heap.pop();
    }
    min_heap.push(meeting);
    min_rooms = std::max(min_rooms, min_heap.size());
  }
  return min_rooms;
}


```

### Maximum CPU Load
```cpp
struct lessEndTime{
  bool operator()(vector<int> &v1, vector<int> &v2){
    return v1[1] > v2[1];
  }
};

int maximumCPULoad(vector<vector<int>> &loads){
  sort(loads.begin(), loads.end(), [](vector<int> &v1, vector<int> &v2){
    return v1[0] < v2[0];
  });
  priority_queue<vector<int>, vector<vector<int>>, lessEndTime> min_heap;
  int ans = 0, cur_load = 0;
  for(auto &load: loads){
    while(!min_heap.empty() && min_heap.top()[1] < load[0]){
      cur_load -= min_heap.top()[2];
      min_heap.pop();
    }
    cur_load += load[2];
    min_heap.push(load);
    ans = std::max(ans, cur_load);
  }
  return ans;  
}




```

### Employee Free Time
```cpp
struct LessStartTime{
  bool operator()(vector<int> &v1, vector<int> &v2){
    return (v1[0] > v2[0]) ? (true) : ();
  }
};

vector<vector<int>> employeeFreeTime(vector<vector<vector<int>>> &employees){


}






```
























