### 685
```cpp
// 删除一条边，使得树满足以下条件
// 1. 只有一个根节点
// 2. 除了根节点外其余节点只有一个父节点，根节点没有父节点
//
int getParent(vector<int> &us, int index){
  while(index != us[index]){
    index = us[index];
  }
  return index;
}

vector<int> getCycle(vector<vector<int>>& edges, int parent, int child){
  int n = edges.size();
  vector<int> us(n + 1, 0);
  for(int i = 1;i<=n;++i){
    us[i] = i;
  }
  vector<int> ans(2, 0);
  for(auto &edge: edges){
    if(edge[0] == parent && edge[1] == child){
      continue;
    }
    int p0 = getParent(us, edge[0]);
    int p1 = getParent(us, edge[1]);
    if(p0 == p1){
      ans[0] = edge[0];
      ans[1] = edge[1];
      break;
    }
    else{
      us[edge[1]] = edge[0];
    }
  }
  return ans;
}


vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
  int n = edges.size();
  vector<vector<int>> parents;
  unordered_set<int> multiparents;
  
  for(auto &edge: edges){
    parents[edge[1]].push_back(edge[0]);
    if(parents[edge[1]].size() > 1){
      multiparents.insert(edge[1]);
    }
  }
  
  if(multiparents.empty()){
    return getCycle(edges, 0, 0);
  }
  vector<int> ans(2, 0);
  for(auto child: multiparents){
    for(int i = parents[child].size() - 1; i >= 0; --i){
      auto res = getCycle(edges, parents[child][i], child);
      if(res[0] == 0 && res[1] == 0){
        return vector<int>{parents[child][i], child};
      }
    }
  }
  return ans;
}
```
- 使用并查集判断是否成环
- 对于父节点个数超过1个的节点，从后往前删除，判断删除单个节点后剩余节点是否成环

















