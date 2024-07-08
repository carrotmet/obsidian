``` C++
二叉树对称

class Solution {

public:

    bool isSymmetric(TreeNode* root) {

        if(!root) return true;

        return isMirror(root->left,root->right);

    }

  

    bool isMirror(TreeNode* t1,TreeNode* t2){

        // 特殊情况1：左右都为空

        if(t1 == nullptr && t2 == nullptr) return true;

        //特殊情况2 ：只有一个节点为空

        if(t1 == nullptr || t2 == nullptr) return false;

        //比较自身节点的左左右右，返回给上一层

        return (t1->val == t2->val)&&isMirror(t1->left,t2->right)&&isMirror(t1->right,t2->left);

    }

};
```
```C++
翻转二叉树：先序+递归----递归三部曲：确定返回与异常，确定操作（向上），确定迭代参数（向下）
class Solution {

public:

    TreeNode* invertTree(TreeNode* root) {

        if(root == nullptr) return nullptr;

  

            TreeNode* help=root->left;

            root->left=root->right;

            root->right=help;

  

        invertTree(root->left);

        invertTree(root->right);

        return root;

  
  

    }

};

二叉树高度：错误版本---
int maxD(TreeNode* root, int dep){
    if(! root) return 0;
    dep = maxD(root->left, dep) > maxD(root->right, dep) ? maxD(root->left, dep) : maxD(root->right, dep);
    return dep + 1;
}


我这样理解：我的代码每次在函数体内计算dep,这个dep每次都要计算这棵树的所有高度，就好比如果一棵树高为5，我的代码会计算 5,4,3,2,1，最后返回5，而实际上，后面的 4,3,2,1是无效的，所以我的代码时间复杂度特别高。正确做法应该是直接在return中，用我的表达式替换dep，也就是：
int maxD(TreeNode* root){
    if(! root) return 0;
    return  maxD(root->left) > maxD(root->right) ? maxD(root->left) : maxD(root->right) + 1;
}
这样是对的


二叉树最小深度节点数：
class Solution {

public:

    int minDepth(TreeNode* root) {

        if(!root) return 0;

        if(!root->left) return minDepth(root->right)+1;

        if(!root->right) return minDepth(root->left)+1;

        int left=minDepth(root->left);

        int right=minDepth(root->right);

        return min(left,right)+1;

  

    }

};

平衡二叉树判断：
class Solution {

public:

int cout=0;

    bool isBalanced(TreeNode* root) {

        if(! root) return true;

        Bh(root);

        if(cout>0) return false;

        return true;

    }

    int Bh(TreeNode* root){

        if(! root) return 0;

        int lh=Bh(root->left);

        int rh=Bh(root->right);

        if(abs(lh-rh)>1) cout++;

        return 1+max(lh,rh);

    }

};


```

```C++
递归前中后序遍历
void traversal(TreeNode* cur ,vector<int> vec){
if (cur== nullptr) return;
vec.push_back(cur->val);
traversal(cur->left,vec);
traversal(cur->right,vec);
}


前序迭代---有右先进右，有左后进左
输出：根左右  压栈：右左根

vector<int> traver(TreeNode* cur, vector<int> res){
if(root == nullptr) return res;
stack<treeNode*> st;
st.push(cur);
while(!st.empty()){
TreeNode* node = st.top;
st.pop();
res.push_back(root.val);
if(node->right) st.push(node->right);
if(node->left) st.push(node->left);
}
return res;
}

后序  压栈：左右根-->输出：根右左-->逆置数组：左右根
引入 reverse()  翻转字符串、数组



应用先序遍历----求二叉树所有路径
class Solution {

public:

    vector<string> binaryTreePaths(TreeNode* root) {

        if(!root) return ans;

        bt(root,"");

        return ans;

        //reverse(ans;)

    }

    void bt(TreeNode* root,string path){
        if(!root) return;
        if(!root->left&&!root->right) {
            path=path+to_string(root->val);
            ans.push_back(path);
            return;
        }
        path=path+to_string(root->val)+"->";
      bt(root->left,path);
      bt(root->right,path);
    }

private:
    vector<string> ans;
};

应用先序----计算左叶子节点和：法1----直接递归
class Solution {

public:

int sum=0;

    int sumOfLeftLeaves(TreeNode* root) {

        if(! root) return 0;

        if(root->left&&! root->left->left&&! root->left->right) return root->left->val+sum+sumOfLeftLeaves(root->left)+sumOfLeftLeaves(root->right);

        return sum+sumOfLeftLeaves(root->left)+sumOfLeftLeaves(root->right);

    }

};
法2----加一个true，提高效率（使用一个额外参数达到压缩递归路径的效果）
class Solution {
public:
    int sumOfLeftLeaves(TreeNode* root) {
        return sumLeftLeaves(root, 0);
    }

private:
    int sumLeftLeaves(TreeNode* node, bool isLeft) {
        if (!node) return 0;
        // 如果当前节点是左叶子节点，则累加其值
        if (isLeft && !node->left && !node->right) {
            return node->val + sumLeftLeaves(node->right, false);
        }
        // 否则，递归计算左右子树
        return sumLeftLeaves(node->left, true) + sumLeftLeaves(node->right, false);
    }
};


找树左下角的值
class Solution {

public:

    int findBottomLeftValue(TreeNode* root) {

        if (!root) return 0;

        pair<int,int> ans = findBottomLeftHelper(root, 0);

        return ans.second;

    }  
private:

    pair<int, int> findBottomLeftHelper(TreeNode* root, int depth) {
        不是返回当前深度，而是返回上一层深度，说明这一层没有节点
        if (!root) return {depth-1, 0}; // 返回当前深度和空值

        if(root&&!root->left&&!root->right) return {depth,root->val};

        // 递归遍历左子树，因为我们要找到最左边的节点

        pair<int, int> left = findBottomLeftHelper(root->left, depth + 1);

        pair<int, int> right = findBottomLeftHelper(root->right, depth + 1);

        if(left.first == right.first) return left;

        return (left.first > right.first) ? left : right;

    }

};

判断是否有路径节点值之和等于目标值
class Solution {

public:

    bool hasPathSum(TreeNode* root, int targetSum) {

        if(! root) return false;

        bool ans = hashh(root,targetSum);

        return ans;

    }

    bool hashh(TreeNode* root, int targetSum) {

        if (!root){

            if(targetSum==0) return true;

            return false;

        }

      return hasPathSum(root->left,targetSum-root->val) | hasPathSum(root->right,targetSum-root->val);

    }

};


二叉树构造系列：
```


