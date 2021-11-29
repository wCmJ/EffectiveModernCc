## 355. Twitter



## 380. O(1) to insert, remove, getRandom
```cpp
class RandomizedSet{
public:
  RandomizedSet(): nums_(2 * 1e5), cnt_(0){}
  
  bool insert(int val){
    if(val_index_.count(val)){
      return false;
    }
    nums_[cnt_++] = val;
    val_index_[val] = cnt_ - 1;
    return true;
  }
  
  bool remove(int val){
    if(val_index_.count(val) == 0){
      return false;
    }
    int index = val_index_[val];
    if(index != cnt_ - 1){
      nums_[index] = nums_[cnt_ - 1];
      val_index_[nums_[index]] = index;
    }
    val_index_.erase(val);
    --cnt_;
    return true;
  }
  
  int getRandom(){
    return nums_[rand() % cnt_];
  }

private:
  unordered_map<int, int> val_index_;
  vector<int> nums_;
  int cnt_;
};
```

## 535. TinyUrl
```cpp
// how to design a hash function
/*
  1. use hashcode: s[0] * 7^(n-1) + s[1] * 7^(n-2) + ... + s[n]
  2. use random number
*/

```



## 622. 设计循环队列
```cpp
class CircularQueue{
public:
  CircularQueue(int k);
  
  bool enQueue(int val);
  
  bool deQueue();
  
  int Front();
  
  int Rear();
  
  bool isEmpty();
  
  bool isFull();
};
```



















