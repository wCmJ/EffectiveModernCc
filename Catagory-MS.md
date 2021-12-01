## 146. LRU Cache
```cpp
class LRUCache {
public:
  LRUCache(int capacity) : capacity_(capacity){}
  
  int get(int key){
    if(key_it_value_.count(key) == 0){
      return -1;
    }
    
    auto it = key_it_value_[key].first;
    keys_.erase(it);
    keys_.push_front(key);
    key_it_value_[key].first = keys_.begin();
    return key_it_value_[key].second;
  }
  
  void put(int key, int value){
    // if capacity_ is 0
    if(capacity_ <= 0){
      return;
    }
    if(get(key) != -1){
      key_it_value_[key].second = value;
      return;
    }
    if(key_it_value_.size() == capacity_){
      // erase one
      int delete_key = keys_.back();
      keys_.pop_back();
      key_it_value_.erase(delete_key);          
    }
    
    // insert
    keys_.push_front(key);
    key_it_value_[key].first = keys_.begin();
    key_it_value_[key].second = value;  
  }
private:
  list<int> keys_;
  unordered_map<int, pair<list<int>::iterator, int>> key_it_value_;
  int capacity_;
};
```

## 56. Merge intervals
```cpp
vector<vector<int>> merge(vector<vector<int>>& intervals) {
  // sort
  sort(intervals.begin(), intervals.end(), [](vector<int> &v1, vector<int> &v2){
    return v1[0] < v2[0];
  });
  
  vector<vector<int>> ans{intervals[0]};
  for(int i = 1;i<intervals.size();++i){
    if(intervals[i][0] > ans.back()[1]){
      ans.push_back(intervals[i]);
    }
    else{
      ans.back()[1] = max(ans.back()[1], intervals[i][1]);    
    }  
  }
  return ans;
}

```

## 200. Number of islands
```cpp
// DFS
```


## 273. Integer to English Words
```cpp
string numberToWords(int num) {
  // hundred, thousand, million, billion
  // 
  unordered_map<int, vector<string>> number_eng;
  number_eng[1].push_back("One");
  number_eng[1].push_back("Ten");
  number_eng[2].push_back("Two");
  number_eng[2].push_back("Twenty");
  number_eng[3].push_back("Three");
  number_eng[3].push_back("Thirty");
  number_eng[4].push_back("Four");
  number_eng[4].push_back("Forty");
  number_eng[5].push_back("Five");
  number_eng[5].push_back("Fifty");
  number_eng[6].push_back("Six");
  number_eng[6].push_back("Sixty");
  number_eng[7].push_back("Seven");
  number_eng[7].push_back("Seventy");
  number_eng[8].push_back("Eight");
  number_eng[8].push_back("Eighty");
  number_eng[9].push_back("Nine");
  number_eng[9].push_back("Ninty");
  number_eng[10].push_back("Ten");
  number_eng[11].push_back("Eleven");
  number_eng[12].push_back("Twelve");
  
  number_eng[13].push_back("Thirteen");
  number_eng[14].push_back("Forteen");
  number_eng[15].push_back("Fifteen");
  number_eng[16].push_back("Sixteen");
  
  number_eng[17].push_back("Seventeen");
  number_eng[18].push_back("Eighteen");
  number_eng[19].push_back("Nineteen");
  
  string ans;
  if(num == 0){
    return "Zero";
  }
  // process three numbers seperately
  return ans;
};
```


## 295. Find Median from Data Stream
```cpp
class MedianFinder {
public:
  MedianFinder()


  void addNum(int num) {}
  
  double findMedian() {}

};
```

## 224. Basic Calculator
```cpp








```













