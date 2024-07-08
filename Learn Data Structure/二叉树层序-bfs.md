```C++
想用二维数组，就要定义一维数组，每次将一个一维数组填满，放入目标二维数组----这意味着需要计算出一个答案，放回二维数组，然后因为某些条件没达成，于是再定义一个一维数组，再放元素，这样才能做到：回答类似----有多种不同路径的问题 --- 输出每一条路径

class Solution {

public:

    vector<vector<int>> levelOrderBottom(TreeNode* root) {
 queue<TreeNode*> que;
        if (root != NULL) que.push(root);
        vector<vector<int>> result;
        while (!que.empty()) 
            int size = que.size();
            vector<int> vec;
            // 这里一定要使用固定大小size，不要使用que.size()，因为que.size是不断变化的

            for (int i = 0; i < size; i++) {
                TreeNode* node = que.front();
                que.pop();
                vec.push_back(node->val);
                if (node->left) que.push(node->left);
                if (node->right) que.push(node->right);
            }
            result.push_back(vec);
        }
        reverse(result.rbegin(), result.rend());
        return result;
    }
};
```

~~~ C++
不是解释刚才一段，是解释这一段
class Solution {
public:
    vector<vector<int>> levelOrderBottom(TreeNode* root) {
    vector<vector<int>> res;
if(! root) return res;
queue<TreeNode*> qu;
qu.push(root);
while(!qu.empty()){
  int size=qu.size();
  vector <int> vec;
for(int i=0; i<size; i++){
TreeNode* node = qu.front();
qu.pop();
vec.push_back(node->val);
if(root->left) qu.push(root->left);
if(root->right) qu.push(root->right);
}
res.push_back(vec);

}
reverse(res.rbegin(), res.rend());
return res;
    }
};

层序----二叉树节点数
class Solution {

public:

    int countNodes(TreeNode* root) {

        if(! root) return 0;

        queue<TreeNode*> qu;

        qu.push(root);

        int res=0;

        while(!qu.empty()){

        int size=qu.size();

            for(int i=0;i<size;i++){

                TreeNode* node=qu.front();

                qu.pop();

                if(node->left) qu.push(node->left);

                if(node->right) qu.push(node->right);

                res++;

            }

        }

        return res;

  

    }

};
