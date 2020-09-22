---
layout: p
title: PAT冲冲冲——乙级
date: 2020-02-02 16:26:41
tags:
---

# PAT冲冲冲——乙级

>  [PAT甲级练习题 ——PAT (Advanced Level) Practice ](https://pintia.cn/problem-sets/994805342720868352/problems/type/7?page=1)
>  [PAT甲级(Advanced Level)真题](https://www.nowcoder.com/pat/5/problems?page=1)
>  [柳婼 の blog经验](https://www.liuchuo.net/archives/8091) 
>  [saquarius's blog](https://saquarius.com/2019/08/pat%E6%80%BB%E7%BB%93/)
>
>  [PAT甲级题目及分类总结](https://blog.csdn.net/a617976080/article/details/89676670)
>  [pat甲级题解目录](https://blog.csdn.net/richenyunqi/article/details/79958195 )

▲报名费256，可以刷[牛客网]( https://www.nowcoder.com/pat )的题来获得-50的优惠券，该练习场下的所有题目只要通过都算

## 乙级练习题

### [NowCoder数列]( https://www.nowcoder.com/pat/2/problem/250 )

> 没想到第二题就是考了个数据范围，由于0≤n≤1000000，所以F(n)必然比long long大，而判断3的倍数可表示为===> F(n) % 3 ---> (F(n-1)%3 + F(n-2)%3) % 3
>
> 求余运算性质：a = b+c -->  a%d = (b%d+c%d) % d

```c++
#include<stdio.h>
long long f[1000000+5];

int main(){
    int n,i;
    f[0]=7;
    f[1]=11;
    for(i=2;i<=1000000;i++){
        f[i]=(f[i-1]%3+f[i-2]%3)%3; 
    }
    
    while(scanf("%d",&n)!=EOF){
        if(f[n]!=0)
          printf("No\n");
         else
          printf("Yes\n"); 
    }
    return 0;
}
```

###  [养兔子](https://www.nowcoder.com/pat/2/problem/251) 

> 非常经典的斐波那契数列题

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 90+5
ll arr[N];
 
int main(){
    int n;
    arr[1] = 1; arr[2] = 2;
    for(int i = 3;i<=N;i++){
        arr[i] = arr[i-1] + arr[i-2];
    }
    while( cin >> n){
        cout << arr[n] <<endl;
    }
    return 0;
}
```

###  [客似云来](https://www.nowcoder.com/pat/2/problem/252) 

> 斐波那契数列的拓展题，将其中某个区间的值累加输出（需要特判是否为某个点）

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 80+5
ll arr[N];
 
int main(){
    int from, to;

    arr[1] = 1; arr[2] = 1;
    for(int i = 3;i<=N;i++){
        arr[i] = arr[i-1] + arr[i-2];
    }
    while( cin >> from >> to){
        ll tmp = 0 ;
        // 注意需要特判是否相等
        if (from == to) tmp = arr[from];
        else{
            for(int i=from; i<= to;i++){
                tmp += arr[i] ;
            }
        }
        cout << tmp <<endl;
    }
    return 0;
}
```

###  [斐波那契凤尾](https://www.nowcoder.com/pat/2/problem/253) 

> 一遍还挺难过的，有不少的坑点
>
> 1.虽然也是斐波那契数列，但是一定要注意前两项的取值
> 2.输出末尾的6位，那么就是%1e6，但是如果有前置0，需要补零，我是使用iomanip中的setw和setfill实现的
> 3.怎么判断超过6位：找出超过6位的n应该算比较简单的方法了吧

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 100000 + 5
ll arr[N];
 
int main(){
    int n;

    arr[1] = 1; arr[2] = 2;
    for(int i = 3;i<=N;i++){
        /*
        用来找到超过1e6的n
        arr[i] = (arr[i-1] + arr[i-2]);
        if (arr[i] > 1000000){
             cout << i << endl;
             break;
        }
        */
        arr[i] = (arr[i-1]%1000000 + arr[i-2]%1000000)%1000000;
    }
    while( cin >> n){
        if (n>= 30){
            cout << setw(6)<<setfill('0') << arr[n] <<endl;    
        }else{
            cout << arr[n] << endl;
        }
        
    }
    return 0;
}
```

Po个C的代码，使用printf的格式化输出的特性

```c
#include<stdio.h> 
int a[100005];
int main()
{
    int n;
    a[1]=1;
    a[2]=2;
    for(int i=3;i<=100000;i++)
      a[i]=(a[i-1]+a[i-2])%1000000;
    while(scanf("%d",&n)!=EOF)
    {
        if(n>=30)
          printf("%06d\n",a[n]);
        else
          printf("%d\n",a[n]);
    }
    return 0;
 } 
 
```

### [ 星际密码](https://www.nowcoder.com/pat/2/problem/254)

>说实话，一开始没看懂题，因为输入的n跟题目里提到的n不是同一个东西：矩阵X为[[1 1],[0 1]]，题目中的n是指多少次幂；而输入里的n是指有多少个密码，真正的n其实是第二行的输入Xi
>
>那么分析下思路，Xi=1时==1，Xi=2时==2，Xi=3时==3

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 100000 + 5
ll arr[N];
 

void initFib(){
    arr[1] = 1; arr[2] = 2;
    for(int i = 3;i<=N;i++){
        arr[i] = (arr[i-1]%10000 + arr[i-2]%10000)%10000;
    }
}

int main(){
    int n;
    int input[100+5];
    initFib();
    while( cin >> n){
        for (int i = 0; i < n; ++i){
            int tmp;
            cin >> tmp;
            cout << setw(4)<<setfill('0') << arr[tmp] ;    
        }
        cout << endl;
    }
    return 0;
}
```

![Fib](PAT冲冲冲——甲级/Fib.jpg)



### [ 母牛的故事](https://www.nowcoder.com/pat/2/problem/255)

> 变形的Fib，公式更新为$f(n) = f(n-1) + f(n-3)$
>
> 最主要的就是确定前几项，比较好的是样例都给出了2==>2,4==>4,5==>6，这样就比较好确定**每头小母牛从第四个年头开始，每年年初也生一头小母牛**到底是什么意思了

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 100000 + 5
ll arr[N];
 
void initFib(){
    arr[1] = 1; arr[2] = 2;arr[3]=3;arr[4]=4;
    for(int i = 5;i<=N;i++){
        arr[i] = arr[i-1] + arr[i-3];
    }
}

int main(){
    int n;
    initFib();
    while( cin >> n){
        cout << arr[n]<<endl;
    }
    return 0;
}
```

###  [童年生活二三事](https://www.nowcoder.com/pat/2/problem/256) 

> Fib数列的板子题，只不过需要理解一下

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

#define N 90 + 5
ll arr[N];
 

void initFib(){
    arr[1] = 1; arr[2] = 2;
    for(int i = 3;i<=N;i++){
        arr[i] = arr[i-1] + arr[i-2];
    }
}

int main(){
    int n;
    initFib();
    while( cin >> n){
        cout << arr[n]  << endl;
    }
    return 0;
}
```

###  [蜜蜂寻路](https://www.nowcoder.com/pat/2/problem/257) 

> 如果固定起点为1，计算到某个位置的走法数的话，跟走阶梯其实是一种思路，就是f(n) = f(n-1) + f(n-2)，即第n个位置的走法数=第n-1位置走法数 + 第n-2位置走法数

| 1->2 | 1    | 2->3 | 1    | 3->4 | 1    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1->3 | 2    | 2->4 | 2    | 3->5 | 2    |
| 1->4 | 3    | 2->5 | 3    | 3->6 | 3    |
| 1->5 | 5    | 2->6 | 5    | 3->7 | 5    |
| 1->6 | 8    | 2->7 | 8    | 3->8 | 8    |

可以发现其中的规律：走法数一直是Fib数列，而值为$fib(N_{to} - N_{from})$

▲但这题还有一个难点在于用例的范围(0 < a < b < 2^31)，即b-a~=2^32-1，为int最大范围，会导致的问题有两个

1. fib数列通常使用数组来存储，但是无法开个2^32大小的数组

   ==>滚动数组、递推（不用数组）

2. 输出的Fib(n)就远远超过long long了，因此要么模拟大数相加，那么另寻他法。

   ==>△还需要注意到的一点是,**输出数据结果范围是 [0, 2^63)**，那么意思是题目要求的输出其实是在long long 范围内的，那么就可以考虑截取输出了

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
// ll credit = 9.2e18;开的足够大能过样例就行
ll credit;

/**
 * 幂计算
 * @author mrli 2019-10-27
 * @param  n [less than 63]
 * @return   [long long type]
 */
ll pow(int n){
    ll ans = 1;
    for (int i = 0; i < n; ++i)
        ans *= 2;
    return ans;
}

ll Fib(int del){
    if (del == 1) return 1;
    else if(del == 2)  return 2;
    else{
        ll f1 = 1; ll f2=2;
        ll ans;
        for(int i = 3;i<=del;i++){
			// ans = f1 + f2 ;也过了
            ans = ( f1%(credit) + f2%(credit) )%credit;
            f1 = f2;
            f2 = ans;
        }
        return ans;
    }
}

int main(){
    int n;
    credit = pow(63)-1;
    cin >> n;
    while( n-- ){
        int from, to;
        cin >> from >> to;
        ll ans;

        cout << Fib(to-from) << endl;    
    }
    return 0;
}
```

看了别人的题解后,发现想多了。题目的意思是**得分点的输出值都在long long 范围内，而不是需要你把输出值压缩在long long范围内**，果然去掉 %运算也过了。

```c++
//蜜蜂寻路
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
#include <string>
#include <vector>
#include <queue>
#include <cmath>
using namespace std;
#define ms(x, n) memset(x,n,sizeof(x));
typedef  long long LL;
const LL maxn = 2147483648+5;
 
LL dp[3]; //滚动数组
int n, a, b;
LL solve()
{
    ms(dp, 0);
    dp[0] = 1, dp[1] = 1;
 
    for(int i = 2; i < b-a+1; i++)
        dp[i%3] = dp[0]+dp[1]+dp[2]-dp[i%3]; //即dp[i]=dp[i-1]+dp][i-2]
 
    LL ans = 0;
    for(int i = 0; i < 3; i++)
        ans = max(dp[i], ans);
    return ans;
}
 
int main()
{
    cin >> n;
    while(n--){
        cin >> a >> b;
        cout << solve() << endl; //从1到4和从2到5答案是一样的
    }
    return 0;
}
```



### [ 分数运算](https://www.nowcoder.com/pat/2/problem/261)

> 牛客网周赛做到过一次，感觉当时写的比现在的简单。难点在**使用GCD进行约分**

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
 
typedef long long ll;
 
/**
 * 辗转相除法,求最大公约数
 * @author mrli 2019-10-28
 * @param  a [description]
 * @param  b [description]
 * @return   [description]
 */
int gcd(int a, int b){
    if (b==0) return a;
    return gcd(b,a%b);
}
 
int main(){
    int a1,a2,b1,b2;
    char op3;
    while( scanf("%d/%d %d/%d %c", &a1, &a2, &b1, &b2, &op3) != EOF){
        int fenmu;
        int fenzi;
        if (op3 == '+'){
            fenmu = a2*b2;
            fenzi = a1*b2+a2*b1;
        }
        else if (op3 == '-'){
            fenmu = a2*b2;
            fenzi = a1*b2-a2*b1;
        }
        else if (op3 == '*'){
            fenmu = a2*b2;
            fenzi = a1*b1;
        }
        else{ //if (op3 == '*'){
            fenmu = a2*b1;
            fenzi = a1*b2;
        }
        // 找出最大公因子,约分
        int common = gcd(fenmu,fenzi);
        int res_zi = fenzi/common;
        int res_mu = fenmu/common;
        if ( res_mu * res_zi > 0)
            cout << abs(fenzi/common) << '/' << abs(fenmu/common) << endl;
        else
            cout << '-' <<abs(fenzi/common) << '/' << abs(fenmu/common) << endl;
    }
    return 0;
}
```



### [ 分解因数](https://www.nowcoder.com/pat/2/problem/262)

>  使用小学的短除法，我们很清楚的知道，要想求出它的每一个质因数，我们需要用质数去试除。`90`能被`2`整除，那就拿商继续除以`2`，除不尽就换`3`，一直到除到质数为止。基础代码框架类似判断质数，只是被判断的数字在过程中不断被除，最终循环结束的时候，那个被处理过的数字，就是最后一个质因数。 

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


int main(){
    int n;
    while( cin >> n){
        bool first = true;
        int tmpn = n;
        cout << n << " = " ;
        for (int i = 2; i <= sqrt(n); ++i){
            while ( tmpn%i == 0 && tmpn != i){
                tmpn /= i;
                cout << i << " * "; 
            }
        }
        cout << tmpn << endl;
    }
    return 0;
}
```



我的第一次做法：

> 一直TLE，估计这种的话，必须得线性筛，我搜了几个题解的结果也证明除了上述题解，其他的都是线性筛，上面的就比较巧妙

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


 bool isPrime( int num ){
     //两个较小数另外处理
     if (num==1) return false;
     if(num ==2|| num==3 )
         return true ;
     //不在6的倍数两侧的一定不是质数
     if(num %6!= 1&&num %6!= 5)
         return false;
     //在6的倍数两侧的也可能不是质数
     for(int i= 5;i <= sqrt( num); i+=6 )
         if(num %i== 0||num %(i+ 2)==0 )
             return false ;
     //排除所有，剩余的是质数
     return true;
}


int main(){
    int n;
    while( cin >> n){
        bool first = true;
        cout << n << " = " ;
        int tmpn = n;
        for(int i=2;i<=tmpn;i++){
            while ( isPrime(i) && n%i == 0){
                n /= i;
                if (first) {
                    first = false;
                    cout << i ; 
                }
                else cout <<  " * " << i ; 
                if (n==0) break;
                
            }
        }
        cout << endl;
    }
    return 0;
}
```





###  [因子个数](https://www.nowcoder.com/pat/2/problem/264) 

> 用到了上题的结论，**一个正整数总可以分解成一个或多个素数的积**，一开始理解错题目了，以为是所有因数的个数，其实是**因数的种数**，比如20->2是因为2，2，5；30->3是因为2，3，5。
>
> 因此这边还是需要素数判别，卡的点也在这，要用线性筛，其实就是上题的回答方式不同罢了

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    int n;
    while( cin >> n){
        int ans=0;
        int tmp = n;
        for (int i = 2; i <= n; ++i){
            bool first = true;
            while (tmp%i==0){
                tmp/=i;
                if (first){
                    first = !first;
                    ans ++;
                }
            }
        }
        cout << ans << endl;
    }
    return 0;
}
```



```
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    int n;
    while( cin >> n){
        int ans=0;
        int tmp = n;

        for (int i = 2; i <= sqrt(n); ++i){
            // bool first = true;
            if (tmp%i==0){
                while (tmp%i==0){
                   tmp/=i;
                }
                ans++;
            }
        }
        if (tmp!=1) ans++;
        cout << ans << endl;
    }
    return 0;
}
```



### [ skew数](https://www.nowcoder.com/pat/2/problem/266)

> 模拟题、实现一个幂运算

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


int pow2(int n){
    int ans=1;
    for (int i = 0; i < n; ++i)
        ans *= 2;
    return ans;
}

int main(){
    ios::sync_with_stdio(false);
    string s;
    while( cin >> s ){
        int size = s.size();
        int ans=0;
        for (int i = 0; i < size; ++i){
            /* code */
            if (s[i] == '2'){
                ans += 2*(pow2(size-i)-1);
                break;
            }
            ans += (s[i]-'0')*(pow2(size-i)-1);
        }
        cout << ans <<endl;
    }
    return 0;
}

```



### [ 一的个数](https://www.nowcoder.com/pat/2/problem/267)

> 非常基础的一道题：r进制表示

```cpp
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    int n, r;
    while( cin >> n >> r){
        int ans=0;
        while(n){
            if (n%r==1) {
                ans++;
            }
            n /= r;
        }
        cout << ans << endl;
    }
    return 0;
}

```

### [ 外星人的语言](https://www.nowcoder.com/pat/2/problem/268)

> r进制的拓展，需要将各位输出出来，由于是逆序的，所以需要一1.个栈来反转一下、或是2.使用string的反转功能

```c++
#include <iomanip>
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int main(){
    int n, r;
    stack<char> s;
    while( cin >> n >> r){
        int ans=0;
        while(n){
            // 0-9
            char c = n%r+48;
            if (n%r>=10) {
                // A-F
                c = n%r-10+65;
            }
            n /= r;
            // 不直接cout，而是存栈
            s.push(c);
        }
        while(!s.empty()){
            // 取出栈里的内容
            char c = s.top();
            s.pop();
            cout << c;
        }
        cout << endl;
    }
    return 0;
}

```



### [ 数位和](https://www.nowcoder.com/pat/2/problem/270)

> 代码为[一的个数](#一的个数)+[外星人的语言](#外星人的语言)的结合版。
>
> 题目要求，将数n，先表示成r进制的形式，然后再计算r进制下n的位数和，然后再用r进制来表示位数和的结果

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    // ACM比赛中cin,的使用比较耗时,因为默认的时候，cin与stdin总是保持同步的，使用这句可以使cin达到和scanf相差无几的输入效率。
    ios::sync_with_stdio(false);
    int n, r;
    while( cin >> n >> r){
        int ans=0;
        while(n){
            ans += n%r;
            n /= r;
        }

        stack <char> s;
        while(ans){
            // 0-9
            char c = ans%r+48;
            if (ans%r>=10) {
                // A-F
                c = ans%r-10+65;
            }
            ans /= r;
            // 不直接cout，而是存栈
            s.push(c);
        }
        while(!s.empty()){
            // 取出栈里的内容
            char c = s.top();
            s.pop();
            cout << c;
        }
        cout << endl;

    }
    return 0;
}

```



###  [进制回文数](https://www.nowcoder.com/pat/2/problem/272) 

> 还是r进制的拓展，
>
> 1.r需要用个2-16的循环
> 2.判断字符串的镜像对称s[i] != s[ssize-i-1]
> 3.踩了个坑,n每次都会被除到很小,因此需要用个临时变量来处理

```c++
#include <iomanip>
#include <bits/stdc++.h>

using namespace std;

typedef long long ll;

int main(){
    int N;
    while( cin >> N){
        bool yes=false;
        for (int r = 2; r <= 16; ++r){
            string s;
            int n = N;
            while(n){
                char c = n%r+48;
                if (n%r>=10) {
                    c = n%r-10+65;
                }
                n /= r;
                s += c;
            }

            // 检测出r进制变换时,n已经被除的很小了,因此需要用个临时变量
            // cout << r << "进制：" << endl;
            // for (int i = 0; i < s.size(); ++i)
            // {
            //     cout << s[i] << endl;
            //     /* code */
            // }
            bool mirror = true;
            int ssize = s.size();
            for (int i = 0; i <= ssize/2; ++i){
                if (s[i] != s[ssize-i-1]){
                    mirror = false;
                    break;
                }
            }
            if (mirror){
                 yes=true;
                 break;
            }
        }
        if (yes) cout << "Yes" << endl;
            else cout << "No" << endl;
    }
    return 0;
}

```



### [ 发邮件](https://www.nowcoder.com/pat/2/problem/274)

> 一道数学题，递推公式为$f(n) = (n-1)*[f(n-1)+f(n-2)]$
>
> 坑点:超出了int，需要用longlong

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

ll email(int n){
    if (n==2) return 1;
    else if(n==3) return 2;
    else{
        return (n-1)*(email(n-1)+email(n-2));
    }
}

int main(){
    ios::sync_with_stdio(false);
    int n;
    while( cin >> n ){
        cout << email(n) << endl;
    }
    return 0;
}

```



### [ 说反话 (20)](https://www.nowcoder.com/pat/2/problem/4075)

> 考查了：对行的读取、字符串的切割。
>
> 本来还以为考了个string的反转，结果比想象中的更简单一点

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


int main(){
    ios::sync_with_stdio(false);
    string s;

    while( getline(cin, s) ){
        stringstream strings;
        string tmps;
        strings << s;
        while( getline(strings, tmps, ' ') ){
            ss.push(tmps);
        }
        while(!ss.empty()){
            string couts = ss.top();
            ss.pop();
            if (!ss.empty())
                cout << couts <<' ';
            else
                cout << couts ;

        }
        cout <<endl;
    }
    return 0;
}

```

#### 补充——string的反转：

```c++
/*法一:使用string::reverse_iterator迭代器,直接用iterator会报错*/
for (string::reverse_iterator it=couts.rbegin(); it != couts.rend() ; ++it)
        cout << *it;

/*法二:使用algorithm算法中的reverse函数*/
// 会修改str中的内容
reverse(str.begin(),str.end());

/*法三:使用使用string.h中的strrev函数
△只能处理char[],不支持string类型
*/
char s[]="hello";
strrev(s);
cout<<s<<endl;

/*法四:自己编写*/
void Reverse(char *s,int n){
    for(int i=0,j=n-1;i<j;i++,j--){
        char c=s[i];
        s[i]=s[j];
        s[j]=c;
    }
}
```

###  [一元多项式求导 (25)](https://www.nowcoder.com/pat/2/problem/4076) 

>被读取方式卡了会
>
>这边有个坑点: 忽略了常数项的问题  
>比如 输入 2 0
>应该输出 0 0



```c++
#include<cstdio>  
int main()
{
	int exp,coe;
	bool flag=false;
	while(scanf("%d %d",&coe,&exp)!=EOF)
	{
		if(exp!=0)
		{
			if(flag) printf(" ");
			printf("%d %d",coe*exp,exp-1);
			flag=true;
		}
	}
	if(flag==false) printf("0 0\n");
	return 0;
}
```

别人的处理

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;


struct poly{
    int coef;
    int index;
}typedef poly;

int main(){
    ios::sync_with_stdio(false);

    int coef;
    int index;
    std::queue<poly> q;
    // scanf和getchar合用比较方便,cin再用getchar无效
    while(scanf("%d%d", &coef, &index) != EOF){
        if (index!=0){
            poly *p = new poly();
            p->coef = coef*index;
            p->index = index-1 ;
            q.push(*p);    
        }
        // 放最后能过,放最初的时候有些过不了
        if (getchar()=='\n') break;
    }

    if (q.size()==0) printf("0 0\n");
    else{
        while(!q.empty()){
            poly p = q.front();
            q.pop();
            if (p.coef != 0){
                if (q.empty())
                    printf("%d %d", p.coef, p.index );
                else
                    printf("%d %d ", p.coef, p.index );
            }
        }
    }
    return 0;
}

```

刷完了牛客网PAT乙级练习题的第一、第三页。大多都是些模拟题、简单题，相当于弱一点的蓝桥杯省赛。由于报名考的是甲级，所以就没继续做下去了...
