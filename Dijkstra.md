```cpp
// 获取图中两点之间的最小路径
/*
  Dijkstra算法思路：
    辅助数据结构：记录节点是否被处理的表visited，记录节点的父节点的表parent，记录节点与起点的距离的表distance
    1. 初始化：更新parent[start] = start，更新distance[start]=0
    2. 从start节点开始，更新表：
      2.1 遍历与start直接相连的节点{n1, n2, ...}，更新distance[ni]=min(distance[ni], distance[i] + weights[start][i]), 如果distance更新了，需要同步更新parent[i] = index
    3. 获取下一个distance最小的节点，以它为start，从第1步开始，直到end节点   
 */

void updateDijkstra(vector<vector<int>> &weights, int index, vector<int> &distance, vector<int> &visited, vector<int> &parent){
  for(int i = 0;i < weights[index].size(); ++i){
    if(weights[index][i] > 0 
    && !visited[i]
    && (distance[i] == INT_MAX || (distance[index] + weights[index][i] < distance[i]))){
        distance[i] = distance[index] + weights[index][i];
        parent[i] = index;
    }
  }
}

int getMinDistance(vector<int> &visited, vector<int> &distance){
  int ans = -1, min_val = INT_MAX;
  for(int i = 0;i<visited.size();++i){
    if(!visited[i] && distance[i] < min_val){
      ans = i;
      min_val = distance[i];
    }
  }
  return ans;
}


vector<int> Dijkstra(vector<vector<int>> &weights, int start, int end){
  int len = weights.size();
  vector<int> parent(len, -1);
  parent[start] = start;
  vector<int> distance(len, INT_MAX);
  distance[start] = 0;
  vector<int> visited(len, false);
  while(start != end){
    visited[start] = true;
    updateDijkstra(weights, start, distance, visited, parent);
    start = getMinDistance(visited, distance);
  }
  vector<int> ans;
  while(end != parent[end]){
    std::cout<<end<<" ";
    ans.push_back(end);
    end = parent[end];
  }
  std::cout<<end<<std::endl;
  ans.push_back(start);
  return ans;
}






```
