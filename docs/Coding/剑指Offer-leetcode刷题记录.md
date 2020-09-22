---
layout: p
title: 剑指Offer_leetcode刷题记录
date: 2020-04-20 13:10:13
tags:
---

> 由于临近春招末期，时间比较紧，就不记录思考过程了，直接贴AC代码。以后有空补上
>
> 代码大多用C++，仅是过而已，没有进行优化。



#### [面试题03. 数组中重复的数字](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

```c++
class Solution {
public:
    int findRepeatNumber(vector<int>& nums) {
        std::ios::sync_with_stdio(false);
        int size = nums.size();
        if (size <0 ) return 0;
        vector<int> arr(size);
        for(int i=0; i < size; i++){
            arr[nums[i]] += 1;
            if (arr[nums[i]] > 1){
                return nums[i];
            }
        }
        return 0;
    }
};
```



#### [面试题04. 二维数组中的查找](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

```c++
class Solution {
public:
    bool findNumberIn2DArray(vector<vector<int>>& matrix, int target) {
        if (matrix.empty()) return false;
        
        int column = matrix[0].size();
        int row = matrix.size();
        if (column <= 0 || row <= 0) return false;
        int r = 0 ;
        int c = column - 1;

        while( r < row && c >= 0  ){
            if (matrix[r][c] == target){
                return true;
            }else if(matrix[r][c] > target){
                c --;
            }else{
                r ++;
            }
        }
        
        return false;
    }
};
```



#### [面试题05. 替换空格](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

```python
class Solution:
    def replaceSpace(self, s: str) -> str:
    	return s.replace(' ', '%20')
```





#### [面试题06. 从尾到头打印链表](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> reversePrint(ListNode* head) {
        vector<int> ans;
        ListNode *p = head;
        while( p != NULL){
            ans.insert(ans.begin(), p->val);
            // or : ans.push_back();
            // then : return reverse(ans.begin(), ans.end());
            p = p->next;
        }
        return ans;;
    }
};
```

顺便重新再写遍链表吧:

```

```





#### [面试题07. 重建二叉树](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        if (preorder.empty() && inorder.empty()) return NULL;
        TreeNode *root = createTree(preorder, inorder, 0, preorder.size()-1, 0, inorder.size()-1);
        return root;
    }


    TreeNode *createTree(vector<int>& preorder, vector<int>& inorder, int preL, int preR, int inL, int inR){
        if (preL> preR) return NULL;
        int k;
        for( k=inL; k<=inR; k++){		// ▲写的挺熟练了, 就是多了个int被恶心坏了
            if (preorder[preL] == inorder[k])
            break;
        }
        TreeNode *root= new TreeNode(preorder[preL]);   
        int numLeft = k - inL;
        root->left = createTree(preorder, inorder, preL+1, preL + numLeft, inL, k-1);
        root->right = createTree(preorder, inorder, preL +numLeft + 1, preR, k+1, inR);
        return root;
    }
};
```





#### [面试题09. 用两个栈实现队列](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

```c++
class CQueue {
private:
    stack<int> s1;
    stack<int> s2;

public:
    CQueue() {

    }
    
    void appendTail(int value) {
        s1.push(value);    
    }
    
    int deleteHead() {
        int ans;
        if(s1.empty() && s2.empty() )return -1;
        if (s2.empty()){
            while(!s1.empty()){
                s2.push(s1.top());
                s1.pop();
            }
        }
        
        ans = s2.top();
        s2.pop();
        return ans;
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * CQueue* obj = new CQueue();
 * obj->appendTail(value);
 * int param_2 = obj->deleteHead();
 */
```



#### [面试题10- I. 斐波那契数列](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

```c++
class Solution {
private:
    int arr[105];
    const int MOD = 1000000007;
public:
    int fib(int n) {
        if (n == 0){
            arr[n] = 0;
            return arr[n];
        }else if (n == 1){
            arr[n] = 1;
            return arr[n];
        }
        if ( arr[n] ){
            return arr[n];
        }else{
            // 这边是递归而不是arr[n-1] + arr[n-2]
            arr[n] = (fib(n-1)%MOD + fib(n-2)%MOD)%MOD;
            return arr[n];
        }
    }
};
```





#### [面试题10- II. 青蛙跳台阶问题](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

> 问题实质就是fib

```c++
class Solution {
    private:
    int arr[105];
    const int MOD = 1000000007;
public:
    int numWays(int n) {
        if (n == 1){
            arr[n] = 1;
            return arr[n];
        }else if (n == 2){
            arr[n] = 2;
            return arr[n];
        }else if (n==0){// 比上题多了个范围		
            arr[n] = 1;
            return arr[n];
        }
        if ( arr[n] ){
            return arr[n];
        }else{
            // 这边是递归而不是arr[n-1] + arr[n-2]
            arr[n] = (numWays(n-1)%MOD + numWays(n-2)%MOD)%MOD;
            return arr[n];
        }
    }
};
```





#### [面试题15. 二进制中1的个数](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

>  ` n & (n-1)`结果为 n <- 将n最右边的1改成0的数

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        uint32_t tmp = n;
        int ans = 0;
        while(tmp){
            ans ++;
            tmp = tmp & (tmp-1);
        }
        return ans;
    }
};
```





#### [面试题16. 数值的整数次方](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

> 快速幂， 对负数判断一下

```c++
    double myPow(double base, int n) {
        double res = 1;
        bool fu = false;
        if (n<0) fu = true;
        int tmp = abs(n);

        while(tmp>0){
            if(tmp&1) res = res * base; 
            // cout << res << " " ;
            base = base * base;
            tmp >>= 1;
        }
        if (fu) return 1/res;
        else return res;
    }
```





#### [面试题17. 打印从1到最大的n位数](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

> 因为n的范围没给， 所以其实需要考虑大数的，只不过不考虑好像也能过。

```c++
class Solution {
public:

    int pow(int base, int n){
        int res =1;
        for(int i=0;i<n;i++)
            res *= base;;
        return res;
    }

    vector<int> printNumbers(int n) {
        int ans = 0;
        for(int i=0 ; i < n ; i++){
            ans += 9*pow(10, i);
        }


        vector<int> v;
        for(int i=1; i <= ans; i ++) 
            v.push_back(i);

        return v;

    }
};
```



> 写了个奇怪的东西

```c++
#include <algorithm>
#include <bits/stdc++.h>
using namespace std;


vector<int> printNumbers(int n) {
    vector<int> res;
    int arr[10005];
    for(int i=0;i < n;i++) //cout << a << " ";
        arr[i] = i;
    do{
     string ans;
    for(int i=0;i < n;i++)
        ans += '0'+arr[i];
        res.push_back(atoi(ans.c_str()));
    }while(next_permutation(arr, arr+n));
    return res;
}


int main(){
    vector<int> v = printNumbers(3);
    for(vector<int>::iterator it = v.begin(); it != v.end(); it++)
        cout << *it << " ";
    return 0;
}
```







#### [面试题18. 删除链表的节点](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* deleteNode(ListNode* head, int val) {
        if (head == NULL) return NULL;
        if (head->val == val && head->next == NULL) return NULL;
        if (head->val == val) return head->next;
        ListNode *pre = head;
        ListNode *p = head->next;
        while(p != NULL){
            if (p->val == val){
                pre->next = p->next;
                delete(p);
                p = pre->next;
            }else{
                pre = p;
                p = p->next;
            }
        }

        return head;
    }
    
};
```







#### [面试题22. 链表中倒数第k个节点](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* getKthFromEnd(ListNode* head, int k) {
        if( head == NULL) return NULL;
        // int (k == 0 ) ; return head;		// 把0删了就可以了

        ListNode *p = head;
        for(int i = 0; i < k-1; i++){
            if (p->next == NULL) return NULL;
            p = p->next;
        }

        ListNode *jnode = head;
        while(p->next != NULL){
            p = p->next;
            jnode = jnode->next;
        }

        return jnode;
    }
};
```





#### [面试题24. 反转链表](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL) return NULL;
        // pre这个NULL卡了很久，如果反转的话，第一个节点需要指向NULL
        ListNode *pre = NULL;			
        ListNode *p = head;
        ListNode *ans = NULL;
        if (p->next==NULL) return head;
        while (p != NULL){
            ListNode *next = p->next;
            if (next == NULL) ans = p;
            p->next = pre;
            pre = p;
            p = next;
        }
        return ans;
    }
};
```





#### [面试题25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

> 大晚上感觉思路还行, 就是细节过不了.

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1 == NULL) return l2;
        if(l2 == NULL) return l1;
        if (l1 == NULL && l2 == NULL) return NULL;

        ListNode *head =new ListNode(0);		// 随便生成一个表头, 最后忽略这个就行了
        ListNode *newlist = head;
        while( l1 && l2){
                if ( l1->val < l2->val ){
                    newlist->next = l1;
                    l1 = l1->next;
                }else{
                    newlist->next = l2;
                    l2 = l2->next;
                }
                newlist = newlist->next;		// 指针也要往后
        }
        newlist->next = l1 ? l1 : l2;	// 最后只剩一个
        return head->next;
    }
};
```





#### [面试题26. 树的子结构](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        bool notsub = false;
        // if(A == NULL || B == NULL) return false;
        if ( A && B){
            if ( A->val == B->val )        // 找到根节点
                // notsub = isSubStructure(A, B);
                notsub = same(A,B);

            if (!notsub){
                // notsub = same(A->left,B);
                notsub = isSubStructure(A->left,B);
            }
            if (!notsub){
                // notsub = same(A->right, B);
                notsub = isSubStructure(A->right, B);
            }
        }
        return notsub;
    }


    bool same(TreeNode* A, TreeNode* B){
        if (B == NULL) return true;
        if (A==NULL) return false;
        if( A->val != B->val) return false;

        return same(A->left,B->left) && same(A->right, B->right);
    }
};
```





#### [面试题30. 包含min函数的栈](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

> 同时维护一个存储最小值的栈

```c++
class MinStack {
private:
    stack<int> common;
    stack<int> assist;
public:
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int x) {
        common.push(x);
        if (assist.empty()) assist.push(x);
        else{
            int top = assist.top();
            if ( x < top ) assist.push(x);
            else assist.push(top);
        }

    }
    
    void pop() {
        // int tmp = common.top();
        common.pop();
        assist.pop();
        // return tmp;
    }
    
    int top() {
        int tmp = common.top();
        return tmp;
    }
    
    int min() {
        int tmp = assist.top();
        // assist.pop();
        return tmp;
    }
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(x);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->min();
 */
```






#### [面试题31. 栈的压入、弹出序列](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof/)

```c++
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> st;
        int n = popped.size();
        int j = 0;
        for (int i = 0; i < pushed.size(); ++i){
            st.push(pushed[i]);
            while(!st.empty() && j < n && st.top() == popped[j]){
            // 注意逻辑短路问题， 判断是否为空 必须在top之前
                st.pop();
                ++j;
            }
        }
        return st.empty();
        // return (j == sz);
    }
};
```



#### [面试题32 - I. 从上到下打印二叉树](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

> BFS层次遍历

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> levelOrder(TreeNode* root) {
        vector<int> ans;
        if (root == NULL) return ans;
        queue<TreeNode *> q;
        q.push(root);
        while(!q.empty()){
            TreeNode *now = q.front();
            q.pop();
            ans.push_back(now->val);
            if (now->left) q.push(now->left);
            if (now->right) q.push(now->right);
        }
        return ans;
    }
};
```



#### [面试题32 - II. 从上到下打印二叉树 II](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
         vector<vector< int> > ans;
        if (root == NULL) return ans;
        queue<TreeNode *> q;
        q.push(root);
        while(!q.empty()){
            int sz = q.size();		// 统计当层的节点数
            vector<int> row;
            for(int i=0; i < sz; i++){
                TreeNode *now = q.front();
                q.pop();
                row.push_back(now->val);
                if (now->left) q.push(now->left);
                if (now->right) q.push(now->right);
            }
            ans.push_back(row);
        }
        return ans;
    }
};
```



#### [面试题32 - III. 从上到下打印二叉树 III](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

> 控制an.push_back的内容

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector< vector<int> > ans ;

        if (root== NULL) return ans;
         queue<TreeNode*> q;
        int level = 1;          // 基层从前往后， 偶层从后往前
        q.push(root);           // 第一层
        while(!q.empty()){
            int n = q.size();
            vector<int> line;
            for(int i = 0; i < n; i ++){
                TreeNode *now = q.front();
                q.pop();
                line.push_back(now->val);
                if ( now->left != NULL) q.push(now->left);
                if ( now->right != NULL ) q.push(now->right);

            }
            if ( level % 2 == 1)  ans.push_back(line);
            else {
                reverse(line.begin(), line.end());
                ans.push_back(line);
            }
            level ++;
        }
        return ans;
    }
};
```

---



```c++
class Solution {
public:
    bool verifyPostorder(vector<int>& postorder) {
        if (postorder.empty()) return false;
        int sz = postorder.size();

        return helper(postorder, 0, sz-1);
    }
        bool helper(vector<int>& postorder, int l, int r){

        int sz = postorder.size();
        int rootkey = postorder[sz-1];

        int smallp = 0;
        while( postorder[smallp] < rootkey) smallp++;
        // 之后smallp 为第一个 >= rootkey的索引， 即右子树的第一个
        int bigp = smallp;
        while( bigp < sz-1 ){   // 检测右子树是不是符合全部大于key
            if(postorder[bigp] < rootkey) return false;
        }

        bool left = true;
        if ( smallp > 0 )
         left =  helper(postorder, l, smallp-1);
        bool right = false;
        if ( bigp > 0)
             right =  helper(postorder, smallp, r-1);
        return (left && right);
        }


};
```

#### [剑指 Offer 41. 数据流中的中位数](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

> [Java中priorityQueue的使用——借用堆完成](https://www.cnblogs.com/Elliott-Su-Faith-change-our-life/p/7472265.html)
> add+offer（push）、element+peek（front-top）、remove+poll（pop）
>
> [c++优先队列(priority_queue)用法详解](https://www.cnblogs.com/huashanqingzhu/p/11040390.html)
>
> - 参数缺省的话，优先队列就是大顶堆，队头元素最大。最小堆可以用仿函数less指定
> - 如果是自定义类型, 则需要自己**重载operator<** 或者 自己**写仿函数**()
>   - 仿函数: （functor）又称为函数对象（function object）是一个能行使函数功能的<u>类</u>。仿函数的语法几乎和我们普通的函数调用一样，不过作为仿函数的类，<u>都必须重载operator()运算符</u>


```c++

// priority_queue<Type, Container, Functional>, 其中Functional为仿函数, 如greater, less
```





[数组中数字出现的次数](https://leetcode-cn.com/problems/shu-zu-zhong-shu-zi-chu-xian-de-ci-shu-lcof) 

```c++
#include <bits/stdc++.h>
using namespace std;

    int findFirst1(int n){
        int i = 0;
        cout << "enter" << n;
        //  && (i < sizeof(int) * 8)
        while( ( n & 1) == 0) {		//▲最后排查出来时这边括号没加
            n = n >> 1;
            i++;
        }
        // cout << "findFirst1" << i << endl;
        return i; 
    }

    bool isbit1(int n, int index){
        n = n >> index;
        return (n & 1);
    }

    void  solve(vector<int> &v, int &one, int &two){
        int sz = v.size();
        cout << sz << endl;
        int firstXOR = 0;
        // cout << "begin" ;
        for(int i = 0; i <sz; i++) {
            cout << v[i];
            firstXOR ^= v[i];
        }
        // cout << endl;
        // cout << "firstXOR" << firstXOR << endl;
        int index = findFirst1(firstXOR);
        // cout << index << endl;
        for(int i = 0; i < sz;i++ ){
            if ( isbit1(v[i], index) ){
                one ^= v[i];
            }else{
                two ^= v[i];
            }
        }
    }


    vector<int> singleNumbers(vector<int>& nums) {
        vector<int> v(2);
        if (nums.empty()) return v;
        solve(nums, v[0], v[1]);
        return v;
    }

int main(){
    int arr[] = {1,2,5,2};
    vector<int> v(arr, arr+ 4);
    vector<int> ans = singleNumbers(v);;
    cout << ans[0] << " " << ans[1] << endl;
}
```





#### [面试题47. 礼物的最大价值](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)-dp

```python
class Solution:
    def maxValue(self, grid: List[List[int]]) -> int:
        row, col = len(grid), len(grid[0])
        # print(row, col )
        # 初始化第一列
        for r in range(1, row):
            grid[r][0] += grid[r-1][0]
        # 初始化第一行
        for c in range(1, col):
            grid[0][c] += grid[0][c - 1]
        # dp状态更新
        for i in range(1, row):
            for j in range(1, col):
                grid[i][j] += max(grid[i-1][j], grid[i][j - 1])
                
        return grid[row - 1][col - 1]
```







#### [面试题49. 丑数](https://leetcode-cn.com/problems/chou-shu-lcof/)

```python
class Solution:
    def nthUglyNumber(self, n: int) -> int:
        dp, a, b, c = [1] * n, 0 , 0, 0
        for i in range(1, n):
            n2, n3, n4 = dp[a] * 2, dp[b] * 3, dp[c] * 5
            dp[i] = min(n2, n3, n4)
            if dp[i] == n2: a+=1
            if dp[i] == n3: b+=1
            if dp[i] == n4: c+=1
        return dp[-1]
```

#### [面试题50. 第一个只出现一次的字符](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

```python
from collections import OrderedDict

class Solution:
    def firstUniqChar(self, s: str) -> str:
        hashtable = OrderedDict()
        for i in s:
            hashtable[i] = hashtable.setdefault(i, 0) + 1
        for k, v in hashtable.items():
            if v == 1:
                return k
        return ' '
```



#### [剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

> 看到"**在排序数组中查询**"==> 二分查找

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        return getRight(nums, target) - getLeft(nums, target) + 1;  
    }

    // 找到和target相等的最边界(target 最小的下标)
    int getLeft(vector<int>& v, int target){
        int len = v.size();
        int l = 0, r = len-1;
        while( l <= r){
            int m = l + r >> 1;
            if ( v[m] < target){
                l = m + 1;
            }else{              // 如果相等的话缩小右边边界
                r = m - 1;
            }
        }
        return l;
    }
    
    // 找到和target的最右界(比target大的最小下标)
    int getRight(vector<int>& v, int target){
        int len = v.size();
        int l = 0, r = len-1;
        while( l <= r){
            int m = l + r >> 1;
            if ( v[m] <= target){		// 如果相等的话, 说明之前都是正确的, 那么就需要缩小左边界的范围
                l = m + 1;
            }else{
                r = m - 1;
            }
        }
        return r;
    }

};
```

### [剑指 Offer 53 - II. 0～n-1中缺失的数字

> 一个长度为n-1的**递增排序数组**中的所有数字都是唯一的==>二分
>
> [为什么最后return l解析](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/solution/mian-shi-ti-53-ii-0n-1zhong-que-shi-de-shu-zi-er-f/)

```c++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int len = nums.size();
        int l = 0, r = len - 1;
        while( l <= r) {		// 当 arr[r - l] 为空时跳出
            int mid = (l + r) >> 1;
            if ( nums[mid] == mid){
                l = mid + 1;
            }else if ( nums[mid] > mid){
                r = mid - 1;
            }
        } 
        // 当 r == l == mid时,还会再进行一轮, 此时有arr[mid] != mid 那么就是当前数字出了问题, return谁都可以, 但由于mid是在while里声明的, 因此只考虑return l or r, 由于不相等的情况下该轮while结束后r := mid - 1, 因此就偏离了。 最终就选取l了.
        return l;			  
    }
};
```



#### [面试题52. 两个链表的第一个公共节点](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

```python
class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        node1, node2 = headA, headB
        
        while node1 != node2:
            node1 = node1.next if node1 else headB
            node2 = node2.next if node2 else headA

        return node1
    

# # Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None




# def getListLength(head):
#     if head == None:
#         return -1
#     ans = 0
#     while head != None:
#         ans += 1
#         head = head.next

#     return ans


# def getIntersectionNode(headA: ListNode, headB: ListNode) -> ListNode:
#     # print(headA, headB)
#     lena = getListLength(headA)
#     lenb = getListLength(headB)
#     print(lena, lenb)
#     diff = lena - lenb 
#     if diff>0:
#         flagalong = True
#     else:
#         flagalong = False


#     if flagalong:
#         listLong = headA
#         listShort = headB
#     else:
#         listLong = headB
#         listShort = headA
#         diff = -diff

#     # print(diff)

#     for i in range(diff):
#         listLong = listLong.next
    
#     # print(listLong.val, listShort.val)

#     while listLong != None and listShort != None and listLong.val != listShort.val:
#         listShort = listShort.next
#         listLong = listLong.next

#     return listLong
    

# headA2 = ListNode(4)
# headA1 = ListNode(3)
# HeadA = ListNode(1)
# headA1.next = headA2
# HeadA.next = headA1


# headB2 = ListNode(4)
# headB1 = ListNode(3)
# HeadB = ListNode(2)
# headB1.next = headB2
# HeadB.next = headB1
```





#### [面试题54. 二叉搜索树的第k大节点](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

> 中序遍历, 从右往左的顺序就是从大往小

```c++
class Solution {
public:
    int kthLargest(TreeNode* root, int k) {
        help(root,k);
        return ans;
    }
    void help(TreeNode* root,int k){
        if(root!=nullptr){
        help(root->right,k);
        visit(root,k);
        help(root->left,k);
        }
    }
    void visit(TreeNode* root,int k){
        th++;
        if(th==k) ans=root->val;
    }
private:
    int th=0;
    int ans=0;
};
```



#### [面试题57. 和为s的两个数字](https://leetcode-cn.com/problems/he-wei-sde-liang-ge-shu-zi-lcof/)

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        l = 0
        r = len(nums) - 1
        while l < r:
            sums = nums[l] + nums[r]
            if sums > target:
                r -= 1
            elif sums < target:
                l += 1
            
            else:return nums[l], nums[r]
        return []
```







#### [面试题58 - I. 翻转单词顺序](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        res = s.split()
        return ' '.join(reversed(res))
    
    def reverseWords(self, s: str) -> str:
        res = s.split()
        return ' '.join(reversed(res))
```



#### [面试题58 - II. 左旋转字符串](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

```python
class Solution:
    def reverseLeftWords(self, s: str, n: int) -> str:
        return s[n:] + s[:n]
```







#### [面试题59 - I. 滑动窗口的最大值](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        ans = []
        if not nums: return []
        for i in range(len(nums) - k + 1):
            ans.append(max(nums[i: i + k]))

        return ans
```



#### [面试题59 - II. 队列的最大值](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

```python
from queue import deque, Queue


class MaxQueue:

    def __init__(self):
        self.queue = Queue()
        self.deque = deque()

    def max_value(self) -> int:
        return self.deque[0] if self.deque else -1


    def push_back(self, value: int) -> None:
        while self.deque and value > self.deque[-1]: 
            self.deque.pop()
        self.deque.append(value)
        self.queue.put(value)


    def pop_front(self) -> int:
        if not self.deque: return -1
        ans = self.queue.get()
        if ans == self.deque[0]:
            self.deque.popleft()
        return ans
```





#### [面试题60. n个骰子的点数](https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof/)-dp

```c++
class Solution {
public:
    vector<double> twoSum(int n) {
        int dp[15][70];
        memset(dp, 0, sizeof(dp));
        for (int i = 1; i <= 6; i ++) {
            dp[1][i] = 1;
        }
        for (int i = 2; i <= n; i ++) {
            for (int j = i; j <= 6*i; j ++) {
                for (int cur = 1; cur <= 6; cur ++) {
                    if (j - cur <= 0) {
                        break;
                    }
                    dp[i][j] += dp[i-1][j-cur];
                }
            }
        }
        int all = pow(6, n);
        vector<double> ret;
        for (int i = n; i <= 6 * n; i ++) {
            ret.push_back(dp[n][i] * 1.0 / all);
        }
        return ret;
    }
};
```



#### [面试题61. 扑克牌中的顺子](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

```python
class Solution:
    def isStraight(self, nums: List[int]) -> bool:
        if not nums: return False
        sort_list = sorted(nums)
        anything = 0
        # if sort_list[-1] - sort_list[0] == 4:
        gap = 0
        for index in range(len(sort_list) - 1):
            now_val = sort_list[index]
            next_val = sort_list[index+1]
            # print(now_val, next_val)
            if now_val == 0: anything += 1
            elif now_val > 0:
                # 出现对子
                if now_val == next_val: return False
                else:
                    gap += next_val - now_val -1 
        # print(gap, anything)
        if gap > anything:
            return False
        else: return True
```



#### [面试题62. 圆圈中最后剩下的数字](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

```python
def f(n, m):
    if n == 0:
        return 0
    x = f(n - 1, m)
    return (m + x) % n

class Solution:
    def lastRemaining(self, n: int, m: int) -> int:
        return f(n, m)
```



```python

# # Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None




# def getListLength(head):
#     if head == None:
#         return -1
#     ans = 0
#     while head != None:
#         ans += 1
#         head = head.next

#     return ans


# def getIntersectionNode(headA: ListNode, headB: ListNode) -> ListNode:
#     # print(headA, headB)
#     lena = getListLength(headA)
#     lenb = getListLength(headB)
#     print(lena, lenb)
#     diff = lena - lenb 
#     if diff>0:
#         flagalong = True
#     else:
#         flagalong = False


#     if flagalong:
#         listLong = headA
#         listShort = headB
#     else:
#         listLong = headB
#         listShort = headA
#         diff = -diff

#     # print(diff)

#     for i in range(diff):
#         listLong = listLong.next
    
#     # print(listLong.val, listShort.val)

#     while listLong != None and listShort != None and listLong.val != listShort.val:
#         listShort = listShort.next
#         listLong = listLong.next

    
#     return listLong
    





# headA2 = ListNode(4)
# headA1 = ListNode(3)
# HeadA = ListNode(1)
# headA1.next = headA2
# HeadA.next = headA1


# headB2 = ListNode(4)
# headB1 = ListNode(3)
# HeadB = ListNode(2)
# headB1.next = headB2
# HeadB.next = headB1


```







#### 最长公共前缀

```python

class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if len(strs) == 0: return ''
        if len(strs) == 1: return strs[0]
        strs = sorted(strs)
        idx = 0
        minlen = min(len(strs[0]), len(strs[-1]))
        print( strs[0],  strs[-1])
        while idx < minlen:
            if strs[0][idx] != strs[-1][idx]:
                return strs[0][:idx]
            idx += 1

        return len(strs[0])  if len(strs[0]) < len(strs[-1]) else len(strs[-1]) 
        # 

s = Solution()
print(s.longestCommonPrefix(['abcd', 'abdd', 'aad']))
```



#### 模拟队列

> 腾讯笔试题第一题，第四题为“两个栈模拟队列”

```c++
/*
 * @Author: Mrli
 * @Date: 2020-04-26 19:58:20
 * @LastEditTime: 2020-04-27 22:58:50
 * @Description: 
 */
// #include <bits/stdc++.h>	//万能头文件
#include <iostream>
using namespace std;		//命名空间
const int MAXN = 1005;
int arr[MAXN];

int main() {
    ios::sync_with_stdio(false);	//取消输入输出流等待同步
    int T;
    cin >>T;
    while(T--){
        int n;      // n个操作
        cin >>n;
        fill(arr, arr+MAXN, 0);
        int di = 0;
        int start = di;
        int p = di; // 栈底为-1 尾指针
        for(int i = 0; i < n; i ++){
            string operate;
            cin >> operate;
            // cout << "p:" << p  << start << endl;
            if (operate == "PUSH") {
                int num;
                cin >> num;
                arr[p++] = num;
            }else if (operate == "TOP"){
                if ( p == start) cout << -1 <<endl;
                else cout << arr[start] <<endl;
            }else if (operate == "POP"){
                if (p == start) cout << -1 <<endl;
                else start++;
            }else if (operate == "SIZE"){
                cout << p - start << endl;
            }else if (operate == "CLEAR"){
                p = di;
                start = di;
            }
        }
    }
    return 0;
}

// 2
// 7
// PUSH 1
// PUSH 2
// TOP
// POP
// TOP
// POP
// POP
// 5
// PUSH 1
// PUSH 2
// SIZE
// POP
// SIZE
```







#### 记一下二维vector的初始化

```c++
vector< vector<int> > v(3, vector<int>(5,4));
for(int i = 0; i < v.size() ; i ++){
    for(int j = 0 ; j < v[0].size(); j++){
        cout << v[i][j] ; 
    }
    cout << endl;   
}
```

和通过数组来初始化

```c++
int arr[] = {1,6,3,2,5};
vector<int> ans(arr, arr + sizeof(arr));
```









华为2016年秋招题：

A:

```python
while True:
    try:		# try一定要最后加， 不然不好debug
        n, m = map(int, input().split())
        grades = list(map(int, input().split()))
        for i in range(m):
            opt, (ids), (val) = input().strip().split()
            ids = int(ids)
            val = int(val)
            if opt == 'U':
                grades[ids-1] = val
            elif opt == 'Q':
                start, end = sorted([ids, val])
                print(max( grades[start-1: end]))
    except:
        break
# print("a" < "b" , "3" < "4")
# print('a' < 'b' , '3' < '4')
# print('A' < 'b' , 'A' < '2')

```

B[编程题]简单错误记录

```python
'''
@Author: Mrli
@Date: 2020-04-28 21:47:46
@LastEditTime: 2020-04-28 22:22:20
@Description: 
'''
import collections
# import sys
rec_dict = collections.OrderedDict()

# 另一种输入方式
# for line in sys.stdin:
#     ele = line.split('\\')[-1].strip('\n')
#     if ele not in lst:
#         lst.append(ele)
#     if ele in dct:
#         dct[ele] = dct[ele] + 1
#     else:
#         dct[ele] = 1

while True:
    try:
        ins = input().strip()
        path = ins.split('\\')[-1]
        if path not in rec_dict:
            rec_dict[path] = 1
        else:
            rec_dict[path] += 1
    except EOFError:
        break
# print(rec_dict)
sort_res = sorted(rec_dict.items(), key = lambda d: d[1], reverse=True)
# print(rec_dict)
for k, v in sort_res[:8]:
    path, linenum = k.split()
    print(path[-16:], linenum, v)

```

Tips: 两种输入方式的推出都为：`ctrl + z`

> EOF是一个计算机术语，为End Of File的缩写，在操作系统中表示资料源无更多的资料可读取。资料源通常称为档案或串流。
>
> 而在不同系统的EOF所代表的值是不一样的，在Visual Studio 2017下为ctrl+c，windows下为ctrl+z，linux/unix下为ctrl+c或ctrl+d；
>
> 运用这个小技巧可以在调试的时候手动结束，很方便。

#### 两路合并算法

```c++

void merge(int *arr, int l, int mid, int r){
    int begin1, begin2;
    begin1 = l;
    begin2 = mid+1;
    int index; // 新数组添加元素的索引值
    // int *newarr = (int *)malloc((r-l+1)*sizeof(int));
    int *newarr = new int[r-l+1];

    for(index = 0; begin1 <= mid && begin2 <= r; ){
        if ( arr[begin1] < arr[begin2] ){
            newarr[index++] = arr[begin1++];
        } else{
            newarr[index++] = arr[begin2++];
        }
    }
    cout <<"lmr: " << l <<" " <<mid << " " << r << endl;
    // cout << "res:" << (begin1 <= mid) << (begin2 <= r) <<endl;
    // while (begin1 <= mid){
    //     newarr[index++] = arr[begin1++];
    // }
    // while(begin2 <= r){
    //     newarr[index++] = arr[begin2++];
    // }   
    if (begin1 <= mid){
        newarr[index++] = arr[begin1++];
    }
    else{
        newarr[index++] = arr[begin2++];
    }   
    int start = l;      // arr开始更新的地方
    int k = 0;          // 新数组
    // while( start <= r){  // 将新数组全部拷贝进 arr[l] -> arr[r]
    //     arr[start++] = newarr[k++]; //copy排好序的数。
    // }
    for(int i = l; i <= r; i++){
        cout << newarr[k];
        arr[i] = newarr[k++];
    }
}
void mergesort(int *arr, int l, int r){
    if (l < r){
        int mid = l + ((r-l) >>1);	// ▲位运算一定要加括号
        // int mid = (l+r)/2;
        mergesort(arr, l, mid);
        mergesort(arr, mid+1, r);
        merge(arr, l, mid, r);
        cout <<"arr: " << l <<" " <<mid << " " << r << endl;
    }
}

 
  void mergeSort(int* arr, int s, int t)
  {
	  if (s < t)
	  {
		  int m = (s + t) / 2;
		  mergeSort(arr, s, m);
		  mergeSort(arr, m + 1, t);
		  merge(arr, s, m, t);
	  }
  }

int main(){
    // srand( (unsigned)time( NULL ) );  
    // Random(N);
    int a[N] = {3,2,5,4,1};
    for(int i = 0 ; i < N; i++) cout << a[i] <<" ";
    // // mergeSort(a, 0, N-1);//  改
    mergesort(a, 0, N-1);
    cout << "after sort:" <<endl; 
    for(int i = 0 ; i < N; i++) cout << a[i] <<" ";
    // for(int i= 0; i < 10 ;i ++){
    //     for( int j = 0; j < 5; j ++){
    //         cout << ( i + j)/ 2  <<" " << (i + (j-2))
    //     }

    // }
    return 0;
}

```



#### 快排

```c++
#include<iostream>
using namespace std;
void quickSort(int a[], int m,int n);
int partion(int a[], int m, int n);
int main()
{
	int a[] = { 6,1,2,7,9,3,4,5,10,8 };
	int m = 0;
	int n = (sizeof(a) / 4)-1;
	quickSort(a, m,n);
	for (int i = 0; i < 10; i++)
	{
		cout << a[i] << " ";
	}
}
void quickSort(int a[], int m, int n)
{
	if (m < n)
	{
		int q = partion(a, m, n);
		quickSort(a, m, q );
		quickSort(a, q + 1, n);
	}
}
int partion(int a[], int m, int n)
{
	int key=m;
	int j= n,i=m;
	int temp1, temp2;
	while (i != j)
	{
		while (a[j] > a[key] && i < j)
		{
			--j;
		}
		
		while ((a[i] < a[key]) && (i < j))
		{
			++i;
		}if (i < j)
		{
			temp1 = a[j];
			a[j] = a[i];
			a[i] = temp1;
		}
	}
		temp2 = a[key];
		a[key] = a[i];
		a[i] = temp2;
		return i;
}
```



```c++
void quickSort(int arr[], int l, int r){
    if ( l >= r) return;
    int i = l, j = r;
    int key = l + r >> 1;
    int x = arr[key];
    while ( i < j){
        while (arr[j] > x ) j --;
        while (arr[i] < x) i++;
        if (i < j){
            swap(arr, i, j);
        }
        quickSort(arr, l, j);
        quickSort(arr, j+1, r);
    }
}
```

上述两种有重复元素就爬了

```c++
void quick_sort(int q[], int l, int r)
{
    if (l >= r) return;

    int i = l - 1, j = r + 1, x = q[l + r >> 1];
    while (i < j)
    {
        do i ++ ; while (q[i] < x);
        do j -- ; while (q[j] > x);
        if (i < j) swap(q[i], q[j]);
    }
    quick_sort(q, l, j), quick_sort(q, j + 1, r);
}

```



#### 记录下C++的split写法：

```c++
#include <bits/stdc++.h>
using namespace std;

int main(){
    string s;
    getline(cin, s);
    // cout << s << endl;
    int dindex = s.find('-');
    cout << dindex<< endl;
    string a = s.substr(0, dindex);
    string b = s.substr(dindex+1);
    cout << a << "*" << b;
    
    cout << set
    return 0;
}
```

[C/C++中substr函数的应用(简单讲解)](https://www.cnblogs.com/ECJTUACM-873284962/p/6763801.html)

[C++如何保留两位有效数字！！！！](https://blog.csdn.net/patrick_star_cowboy/article/details/79199596)





```
def removeElement(nums, val):
    # print(nums)
    nums = list(map(str, nums))
    st = ''.join(nums)
    st = st.replace(str(val), '')
    return list(map(int, st))
```





```python
class Solution:
    @staticmethod
    def firstUniqChar( s: str) -> str:
        hashtable = OrderedDict()
        ans = ''
        for i in s:
            hashtable[i] = hashtable.setdefault(i, 0) + 1
        for k, v in hashtable.items():
            if v == 1:
                return k


res = Solution.firstUniqChar("abaccdeff")
print(res)
```







#### Sum为m, 拆成n个数, 有哪些情况?(非连续)

```c++
#include <bits/stdc++.h>

using namespace std;
vector<int> ans;
int idx;

void printRes(){
    for(auto &i: ans){
        cout << i << " ";
    }
    cout << endl;
}


// way1
void func(int val, int n){
    if (n == 1){
        ans.push_back(val);
        idx ++;
        cout << "Case:" << idx << endl;
        printRes();
        cout << "==========" << endl;
        ans.pop_back();
        return ;
    }

    // 去重
    int last;
    if ( ans.empty() ) last = 1;
    else 
        last = ans.back();
    for (int i = last; i <= val/2 ; i++) {
        ans.push_back(i);
        func(val-i, n-1);
        ans.pop_back();
    }
}

// way2
// void func(int val, int n, int start){
//     if (n == 1){
//         ans.push_back(val);
//         idx ++;
//         cout << "Case:" << idx << endl;
//         printRes();
//         cout << "==========" << endl;
//         ans.pop_back();
//         return ;
//     }
//     for (int i = start; i <= val/2 ; i++) {
//         ans.push_back(i);
//         func(val-i, n-1, i);
//         ans.pop_back();
//     }
// }


int main(){
    // way1
    func(20, 4);
    
    // way2
    // func(20, 4, 1);
    return 0;
}
```



#### Sum为m, 拆成n个数, 有哪些情况?(连续)

```java
public int subarraySum(int[] nums, int k) {
        // hash
        // 记录合适的连续字符串数量
        int count=0;
        // 记录前面数字相加之和
        int pre=0;
        // map记录前几个数字之和为K出现相同和的次数为V
        HashMap<Integer,Integer> map = new HashMap<>();
        // 初始化
        map.put(0,1);
        for (int i = 0; i < nums.length; i++) {
            pre+= nums[i];
            // 如果前面数字之和加上这个数字正好等于K（存在一个数字加上nums[i]结果为K
            // 说明找到了
            if (map.containsKey(pre-k)){
                // 累计
                count+=map.get(pre-k);
            }
            // 计算新的和放入map
            map.put(pre,map.getOrDefault(pre,0)+1);
        }
        return count;
    }
```





