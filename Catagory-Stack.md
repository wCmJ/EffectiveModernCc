## 71. 简化路径
```cpp
// 给定一个字符串path，表示指向某一文件或目录的Unix风格绝对路径（以'/'开头），转化为更加简洁的规范路径
/*
  .表示当前目录本身
  ..表示将目录切换到上一级
  //被视为单个/
  ...等被视为文件/目录名称

*/

string simplifyPath(string path) {
  int len = path.size();
  stack<string> files_and_dirs;
  for(int i = 0;i<len;){
    int start = i + 1;
    while(start < len && path[start] != '/'){
      ++start;
    }
    string temp = path.substr(i + 1, start - i - 1);
    if(temp.empty()){
      // donothing
    }
    else if(temp == "."){
      // donothing
    }
    else if(temp == ".."){
      if(!files_and_dirs.empty()){
        files_and_dirs.pop();
      }
    }
    else{
      files_and_dirs.push(temp);
    }
    i = start;
  }
  string ans;
  while(!files_and_dirs.empty()){
    ans = "/" + files_and_dirs.top() + ans;
    files_and_dirs.pop();
  }
  return ans.empty() ? "/" : ans;
}

```


## 114. 将二叉树展开为链表
```cpp
// 以先序遍历顺序将二叉树展开为单链表，左子节点为空，右子节点指向链表下一个节点

struct TreeNode {
  int val;
  TreeNode *left;
  TreeNode *right;
  TreeNode() : val(0), left(nullptr), right(nullptr) {}
  TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
  TreeNode(int x, TreeNode *l, TreeNode *r) : val(x), left(l), right(r) {}
};

void flatten(TreeNode *root) {
    // use stack to travser Tree
    if(root == nullptr)return;
    TreeNode *temp = root, *last = nullptr;
    stack<TreeNode*> nodes;
    while(true){
      while(temp){
        TreeNode *left = temp->left, *right = temp->right;
        if(last != nullptr){
          last->left = nullptr;
          last->right = temp;
        }
        last = temp;
        temp = left;
        if(right){
          nodes.push(right);
        }      
      }
      if(nodes.empty())break;
      temp = nodes.top();
      nodes.pop();
    }    
}




```

























