



## [75. 颜色分类](https://leetcode-cn.com/problems/sort-colors/)

> **注意:**不能使用代码库中的排序函数来解决这道题。
>
> 简单模拟题： 首先，迭代计算出0、1 和 2 元素的个数，然后按照0、1、2的排序，重写当前数组。

```cpp
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int arr[4] = {0,0,0,0};
        for(int i: nums){
            arr[i] ++ ;
        }
        int idx = 0;
        int len = nums.size();
        for(int i = 0; i <= 2;i ++ ){
            int x = arr[i];
            for(int j = 0 ; j < x; j ++){
                nums[idx++] =i;
            }
        }
    }
};
```

## 2020年10月8日

### [344. 反转字符串](https://leetcode-cn.com/problems/reverse-string/)

> 需要不额外增加空间==> 原地交换 ==>双指针

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int len = s.size();
        for (int i = 0; i < len / 2; i++) {
            swap(s[i], s[len-i-1]);
        }
    }
};
```



## 2020年10月14日

### [1002. 查找常用字符](https://leetcode-cn.com/problems/find-common-characters/)

> 一共会有26位小写字母, 因此用个数组记录最少出现的次数

```c++
class Solution {
public:
const int M = 26;
vector<string> commonChars(vector<string>& A) {

    vector<int> minFreq(M, INT_MAX);
    vector<int> freq(M);
    for (auto &x: A) {
        // 统计出当前这个string中各个字符出现的次数
        fill(freq.begin(), freq.end(), 0);
       for (int i = 0; i < x.size(); i++) {
          freq[ x[i] - 'a'] ++; 
       }

       for (int j = 0; j < M; j++) {
           // 确定每个字符在所有字符中最小的出现次数
          minFreq[j] = min(freq[j], minFreq[j]);
       }
    }

    vector<string> ans;
    for (int i = 0; i < M; i++) {
       for (int j = 0; j < minFreq[i]; j++) {
           // emplace_back 骚操作
          ans.emplace_back(1, 'a' + i);
       }
    }

    return ans;
}
};
```

Python写法, 也可以使用Collection.Counter的奇淫怪技

```python
from typing import List

class Solution:
    def commonChars(self, A: List[str]) -> List[str]:
        res = dict()
        if not A: return []
        for c in A[0]:
            cnt = A[0].count(c)
            for s in A:
                cnt = min(cnt, s.count(c))
                if cnt == 0:
                    break
            if cnt > 0:
                res[c] = cnt
        ans = []
        for k, v in res.items():
            print(k, v)
            ans.extend(k * v)
        return ans
        
ans = Solution.commonChars(["bella", "label", "roller"])
print(ans)

```



## 2020年10月15日

### [116. 填充每个节点的下一个右侧节点指针](https://leetcode-cn.com/problems/populating-next-right-pointers-in-each-node/)——

> - 你只能使用常量级额外空间。 ===> 队列的BFS是个好的解决方案
> - 使用递归解题也符合要求，本题中递归程序占用的栈空间不算做额外的空间复杂度。

```c++
class Solution {
public:
      Node* connect(Node* root) {
        if (root == NULL) return NULL;
        queue<Node *> q;
        q.push(root);

        while(!q.empty()){
            int sz = q.size();		// 记录每一层的节点个数
            for (int i = 0; i < sz; i++) {
                Node *now = q.front();
                q.pop();
               if (i == sz - 1){
                   now->next = NULL;
               }else{
                   now->next = q.front();
               }
               if ( now->left != NULL) q.push(now->left);
               if ( now->right != NULL ) q.push(now->right);
            }
        }
        return root;
    }
};
```





## 2020年10月16日

### [977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)

> 非常简单的一道模拟题, 第一个想法就是用Python的map实现

```PYTHON
# 99.86%; 5.22%
# 由于最后是又生成了个list, 因此内存消耗比较大
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        return list(sorted(map(lambda x: x*x, A)))
    
# 99.62%; 94.78%
class Solution:
    def sortedSquares(self, A: List[int]) -> List[int]:
        for i in range(len(A)):
            A[i] = A[i] * A[i]
        A.sort()
        return A
```

双指针, O(n)时间复杂度, O1空间复杂度

> 由于题目提供了降序, 并最终生成有序的列表。 因此可以使用双指针逐次比出一个新的有序列表来。一种是找到最小的分界线（输入中绝对值最小）后开始， 二是从大的逆序排出新序列。

```cpp

class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        int noNegative = 0;
        vector<int> ans;
        int len = A.size();
        if (len == 0) return ans;
        ans.resize(len);
        int i = 0, j = len - 1;
        int pos = len - 1;
        while( i <= j){
            int nowi = A[i] * A[i];
            int nowj = A[j] * A[j];
            if ( nowi >= nowj ){
                ans[pos--] = nowi;
                i ++;
            }else{
                ans[pos--] = nowj;
                j --;
            }
        } 
        return ans;
    }
};



// 法二
class Solution {
public:
    vector<int> sortedSquares(vector<int>& A) {
        int noNegative = 0;
        vector<int> ans;
        int len = A.size();
        if (len == 0) return ans;
        
        // 找到最后一个负数
        for (int i = 0; i < len; ++i) {
            if (A[i] < 0) noNegative = i; 
            else  break;
        }
        int i = noNegative, j = noNegative + 1;
        
        // 或者, 找到第一个非负数
        // while( noNegative < len && A[noNegative] < 0){
        //     noNegative++;
        // };
        // int i = noNegative -1 , j = noNegative ;
        
        while( i >= 0 || j < len ){
            if ( i < 0 ){
                ans.push_back( A[j] * A[j]);
                j ++; 
            }
            else if ( j == len) {
                ans.push_back(A[i] * A[i]);
                i --;
            }
            else if (A[i] * A[i] <  A[j] * A[j]){
                ans.push_back(A[i] * A[i]);
                i--;
            }else{
                ans.push_back( A[j] * A[j]);
                j++;
            }
        }        
        return ans;
    }
};


```

## 2020年10月18日签到

### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)、[2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

> 由于当天有事，自己的电脑不在身边， 所以用同学的电脑做了两题。

```c++
# 暴力法一, On2
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int len = nums.size();
        if (len == 0) return {};
        for (int i = 0; i < len; i++) {
           for (int j = i + 1; j < len; j++) {
              if ( nums[i] + nums[j] == target){
                  return {i, j};
              }
           }
        }
        return {};
    }
};

// 优化On1: 考虑到上面i，在找target-i时又遍历了遍,导致了额外循坏， 因此可以通过hashtable来优化查询
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        int len = nums.size();
        if (len == 0) return {};
        unordered_map<int, int> mp;
        for (int i = 0; i < len; i++) {
           unordered_map<int, int>::iterator iter = mp.find( target - nums[i] );
           if ( iter != mp.end() ){         // 找到了
                return {iter->second, i};
           }
           mp[ nums[i] ] = i;
        }
        return {};
    }
};
```

笔记： 

- 学到了如何处理函数返回值`vector<type>`为空的场景==>`return {}`, 解释： C++11后支持`vector<int> v = {1,2,3,4}`的初始化，因此可以这么操作
- unordered_map、map
  - 其额find函数可以用来找寻map中是否有相应的key：因为所以用find来找target-nums[i]的值， 这边mp的key存的是输入的值，value存的是索引
  - 如果没有则会返回相应的末尾迭代器，e.g.`mp.end()`，所以这边find的返回值可以直接用iterator迭代器来承接。
    - 同时需要注意：当是STL中封装的类型如map, vector……的find时没有找到用` iter != v.end()`， 只有当string用find的时候才是`s.find("c") != string::npos`

**两数相加**

> 顺手写了下第二题的两束相加： 链表 + 加法进位

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        // 链表头
        ListNode *head = new ListNode(0);
        ListNode *tail = head;
        int carry = 0;
        while (l1 || l2) {
            // 如果;1没值则默认补0
            int v1 = 0 , v2 = 0;
            if (l1){
                v1 = l1->val;
                l1 = l1->next;
            }
            if (l2){
                v2 = l2->val;
                l2 = l2->next;
            }
            
            tail->next = new ListNode(sum % 10);
            tail = tail->next;
            carry = sum / 10;
        }
        if (carry > 0) tail->next = new ListNode(carry);
        
        return head->next;
    }
};


// 非常简介的写法， 让我想起了闫总的大数相加的模板
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = new ListNode(0), *ptr = head;
        int c = 0;
        while (l1 || l2 || c) {
            if (l1) {c += l1->val; l1 = l1->next;}
            if (l2) {c += l2->val; l2 = l2->next;}
            ptr->next = new ListNode(c%10);
            c = c/10;
            ptr = ptr->next;
        }
        return head->next;
    }
};
```

笔记：

- `if ( node != NULL)`、`if ( node != nullptr ) `、`if ( node )`三者效果在Leetcode中等价, `if (nullptr == NULL) `返回值为true

  [nullptr和NULL区别](https://blog.csdn.net/zzq060143/article/details/96278516)

  - nullptr，①是c++11中的关键字，表示空指针。②nullptr是一个字面值常量，类型为std::nullptr_t,空指针常数可以转换为任意类型的指针类型。
  - NULL是一个宏定义，在c和c++中的定义不同，c中NULL为`（void*)0`,而c++中NULL为整数0；在c++中`int *p=NULL; `实际表示**将指针P的值赋为0**，而c++中当一个指针的值为0时，认为指针为空指针





## 2020年10月19日

### [52. N皇后 II](https://leetcode-cn.com/problems/n-queens-ii/)——补题



### [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

> 非常经典的一道链表题：dummyHead节点 + 快慢指针。 
>
> - 做的时候看到**一遍遍历**就想到快慢指针了， 没想起头结点，当过不了样例的时候才明白得对删头操作进行特判， 最后还是加上dummyHead会更通用一些。
>
> - Q: 让cur指针先走多少?
>
>   A: n = 2时, 表面意思是到Cur指针指到nullptr时，删除在cur前两个（隔1个）的指针pre。所以可以看出如果没有头结点，操作方式为：**可以先让cur先走n步(隔n-1个)，再让pre和cur同步往前走**。但这么操作的结果是，当cur指向nullptr时，pre正好指向被删除的元素，这样是无法找到pre的前驱结点的，就无法完成pre结点的删除--->所以pre不能指向应该删除结点，而应该是pre的前驱结点。
>
>   ​	-->总结: 操作方式应该改为: 可以先让cur先走**n+1步**，再让pre和cur同步往前走。这种情况pre指向的就是前驱结点了。
>
>   - dummyHead结点存在不影响先走n+1步的结论，不需要为dummyHead额外多走: 在while同步走时抵消了cur和pre都要走dummy
>
>   ▲.[动画解析](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/solution/dong-hua-tu-jie-leetcode-di-19-hao-wen-ti-shan-chu/)

**dummyHead**: 在对链表进行操作时，一种常用的技巧是添加一个哑节点（dummy node），它的 next指针指向链表的头节点。这样一来，我们就不需要对头节点进行特殊的判断了。
本题中，如果我们要删除节点 y，我们需要知道节点 y的前驱节点 x，并将 x的指针指向 x的后继节点。但由于头节点不存在前驱节点，因此我们需要在删除头节点时进行特殊判断。但如果我们添加了哑节点，那么头节点的前驱节点就是哑节点本身，此时我们就只需要考虑通用的情况即可。

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode *dummyHead = new ListNode(0, head);
        ListNode *pre = dummyHead, *cur = dummyHead;
        for (int i = 0; i < n + 1; i++) {
           cur = cur->next;
        }
        while( cur ){
            cur = cur->next;
            pre = pre->next;
        }
        ListNode *delNode = pre->next;
        pre->next = delNode->next;
        delete delNode;
        ListNode *ansNode = dummyHead->next;
        delete dummyHead;
        return ansNode;
    }
};
// 不delete结点
// 执行用时：4 ms, 在所有 C++ 提交中击败了94.01%的用户
// 内存消耗：10.9 MB, 在所有 C++ 提交中击败了12.60% 的用户

// delete结点, 会降低内存消耗, 但是执行用时又会下降
```

### [844. 比较含退格的字符串](https://leetcode-cn.com/problems/backspace-string-compare/)——补题

> 简单题：大概率是模拟。同时，塞入-退格消除的模式又很容易让人想到栈的操作， 但不一定需要显性的用到栈-->可以STL提供容器的push_back, pop_back， 因此我第一个想到的是vector， 并且两个vector之间还可以用==来判断是否相等， 因此就出现了如下的代码。看了题解后发现string确实更直观一些

```cpp
// 法一： 时间复杂度O(N+M) ， 空间复杂度O(N+M)
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        vector<char> v1, v2;
        for (int i = 0; i < S.size(); i++) {
           if ( S[i] != '#' ){
               v1.push_back(S[i]);
           }else{
               if (!v1.empty())
                v1.pop_back();
           }
        }        

        for (int i = 0; i < T.size(); i++) {
           if ( T[i] != '#' ){
               v2.emplace_back(T[i]);
           }else{
               if (!v2.empty())
                  v2.pop_back();
           }
        }     
        return v1 == v2;   
    }
};

// 官方答案
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        return build(S) == build(T);
    }

    string build(string str) {
        string ret;
        for (char ch : str) {
            if (ch != '#') {
                ret.push_back(ch);
            } else if (!ret.empty()) {
                ret.pop_back();
            }
        }
        return ret;
    }
};




```



为了实现O(1)的空间复杂度： 1.原地修改 2. 双指针

```cpp
// 原地修改
class Solution {
public:
    int type(string & s) {
        int st = 0;
        for (auto c : s) {
            if (c != '#')
                s[st++] = c;
            else if (st > 0)
                st--;
        }
        return st;
    }
    bool backspaceCompare(string S, string T) {
        int len1 = type(S), len2 = type(T);
        return len1 == len2 && equal(
            S.begin(), S.begin() + len1,
            T.begin(), T.begin() + len2
        );
    }
};

// 双指针
class Solution {
public:
    bool backspaceCompare(string S, string T) {
        int lens = S.size(), lent = T.size();
        int i = lens - 1, j = lent - 1;
        int skips = 0, skipt = 0;
        while( true ){
            while ( i >= 0 ){
                if ( S[i] == '#' ){		// 记录本次有多少个退格键#
                    skips++;
                    i--;
                }else if (skips){		// 生效退格键
                    i -- ;
                    skips --;
                }else {                 // 如果是正常字母的话就跳出
                    break;
                }
            }
            while ( j >= 0 ){			// T字符串同理
                if ( T[j] == '#' ){
                    skipt++;
                    j--;
                }else if (skipt){
                    j -- ;
                    skipt --;
                }else {    
                    break;
                }
            }
            if ( i < 0 || j < 0) break;			// 一旦有至少一个遍历完了, 那么就可以知道结果了. 只需要看是不是i, j同时遍历完的
            if ( S[i] != T[j] ) return false;	// 如果当前的不一样, 那么一定不一样
            i --, j --;
        }

        if ((i == -1) && (j == -1)) return true;
        return false;
    }
};
```



## 2020年10月20日

> 今天跟朋友出去玩了, 回来后在改项目代码, 第二天补上



### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)

> 题意: 给一个链表，然后依次头尾头尾头尾取元素，组成新的链表。

这题没什么思路， 2020年10月21日先看题解， 过几天再自己实现()

**解法一：存储线线性表**

> 链表的缺点就是不能随机存储，当我们想取末尾元素的时候，只能从头遍历一遍，很耗费时间。第二次取末尾元素的时候，又得遍历一遍。
>
> 所以先来个简单粗暴的想法，把链表存储到线性表中，然后用双指针依次从头尾取元素即可。思路跟双向队列一致

**解法二：对半，逆序，重组**





## 2020年10月21日

### [925. 长按键入](https://leetcode-cn.com/problems/long-pressed-name/)——补题

> 两个字符串异同问题, 从左往右-->双指针依次对比

```cpp
// 第一版, 以name字符串为遍历条件, 1."leelee", "lleeelee"样例过不了, 因为如下写法相当于看到e就会先把后面一次性碰到的e全都吃掉, 而当i到第二个e的时候后面就没有e与之匹配了; 2.以及没考虑过是否匹配玩的问题
class Solution {
public:
    bool isLongPressedName(string name, string typed) {
        int len1 = name.size(), len2 = typed.size();
        int i = 0, j = 0;
        while( i < len1 ){
            if ( name[i] != typed[j] && (i > 0 && name[i] != name[i-1])) return false;
            while(typed[j] == name[i] ) j ++;
            i ++;
        }
        return true;
    }
};

class Solution {
public:
    bool isLongPressedName(string name, string typed) {
        int len1 = name.size(), len2 = typed.size();
        int i = 0, j = 0;
        while( j < len2 ){
            if ( name[i] == typed[j] ){
                // 如果当前遍历的字母相同则同时往后+1
                i++; j++;
            }else if ( j > 0 && typed[j-1] == typed[j]){
                 // 如果typed后一位跟前一位相同， 则往后移一位， 但需要注意的是每一次移动都得跟name[i]再比一次， 不然就会出现1版中的问题
                j ++ ;
            }else{
                return false;
            }
        }
        return i == len1;
    }
};
```

总结: 双指针以哪个为准问题: 

- 双指针遍历同一个对象: 如[977. 有序数组的平方](https://leetcode-cn.com/problems/squares-of-a-sorted-array/)
- 双指针分别遍历不同对象: 如本题、[PAT**1084** **Broken Keyboard** ](https://pintia.cn/problem-sets/994805342720868352/problems/994805382902300672)， 以长的为while或者for的遍历条件，以基准、短的最后遍历的i指针是否等于len来判断整个的完成情况

## 2020年10月22日

#### [763. 划分字母区间](https://leetcode-cn.com/problems/partition-labels/)——补题

## 2020年10月23日

### [234. 回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/)——补题

> 回文的判断一般都是双指针， 一个从前，一个从后，只不过由于这个不是线性表，而实链表，无法像线性表那么随意按index来取元素， 因此第一个做法就是先按链表遍历的方式把遍历的结果赋值到线性表上再按线性表的做法做。O(n)-O(n)

#### 常规做法

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if (head == nullptr) return true;
        vector<int> arr;
        while(head){
            arr.push_back(head->val);
            head = head->next; 
        }
        int len = arr.size();
        for (int i = 0, j = len - 1; i < j; ) {
           if ( arr[i] == arr[j] ){
               i ++;
               j --;
           }else{
               return false;
           }
        }
        return true;

    }
};
```

#### *Hash做法

> hash的思想: 用一串独一无二的数来表示某个对象, 如果hash值相同, 则"能"认为是同一个对象.
>
> 根据此人提出的hash公式: [hash 遍历一次就行 代码很短](https://leetcode-cn.com/problems/palindrome-linked-list/solution/ha-xi-bian-li-yi-ci-jiu-xing-by-tcan1993/), 能发现顺序和逆序计算hash的值应该是一样的,因此可以直接通过对比计算得到的hash值来判断是否是回文

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if ( head == null) return true;
        long hash1 = 0, hash2 = 0;
        long h = 1;
        // seed 是一个质数, 关于seed值的选取: 我们一般选质数的时候，都选比所有val大的质数，尽量避免哈希碰撞，这题我就随便取了个大点的。比如对仅包含英文字母的字符串哈希，我们通常选取131(字母z的ASCII为122)。如果叫我证明就为难我了，减小哈希冲突概率的方式还有双值哈希。一般哈希写法都是自己取模(模数较大，避免冲突)，而我偷懒让它溢出
        long seed = (long)(1e9 + 7);
        ListNode node = head;
        while( node != null){
            // arr[0]的sedd将会被计算n-1次即== arr[0]* seed^(n-1)
            hash1 = hash1 * seed + node.val;
            // seed幂次依次增加, hash2只是吧各项累加
            hash2 = hash2 + h * node.val;
            h = h * seed;
            node = node.next;
        }
        return hash1 == hash2;
    }
}
```

#### 递归做法:

在此记一个特别直观的递归动画[回文链表](https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-by-leetcode-solution/)——递归

```cpp
class Solution {
    public:
        bool isPalindrome(ListNode* head) {
            if ( head == nullptr ) return true;
            frontNode = head;
            return isRecurcive(frontNode);
        }

        bool isRecurcive(ListNode *currentNode){
            if ( currentNode != nullptr ){
                // 如果没到根节点就一直
                if ( !isRecurcive(currentNode->next) ) return false;
                // 
                if ( currentNode->val != frontNode->val ) return false;
                frontNode = frontNode->next;
            } 
            return true;
        }
    
    	// 换种常见的迭代写法:
        bool isRecurcive(ListNode *currentNode){
            // 终止条件写上面
            if ( currentNode == nullptr ) return true;
            if ( !isRecurcive(currentNode->next) ) return false;
            if ( currentNode->val != frontNode->val ) return false;
            
            // 具体每部递归中的内容写在下面
            frontNode = frontNode->next;
            return true;
        }
    private:
        ListNode* frontNode;
};
```

想法很酷, 但是性能并不是很好, 因此隐式地调用了栈, 其实还是有O(n)的空间复杂度



## 2020年10月24日

### [1024. 视频拼接](https://leetcode-cn.com/problems/video-stitching/)——1024程序员日



## 2020年10月25日

### [845. 数组中的最长山脉](https://leetcode-cn.com/problems/longest-mountain-in-array/)——中等题DP





## 2020年10月26日

### [1365. 有多少小于当前数字的数字](https://leetcode-cn.com/problems/how-many-numbers-are-smaller-than-the-current-number/)

> 一下又欠了好几天了，这次是道简单题， A了

```cpp
// 暴力
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        if (nums.empty()) return {};
        int len = nums.size();
        vector<int> ans(len, 0);
        for(int i = 0; i < len; i ++ ){
            int now = nums[i];
            for(int j = 0; j < len; j ++ ){
                if ( j == i ) continue;
                if ( nums[j] < now){
                    ans[i] ++ ;
                }
            }
        }
        return ans;
    }
};


// 计数排序, 时间复杂度O(n + k)
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
         vector<int> all_num(101, 0);
        int len = nums.size();
        for (int i = 0; i < len; i++) {
            int now = nums[i];
            all_num[now]++;
        }

        // 前缀和数组, 利用了nums值为有限100以内
        for (int i = 1; i < 101; i++) {
           all_num[i] = all_num[i] + all_num[i-1];
        }

        vector<int> ans(len, 0);
        for (int i = 0; i < len; i++) {
            // 这边需要特判0, 以及注意是小于关系, 因此这边取的是C-1的前缀和值
            ans[i] = nums[i] == 0 ? 0 : all_num[nums[i] - 1];
        }
        return ans;
    }
};


```

## 2020年10月27日

### [144. 二叉树的前序遍历](https://leetcode-cn.com/problems/binary-tree-preorder-traversal/)

> 常规题:树的遍历。

```cpp
// 递归写法
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == nullptr ) return {};
        vector<int> ans;
        preOrder(root, ans);
        return ans;
    }

    void preOrder(TreeNode *root, vector<int> &ans){
        if ( root == nullptr)return ;
        ans.push_back(root->val);
        preOrder(root->left, ans);
        preOrder(root->right, ans);
    }
};

// 迭代写法, 由于递归是隐式的调用了栈, 因此可以通过显性递归的来调用栈。由于遍历是：根->左->右,因此尝试分析这个过程后, 发现是左结点比右结点要先出栈, 因此先入栈的是右结点, 之后才是左结点
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == nullptr ) return {};
        vector<int> ans;
        stack<TreeNode *> st;
        st.push(root);
        TreeNode *node;
        while( !st.empty() ){
            node = st.top();
            st.pop();
            ans.push_back(node->val);
            // 先入栈右结点后入栈左结点
            if ( node->right){
                st.push(node->right);
            }
            if ( node->left ){
                st.push(node->left);
            }
        }
        return ans;
    }
};


// 官方题解, 直意可以实现先左后右的写法
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        if (root == nullptr ) return {};
        vector<int> ans;
        stack<TreeNode *> st;

        // st.push(root);  不能加
        TreeNode *node = root;
        while( !st.empty() || node != nullptr){
            while( node != nullptr){
                // 将左子树一探到底
                ans.push_back(node->val);
                st.push(node);
                node = node->left;
            }

            // 此时node == nullptr, 取结点开始向右遍历
            node = st.top();
            st.pop();
            node = node->right;
        }
        return ans;
    }
};
```



### [145. 二叉树的后序遍历](https://leetcode-cn.com/problems/binary-tree-postorder-traversal/)

```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        if (root == nullptr) return {};
        stack<TreeNode *>st;
        TreeNode *node = root;
        vector<int> ans;
        while( !st.empty() || node != nullptr){
            // 把右子树一探到底
            while( node != nullptr ){
                ans.emplace_back(node->val);
                st.push(node);
                node = node->right;
            }
            // 然后再开始探左子树
            node = st.top();
            st.pop();
            node = node->left;
        }
        // 注意这边需要反转
        reverse(ans.begin(), ans.end());
        return ans;
    }
};

```



### [94. 二叉树的中序遍历](https://leetcode-cn.com/problems/binary-tree-inorder-traversal/)

```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        if (root == nullptr ) return {};
        vector<int> ans;
        stack<TreeNode *> st;

        // st.push(root);
        TreeNode *node = root;
        while( !st.empty() || node != nullptr){
            while( node != nullptr){
                st.push(node);
                node = node->left;
            }
            // 此时node == nullptr
            node = st.top();
            st.pop();
            ans.push_back(node->val);
            node = node->right;
        }
        return ans;
    }
};

```

记录一下区别:

- 先序遍历: 把左子树一路探到底, 再右子树
- 后序遍历: 把右子树一路探到底, 再左子树, 但最后需要**将vector反转**
- 中序遍历: 跟先序遍历相同, 只不过vector.push_back的位置在遍历完左子树取父节点的时候push

迭代写法总结: https://leetcode-cn.com/problems/binary-tree-preorder-traversal/solution/tu-jie-er-cha-shu-de-si-chong-bian-li-by-z1m/



## 2020年10月28日

### [1207. 独一无二的出现次数](https://leetcode-cn.com/problems/unique-number-of-occurrences/)

```cpp

class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        unordered_map<int, int> mp;
        if (arr.empty()) return false;
        int len = arr.size();
        // 1.先用hash表存储各个数字出现的次数
        for (int i = 0; i < len; i++) {
           mp[arr[i]]++;
        }

        unordered_set<int> s;
        // 再遍历各个数字, 判断是否在s中出现
        for(unordered_map<int, int>::iterator it = mp.begin(); it != mp.end(); it++){
            if ( s.find(it->second) == s.end() ){
                s.insert(it->second);
            }
        }
        // 如果set中的个数和mp中的个数相同,则表示出现的数字都不相同
        return s.size() == mp.size();
    }
};
```

Java版本

```java
class Solution {
    public boolean uniqueOccurrences(int[] arr) {
        HashMap<Integer, Integer> mp = new HashMap<>();
        for(int i = 0; i < arr.length; i ++ ){
            mp.put( arr[i], mp.getOrDefault(arr[i], 0) + 1);
        }
        return mp.size() == new HashSet<>( mp.values()).size();
    }
}
```



## 2020年10月29日

### [129. 求根到叶子节点数字之和](https://leetcode-cn.com/problems/sum-root-to-leaf-numbers/)

> 考察树的遍历

```cpp
// 自己写的, 感觉确实比别人多绕了一点, 尤其是在进制的转换上, 想的是用string拼接再转数字, 太绕了..
class Solution {
private:
    vector<string> ans;
public:
    int sumNumbers(TreeNode* root) {
        if ( root == nullptr ) return 0;
        dfs(root, "");
        int sum = 0;
        for (auto &x: ans) {
           sum += stoi(x);
        }
        return sum;
    }

    void dfs(TreeNode *root, string s){
        // DFS递归终止条件, 碰到null节点
        if(root == nullptr) return;
        if (root->left == nullptr && root->right == nullptr) {
            // 判断是否是叶子节点
            ans.emplace_back(s + to_string( root->val));
            return ;
        }
        dfs(root->left, s + to_string( root->val ));
        dfs(root->right,s + to_string( root->val ));
    }
};

// 题解:
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if ( root == nullptr ) return 0;
        return dfs(root, 0);
    }
	
    int dfs(TreeNode *root, int s){
        if (root== nullptr) return s;
        int now = 10*s + root->val;
        if ( root->left == nullptr && root->right == nullptr ) {
            return now;
        }
        return dfs(root->left, now) + dfs(root->right, now);
    }
};



// BFS版本

class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (root == nullptr) return 0;
        return bfs(root);
    }

    int bfs(TreeNode* root){
        int sum = 0;
        queue<int> qn;
        queue<TreeNode*> qt;
        qn.push(root->val);
        qt.push(root);
        while( !qt.empty() ){
            int n = qn.front(); qn.pop();
            TreeNode *now = qt.front(); qt.pop();
            if ( now->left == nullptr && now->right == nullptr )
                sum += n;
            else{
                if ( now->left ){
                    qt.push(now->left);
                    qn.push(10* n + now->left->val);
                }
                if (now->right){
                    qt.push(now->right);
                    qn.push(10* n + now->right->val);
                }
            }
        }
        return sum;
    }
};

```

总结: 

- DFS套路的写法必须关注终止条件, 一般情况下是 1. 到了终止情况  2.满足条件
- DFS在进入递归的时候不一定要写判断能否进入下一次递归的if， 可以在下一层递归的开头进行终止；但是BFS在入栈前一定要进行if判断

题解:Java递归

```java
public int sumNumbers(TreeNode root) {
    return helper(root, 0);
}

public int helper(TreeNode root, int i){
    if (root == null) return 0;
    int temp = i * 10 + root.val;
    if (root.left == null && root.right == null)
        return temp;
    return helper(root.left, temp) + helper(root.right, temp);
}
```





## 2020年10月30日

### [463. 岛屿的周长](https://leetcode-cn.com/problems/island-perimeter/)

> 看到这种迷宫, 方阵, 数块题, 第一个想到的就是BFS... 可惜这题BFS并不好做, 这是道模拟题....当然DFS也是可以的.
>
> 计算连通块的周长, 即碰到水域或边界这条边就生效, 如下为模拟代码

```cpp
int dirx[] = {1, 0, -1, 0};
int diry[] = {0, 1, 0, -1};

class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        int height = grid.size();
        int width = grid[0].size();
        int ans = 0;
        for (int i = 0; i < height; i++) {
            for (int j = 0; j < width; j++) {
                if ( grid[i][j] == 1){
                    int cnt = 0;
                    for(int k = 0; k < 4; k ++ ){
                        int newx = i + dirx[k];
                        int newy = j + diry[k];
                        if ( newx < 0 || newy < 0 || newx >= height || newy >= width || grid[newx][newy] == 0){
                            cnt ++;
                        }
                    }
                    ans += cnt;
                }
            }
        }
        return ans;
    }
};

// DFS可以计算多个块的总周长
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
const int maxn = 105;
bool visited[maxn][maxn];
int dirx[] = {1, 0, -1, 0};
int diry[] = {0, 1, 0, -1};

class Solution {
private :
    vector<vector<int>> grid;
public: 
    int islandPerimeter(vector<vector<int>>& g) {
        grid = g;
        int ans = 0;
        for (int i = 0; i < grid.size(); i++) {
           for (int j = 0; j < grid[0].size(); j++) {
               if( visited[i][j] == false && grid[i][j] == true){
                   ans += dfs(i, j );
               }
              
           }
        }
        return ans;
    }

    int dfs(int x, int y){
        if ( x < 0 || y < 0 || x >= grid.size() || y >= grid[0].size()){
            return 1;
        }
        if ( visited[x][y] == true) return 0;
        visited[x][y] = true;
        int ans = 0;
        for (int i = 0; i < 4; i++) {
           int newx = x + dirx[i];
           int newy = y + diry[i];
           ans += dfs(newx, newy);
        }
        return ans;
    }
};

int main(){
    Solution s = Solution();
    vector< vector<int> >input = {{0,1,0,0},{1,1,1,0},{0,1,0,0},{1,1,0,0}};
    int ans = s.islandPerimeter(input);
    cout <<ans <<endl;
    return 0;
}

```

## 2020年10月31日

**[381. O(1) 时间插入、删除和获取随机元素 - 允许重复](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)**

先做中等题: [380. 常数时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/), 和381的区别在于如果容器中已有元素直接返回false

### [380. 常数时间插入、删除和获取随机元素](https://leetcode-cn.com/problems/insert-delete-getrandom-o1/)

insert，我们具有两个平均插入时间为 O(1)的选择：

- 哈希表：Java 中为 HashMap，Python 中为 dictionary, C++ 为map。
- 动态数组：Java 中为 ArrayList，Python 中为 list, C++中为vector。

`HashMap`可以实现平均`O(1)`的插入和删除(额外针对有KV情况来说; 链表也是O(1).)，但无法实现随机访问。
线性表:`ArrayList`可以实现随机访问。
因此，考虑将这两者结合来实现。



个人思考:

**线性表:**

- 要O(1)随机访问的效率, 则需要线性表, 或者是带key-value的hashmap(其实线性表的索引-value就是key-value), 都可以实现O(1)访问.
- getRandom 的思想是选择一个随机索引，然后使用该索引返回一个元素。而哈希表中没有索引，因此要获得真正的随机值，则要将哈希表中的键转换为列表的索引，因此通过这个能确认使用线性表

**hashmap:**
insert，我们具有两个平均插入时间为 O(1)的选择：

- 哈希表：Java 中为 HashMap，Python 中为 dictionary, C++ 为map。
- 链表
显然, LRU中hashmap+双向链表也是可以操作的。 只不过随机返回现有集合中的一项。每个元素应该有**相同的概率**被返回，这个要求O（1）并不满足。 因此这边可以使用hashmao + vector就能完成这道题的题目要求了

这题还有个需要注意的是: 删除任意索引元素需要线性时间，这里的解决方案是总是删除最后一个元素。

- 将要删除元素和最后一个元素交换。
- 将最后一个元素删除。

为此，必须在常数时间获取到要删除元素的索引，因此需要一个哈希表来存储值到索引的映射。

```cpp
class RandomizedSet {
public:
    // 存储元素的值
    vector<int> nums;
    // 记录每个元素对应在 nums 中的索引
    unordered_map<int,int> valToIndex;

    bool insert(int val) {
        // 若 val 已存在，不用再插入
        if ( valToIndex.find(val) != valToIndex.end() ) return false;
        // 若 val 不存在，插入到 nums 尾部，并记录 val 对应的索引值
        valToIndex[val] = nums.size();
        nums.push_back(val);
        return true;
    }

    bool remove(int val) {
        // 若 val 不存在，不用再删除
        if ( valToIndex.find(val) == valToIndex.end() ) return false;
        // 先拿到 val 的索引
        int index = valToIndex[val];
        // 将最后一个元素对应的索引修改为 index
        valToIndex[nums.back()] = index;
        // 在nums数组上将最后一个元素换到index的位置上去, 将要删除的元素放在最后
        swap(nums[index], nums.back());
        // 在数组中删除元素 val
        nums.pop_back();
        // 同时从map中删除该value对应索引key。注此处有一说明，之所以没有交换value，而是直接赋值的形式，是因为，数组内部元素在增删之后会再次索引对齐，在此情况下交换map中的索引没有意义。
        valToIndex.erase(val);
        return true;
    }

    int getRandom() {
        // 随机获取 nums 中的一个元素
        return nums[rand() % nums.size()];
    }
};
```

### [381. O(1) 时间插入、删除和获取随机元素 - 允许重复](https://leetcode-cn.com/problems/insert-delete-getrandom-o1-duplicates-allowed/)

> 跟上题最大的区别就在于容器中会出现重复的数字, 因此在用mp保存的时候需要用

在删除时，我们找出 val 出现的其中一个下标index, 将nums[index]与nums.back()交换. 随后将index在idx中去除(如果该key没有val的话需要erase该key, 方便find), 并将idx中last_val的索引`nums.size() - 1`改成index, 最后将nums最后一个元素val踢出

```cpp
class RandomizedCollection {
private:
    unordered_map<int, unordered_set<int> > idx;
    vector<int> nums;
public:
    RandomizedCollection() {
    }
    
    bool insert(int val) {
        // val对应的set中插入在nums中的索引值
        idx[val].insert(nums.size());
        // 更新待进来下一个元素的索引值
        nums.push_back(val);
        // 如果为1则表示之前没有val这个元素
        return idx[val].size() == 1;
    }
    
    // 主要分为两部分: 1.对val的处理, 2.对nums最后一个元素的处理
    bool remove(int val) {
        // 如果没有找到对应的key，表示该元素未出现过
        if ( idx.find(val) == idx.end() ) return false;
        // 找到容器中val对应的set中的第一个元素(在nums中的下标)
        int index = *(idx[val].begin());
        // 获得被替换, nums最后一个元素值
        int last_val = nums.back();
        // 2.1 把要删除的元素值(val对应index位置的值)换成最后一个元素值——换值
        nums[index] = last_val;
        // 1.1 删除val在set中的索引记录
        idx[val].erase(index);
        // 2.2 删除last_val原来的下标——nums最后一个索引
        idx[last_val].erase( nums.size() - 1);
        
        // ▲唯一一处insert需要判断是否真
        // 2.3 把最后一个元素的索引改成val的索引(如果本身不是最后一个元素的话, 如果本身就是最后一个, 那么自己就是需要删除的, 因此不需要insert)
        if ( index < nums.size() - 1 ){
            // 将val对应的下标给——换索引
            idx[last_val].insert(index);
        }
        // 1.2 如果已经val这个key没有value了, 就删除这个key, 目的是为了跟find()==end()配合
        if ( idx[val].empty() ){
            idx.erase(val);
        }
        
        // 1.3 删除index对应val
        nums.pop_back();
        return true;
    }
    
    int getRandom() {
        // 某个数所出现的比例所决于其在nums中出现的个数
        return nums[rand() % nums.size()];
    }
};
```

记录:

- mp.find找的是key
- void pop_back在没有元素时不会报错; erase同理, 如果val就是最后一个元素,则29和31行其实就重复删了
- 根据数量线性概率表达方式可以学一下`nums[rand() % nums.size()]`

## 2020年11月1日

### [140. 单词拆分 II](https://leetcode-cn.com/problems/word-break-ii/)

### [139. 单词拆分](https://leetcode-cn.com/problems/word-break/)

> 又是一道困难题, 决定等DP专题的时候再来补这题



刷道复习面经时候看的题： [93. 复原IP地址](https://leetcode-cn.com/problems/restore-ip-addresses/)

```cpp

class Solution {
private:
    vector<string> ans;
    
public:
    vector<string> restoreIpAddresses(string s) {
        if ( s.size() > 12 ) return {};
        // 第一个dot不可能在首位
        BackTracking(s, 0, 1);
        return ans;
    }


    // 当前string, 已有dot数, dot位置
    void BackTracking(string s, int dot_num, int dot_position){
        if( dot_num == 3){
            if ( islegalIP(s) ){
                ans.push_back(s);
            }
            return ;
        }
        //
        for (int i = dot_position; i < s.size(); i++) {
           s.insert(i, ".");
           // dot不可能连续， 需要为 + 2
           BackTracking(s, dot_num + 1, i + 2);
           // 从i位置删除一个字符
           s.erase(i, 1);
        }
    }
    
    bool islegalIP(string s){
        if ( s.size() > 12 + 3) return false;
        for (int i = 0; i < s.size(); i++) {
            // 遇到.前的数
            int tmp = 0;
            // 起点
            int j = i;
            while( i < s.size() && s[i] != '.'){
                tmp = tmp * 10 + s[i++] - '0' ;
                if( tmp > 255) return false;
            }
            // 不用+1, 因为此时i在dot的位置
            int delta = i - j;
            // 去前导0的情况
            if ( delta > 3 || ( delta == 3 && tmp < 100 ) || (delta == 2 && tmp < 10)){
                return false;
            }
        }
        return true;    
    }
};

```





## 2020年11月2日

### [349. 两个数组的交集](https://leetcode-cn.com/problems/intersection-of-two-arrays/)

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> ans;
        unordered_set<int> s;		// 帮助统计数是否在ans中出现过
        for (int i = 0; i < nums1.size(); i++) {
           vector<int>::iterator it = find(nums2.begin(), nums2.end(), nums1[i]);
           if ( it != nums2.end()){	// 判断该元素是否在nums2中也出现过
               if ( s.find(nums1[i]) == s.end() ){
                   s.insert(nums1[i]);
                   ans.push_back(nums1[i]);
               }
           }
        }
        return ans;
    }
};


// 纯使用STL algorithm破题
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        if( nums1.empty() || nums2.empty() ) return {};
        set<int> s1(nums1.begin(), nums1.end());
        set<int> s2(nums2.begin(), nums2.end());
        set<int> C;
        set_intersection(s1.begin(),s1.end(),s2.begin(),s2.end(),inserter( C , C.begin() ) );    /*取并集运算*/
        
        vector<int> ans(C.begin(), C.end());
        return ans;
    }
};
```

注意:

- set_intersection不能对unordered_set使用, 效果不正常
- `set<int> s1(nums1.begin(), nums1.end());`根据STL容器生成STL容器是可行的

## 2020年11月3日

### [941. 有效的山脉数组](https://leetcode-cn.com/problems/valid-mountain-array/)

```cpp
// 自己写的版本, 写的比较丑
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        if ( A.size() < 3 ) return false;
        int len = A.size();
        bool up2low = true;		// 记录是否有上坡到下坡的转变过程
        int changeTime = 0;		// 记录是否全升或全降
        for (int i = 1; i < len; i++) {
           int  j = i - 1;
           if (A[i] == A[j] ) return false;		// 如果出现相等, 根据定义必不是
           if ( A[i] < A[j] ){					// 出现了下坡
               up2low = false;
               changeTime +=  1;
           }
            // 如果已经是下坡了, 还出现上坡的情况, 则false
           if ( up2low == false && A[i] > A[j]) return false;
        }
		
        // 1. 如果没出现过下坡; 2.如果没出现过上坡
        return changeTime > 0 && changeTime != A.size() - 1;
    }
};

// 题解
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        int N = A.size();
        int i = 0;

        // 递增扫描
        while (i + 1 < N && A[i] < A[i + 1]) {
            i++;
        }

        // 最高点不能是数组的第一个位置或最后一个位置
        if (i == 0 || i == N - 1) {
            return false;
        }

        // 递减扫描
        while (i + 1 < N && A[i] > A[i + 1]) {
            i++;
        }

        return i == N - 1;
    }
};


// 比较美观的写法-双指针形式, 实际跟一个for一样
class Solution {
public:
    bool validMountainArray(vector<int>& A) {
        if ( A.size() < 3 ) return false;
        int len = A.size();
        int i = 0, j = len - 1;
        while( i+1 < len && A[i+1] > A[i] ){
            i ++;
        }
        while( j -1 > 0 && A[j-1] > A[j]){
            j --;
        }
        // 1. 如果是山脉数组, 则i和j都需要移动, 避免单调情况
        // 2. 山脉数组要求一个峰值, 即i==j
        return  i != 0 && j != len -1 && i == j ;
    }
};
```

## 2020年11月4日

### [57. 插入区间](https://leetcode-cn.com/problems/insert-interval/)——困难



## 2020年11月5日

### [127. 单词接龙](https://leetcode-cn.com/problems/word-ladder/)

所以这道题要解决两个问题：

- 图中的线是如何连在一起的
- 起点和终点的最短路径长度

```cpp
// 比较直观的BFS
class Solution {
public:
    int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
        unordered_set<string> s( wordList.begin(), wordList.end() );
        if ( s.find(endWord) == s.end() ) return 0;
        int times = 0;
        int wordLen = beginWord.size();

        unordered_set<string> visited;      // 判断是否已经遍历过
        queue<string> q;
        q.push(beginWord);
        visited.insert(beginWord);
        while( !q.empty() ){
            times += 1;
            int qlen = q.size();						// ▲KeyPoint
            for (int k = 0; k < qlen; k++) {            // 把该层的节点全部遍历完, 一次for算一次操作
                string nows = q.front(); q.pop();
                for (int i = 0; i < wordLen; i++) {
                    string change = nows;                   // 需要先赋值给一个新变量再修改每一位
                    for (int j = 0; j < 26; j++) {
                        change[i] = j + 'a';                // 修改某一位
                        if ( nows == endWord ) return times;// 如果是的话就直接返回当前操作次数
                        // 需要没访问过, 且在wordList中
                        if ( s.find(change) != s.end() && visited.find(change) == visited.end() ){     
                            q.push(change);
                            // 插入标记已访问过
                            visited.insert(change);
                        }
                    }
                }
            }
        }
        return 0;
    }
};
```

▲ 遍历某一层节点时, 竟然忘记存储了, 懵逼到怀疑自己会不会BFS....切记一定要把q.size()赋值给一个变量, 不然for时每一次的q.size()都是不一样的







## 2020年11月6日

### [1356. 根据数字二进制下 1 的数目排序](https://leetcode-cn.com/problems/sort-integers-by-the-number-of-1-bits/)

```cpp
typedef pair<int, int> pp;

class Solution {
private:
    int getOneNumber(int n){
        int ans = 0;
        while(n){
            if (n & 1) ans += 1;
            n >>= 1;
        }
        return ans;
    }
    
public:
    vector<int> sortByBits(vector<int>& arr) {
        if( arr.empty() )return{};
        priority_queue< pp, vector< pp >, greater< pp >  >q;
        for (auto &x: arr) {
           q.push( make_pair(getOneNumber(x), x) );
        }

        vector<int> ans;
        while(!q.empty()){
            ans.push_back(q.top().second); q.pop();
        }
        return ans;
    }

};


// 题解
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        vector<int> bit(10001, 0);
        for (int i = 1;i <= 10000; ++i) {
            bit[i] = bit[i>>1] + (i & 1);
        }
        // 匿名函数写法
        sort(arr.begin(),arr.end(),[&](int x,int y){
            if (bit[x] < bit[y]) {
                return true;
            }
            if (bit[x] > bit[y]) {
                return false;
            }
            return x < y;
        });
        return arr;
    }
};
```

此外统计二进制中1的个数还可以用判断是否为2的幂的方法

```cpp
int bitCount(int n) {
    int count = 0;
    while (n) {
        n &= (n - 1); // 清除最低位的1
        count++;
    }
    return count;
}
```

