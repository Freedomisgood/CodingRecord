# PAT冲冲冲——甲级

>  [PAT甲10级练习题 ——PAT (Advanced Level) Practice ](https://pintia.cn/problem-sets/994805342720868352/problems/type/7?page=1)
>  [PAT甲级(Advanced Level)真题](https://www.nowcoder.com/pat/5/problems?page=1)
>  [柳婼 の blog经验](https://www.liuchuo.net/archives/8091) 
>  [saquarius's blog](https://saquarius.com/2019/08/pat%E6%80%BB%E7%BB%93/)
>
>  [PAT甲级题目及分类总结](https://blog.csdn.net/a617976080/article/details/89676670)
>  [pat甲级题解目录](https://blog.csdn.net/richenyunqi/article/details/84981078)
>
>  - [PAT考试经验总结（甲乙级均适用）~~想满分的请看这里！~~](https://blog.csdn.net/a845717607/article/details/88084747)——前人经验

▲报名费256，可以刷[牛客网]( https://www.nowcoder.com/pat )的题来获得-50的优惠券，该练习场下的所有题目只要通过都算

## 甲级练习题

> 由于甲级题目较多，也较难，因此决定还是将两者分开写两篇文章了。

### [   1001 A+B Format (20 分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805528788582400)

> 看似很简单的一道题，但坑点确实不少，一遍过挺难的

```c++
#include <iomanip>
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    ll m, n;
    // 考虑到每次都是取后三位，所以需要用栈来逆序输出
    stack<ll> s;
    while(scanf("%lld%lld",&m,&n) != EOF){
        ll res_ans = m + n;
        // 这边0得特判
        if (res_ans==0)printf("0");
        else if (res_ans<0){
            printf("-");
        }else ;
        ll ans = abs(res_ans);
        while( ans ){
            ll three = ans%1000;
            s.push(three);
            ans /= 1000;
        }
        bool first = true;
        // 逆序输出
        while(!s.empty()){
            ll n = s.top();
            s.pop();  
            // 首个三位不需要补零，其他的都需要补零
            if (first) {
                printf("%lld", n);
                first = !first;
            }
            else printf("%03lld", n);
            if (!s.empty()) printf(",");
        }
        printf("\n");
    }
    return 0;
}
```

### [1002  A+B for Polynomials](https://pintia.cn/problem-sets/994805342720868352/problems/994805526272000000)

> 模拟题，对我来说，又重新温习了遍Map的使用。
>
> 该题就一个坑点：系数为0的项不需要显示。

```c++
#include <iomanip>
#include <bits/stdc++.h>
#include <algorithm>

using namespace std;
typedef long long ll;

int main(){
    int m, n;
    std::map<int, float> mp;
    std::map<int, float>::iterator i;

    scanf("%d", &m);
    for (int i = 0; i < m; ++i){
        float coef;
        int exp;
        scanf("%d %f", &exp, &coef);
        mp[exp] = coef;
    }

    scanf("%d", &n);
    for (int j = 0; j < n; ++j){
        // 两个临时变量
        float coef;
        int exp;
        scanf("%d %f", &exp, &coef);
        i = mp.find(exp);
        if ( i != mp.end() ){
            float sum = i->second + coef;
            if ( abs( sum - 0) < 1e-6 ){
                // △坑点:如果系数为0,不显示
                mp.erase(exp);
            }else mp[exp] = sum;
        }else{
           mp[exp] = coef; 
        }
    }
    
    // 从小到大输出
    // for ( i = mp.begin(); i != mp.end(); ++i)
    printf("%d", mp.size());
    // 使用反向迭代器->从大到小输出
    for (std::map<int, float>::reverse_iterator i = mp.rbegin(); i != mp.rend(); ++i){
        printf(" %d %.1f", i->first, i->second);
    }
    printf("\n");

    return 0;
}
```





###  [ 1003  Emergency](https://pintia.cn/problem-sets/994805342720868352/problems/994805523835109376) 

作为一个城市紧急援救队的指挥者，你得到了一个国家的特殊地图。地图上分散着几座城市，城市间用道路连接着。每个城市援救队的数量以及两座城市之间每条道路的长度已经在地图上标出。当某些城市发生了突发事件，需要你的帮助时，你的工作是带领你的队伍尽快的赶到事发现场，与此同时，召集尽可能多的在路上的队伍。

**输入**

每个输入文件包含一个测试实例。每个实例的第一行有四个正整数：N(<= 500)是城市的个数（城市的编号从0到N-1），M是道路的个数，C1和C2分别是你现在所在的城市以及你必须去救援的城市。下一行有N个整数，第i个整数是第i个城市中救援队的数量。然后下面有M行，每行表示一条道路。每一行有三个整数c1,c2和L，分别表示道路连接的两个城市以及道路的长度。保证C1到C2之间存在至少一条路径。

**输出**

对于每个测试实例，在一行中输出两个数字：C1和C2之间不同的最短路径的个数，你能聚集起来的最多的救援队数量。

一行中的所有数字必须被一个空格分隔开，在每行的结尾不允许出现空格。

>  思路：本题是求起点到目标点的最短路径的数目，以及所有最短路径中点权的最大值，可用dijkstra算法 

```c++
#include <iomanip>
#include <bits/stdc++.h>
#include <algorithm>

#define INF 0x3f3f3f

using namespace std;
typedef long long ll;

int n, m, s, d;
const int maxn = 500+5;

/*关于起点为s的路径的变量(记录的都是以 起点s 为中心的量)：
pathl(path_length缩写)存储从起点到所有点的最短路径的边权(本例为起点城市到所有城市的最短路径的距离) ————最短路
maxv(max_hands缩写)存储从起点到其他点的全部最短路径中的点权最大值(本例为起点城市到其他城市的所有最短路径中 即人手最多的那条路径的人手数)  ———— 最短路中能获得最大权值的节点权值e.g.maxv[3]即s->3能获得的最大权值

▲核心思想是根据路径最短来更新的,所以即使v' < v, 但只要l' < l,那么也会更新。即@77代码处

pathc(path_count缩写)存储从起点到其他点的最短路径的数目； 
△根据题意,额外需要维护的值
*/
int pathc[maxn];
int pathl[maxn];
int maxv[maxn];

/* 图信息的变量 */
// e(edges):点间的边关系, 初始化默认为0
// visited:判断v是否被访问过
// 存放节点v权值的量, node_value
int e[maxn][maxn];
int visited[maxn];
int value[maxn];

/**
 * [dijkstra description]
 * dijkstra求最短路径的特点是探索当前节点->下个节点,边权值最小的将被当做下个节点
 * 最终可以找出节点s到所有节点的最短路径
 * 原理:根据初始点，挨个的把离初始点最近的点一个一个找到并加入集合,集合中所有的点的d[i]都是该点到初始点最短路径长度
 * @author mrli 2019-11-09
 */
void dijkstra(){
    /*
    这份dijkstra的思路为:
        第一次, for1先遍历选中一个节点v, 然后使用for2找到遍历, 找到与v路径最短的下一个节点v', then更新
        第二次, for1那么根据与下个节点路径最短的规则,还是会找到v', 因此minI=v', 然后再找v''
    第二种:
        先安排节点s, 设个while(1) 以外的节点, 保存当前节点v编号, 然后遍历,tmpv为需要更新的节点的编号
        区别在于 
        法一:当前的v未访问过,所以设置visited[v]=1,然后找下一个tmpv,在下次的while循环的时候再设置visited
        法二:当前的v已经是visited==1, 在找到tmpv后直接设置visited[tmpv]=1,
    */
    // 初始化数组
    fill(pathl, pathl+ maxn, INF);
    pathc[s] = 1;
    pathl[s] = 0;
    maxv[s] = value[s];
    while(1){
        /*找出本轮尚未确定最短路径的城市中，起点到剩余城市中，距离最小minl的那个城市minI。
        如果minl是无穷大，证明起点城市与剩余城市均不可达，即不连通；
        如果minI就是目标城市d，则表明已经确定起点城市到目标城市的最短路径，提前结束寻找。
        否则，将本轮能确定最短路径的城市minI设为已经处理好,v[minI]=1；
        */
        int minl = INF, minI = -1;
        // 以s节点为例, 先遍历边其他未访问过的节点,找到其中边权值最小的作为下一个访问节点
        // 首先第一个访问的肯定是s节点
        for (int i = 0; i < n; ++i){
            if (visited[i] == 1) continue;
            if (pathl[i] < minl){
                minl = pathl[i];
                minI = i;
            }
        }
        // 终止条件:
        // 1.在 dijkstra 算法里, 如果节点已经判断到终点了, 那么到终点的最短路径就已经被计算出来了,此时可以结束
        // 2.当前循环全是Visited == 1的节点,所有节点都被遍历过了, 循环结束
        if (minI == d || minl== INF) break;
        visited[minI] = 1;

        // minI节点->下个节点
        for (int i = 0; i < n; ++i){
            // 在未达、且可达的节点中考虑，否则continue
            if( visited[i] == 1 || e[minI][i] == 0) continue;
            // 当节点minI的最短路径 + 当前minI->下一个节点的边权值
            int tmpl = pathl[minI] + e[minI][i];
            int tmpv = value[i] + maxv[minI];
            // 判断是否要更新, 如果当前路径l小于之前的话,那么更新
            if(tmpl < pathl[i]){
                pathl[i] = tmpl;
                maxv[i] = tmpv;
                pathc[i] = pathc[minI];
            }
            // 如果长度是相等的,那么最大化Value
            else if (tmpl == pathl[i]){
                pathc[i] += pathc[minI];
                if (tmpv > maxv[i]) maxv[i] = tmpv;
            }
        }
    }
}

int main(){
    scanf("%d %d %d %d", &n, &m, &s, &d);
    for (int i = 0; i < n; ++i){
        scanf("%d", &value[i]);
    }

    for (int i = 0; i < m; ++i){
        int v1, v2, l;
        scanf("%d %d %d", &v1, &v2, &l);
        e[v1][v2] = l;
        e[v2][v1] = l;
    }
    dijkstra();
    // 最短的路径, 最大的权值
    printf("%d %d\n", pathc[d], maxv[d]);
    return 0;
}
```

[大佬的代码(带注释)]( https://blog.csdn.net/yeziand01/article/details/80716688 )

### [ 1004 Counting Leaves](https://pintia.cn/problem-sets/994805342720868352/problems/994805521431773184) 

一个家庭的层级结构经常被表现为一个家谱树。你的任务是统计这些家庭成员中谁没有孩子。

**输入**

每个输入文件包含一个测试实例。每个实例开始的一行包含N和M，N指树中的结点个数（0<N<100），M指非叶结点的个数。然后下面有M行，每行的格式如下：

ID K ID[1] ID[2] ...ID[K]

ID是一个两位数的数字，表示一个非叶结点。K表示其孩子的数量。随后是一个序列，序列中是该结点的孩子结点的两位数ID。为了简单起见，我们把根结点的ID固定为01。

**输出**

对于每个测试实例，你应该计算从根结点开始的每一层中没有孩子的家庭成员的个数。数字必须在一行内输出，用空格分隔，在每行结尾不能有多余的空格。

测试样例表示了一个只有两个结点的树，01是根结点，02是它仅有的孩子。因此在根结点01层级，没有叶节点。再下一层级，有一个叶结点。然后我们应该在一行内输出“0 1”。

> 节点带有孩子的信息用vector来模拟图中的邻接表写法，然后用BFS来实现遍历

```c++
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100+5;

// 类似邻接表的写法记录节点
std::vector<int> v[MAXN];
// 用来记录每一层的叶子节点数
int cnt[MAXN];
// 来计算层数
// level记录当前节点p的所在层数
int level[MAXN];
// maxlevel记录深度0-N
int maxlevel;


void bfs(){
    queue<int> q;
    // 设置起点，root==01节点
    q.push(1);
    while(!q.empty()){
        int p = q.front();
        q.pop();
        // 自己是个叶子节点
        if(v[p].size() == 0){
            cnt[level[p]] ++;
            maxlevel = max(level[p], maxlevel);
        }else{
            // 如果当前节点p不是叶子节点， 则继续向其叶子节点扩展
            for (int i = 0; i < v[p].size() ; ++i){
                q.push(v[p][i]);
                // p节点的叶子节点的层数为p节点层数+1
                level[v[p][i]] = level[p] + 1;
            }   
        }
    }
}

int main(){
    int N, M;
    cin >> N >> M;
    while(M--){
        int parent, num;
        cin >> parent >> num;
        // 类似邻接表， parent记录了其孩子
        for (int i = 0; i < num; ++i){
            int tmp;
            cin >> tmp;
            v[parent].emplace_back(tmp);
        }
    }
    bfs();
    // 0为第一层
    for (int i = 0; i <= maxlevel; ++i){
        if(i==0) cout << cnt[i];
        else cout << " " << cnt[i] << endl;
    }
    return 0;
}
```

### [1005 Spell It Right](https://pintia.cn/problem-sets/994805342720868352/problems/994805519074574336)

> 感觉突然来了一道放水题，就纯模拟
>
> 坑点：全0的时候特判为zero

```c++
#include <bits/stdc++.h>
#include<algorithm>
using namespace std;

#define maxn 100 + 5
int arr[maxn];
string num[10] = {"zero" ,"one" ,"two" ,"three", "four" ,"five", "six", "seven", "eight", "nine"};

int main() {
    int sum = 0;
    char ch = getchar();
    stack<int> s;

    while( ch != '\n' ){
        int tmp = ch - '0';
        sum += tmp;
        ch = getchar();
    }
    // 坑点,需要特判0
    if(sum==0){
        printf("zero\n");
    }else{
        while(sum){
            int ge = sum %10;
            sum /= 10;
            s.push(ge);
        }
        bool first = true;
        while(!s.empty()){
            int ans = s.top();
            s.pop();
            if (first)  {
                printf("%s", num[ans].c_str());
                first = !first;
            }
            else printf(" %s", num[ans].c_str());
        }
    }

    printf("\n");
    return 0;
}
```

▲小结一下: 每次用while来取位的时候，必须先判断while(xxx)中的xxx是否初始就为0



###  [1006 Sign In and Sign Out (25 分)]( https://pintia.cn/problem-sets/994805342720868352/problems/994805516654460928 )

> 更加简单的模拟题，由于string的比较特性可以直接用来比较时间，所以处理很方便
>
> △学会使用algorithm里的sort能省很多时间
>
> ▲比较运算符<重载、或是编写外部比较函数，都会按照return里为true的逻辑排序,如first.xxx < second.xxx那么就是从小到大

```c++
#include <bits/stdc++.h>
#include<algorithm>
using namespace std;

struct record{
    string id;
    string intime;
    string outtime;
}typedef rc;

// 按照进入的时间排序, 从小到大
bool cmpin(const rc& f, const rc& s){
    return f.intime < s.intime;
}

// 按照出去的时间排序, 从大到小
bool cmpout(const rc& f, const rc& s){
    return f.outtime > s.outtime;
}

int main() {
    int T;
    std::vector<rc> v;
    std::vector<rc>::iterator it;
    cin >> T;
    while(T--){
        rc* p = new rc();
        cin >> p->id >> p->intime >> p->outtime;
        v.emplace_back(*p);
    }

    sort(v.begin(), v.end(), cmpin);
    cout << v.begin()->id << " ";

    sort(v.begin(), v.end(), cmpout);
    cout << v.begin()->id <<endl;
    // for(it = v.begin(); it!= v.end(); it++){
    //     cout << it->id << " "<<  it->intime <<" "<< it->outtime << endl;
    // }
    return 0;
}
```

[C++中sort的比较函数写法](https://blog.csdn.net/erd8888/article/details/88795843)

 注意：比较函数必须写在类外部（全局区域）或声明为静态函数 

###  [ 1007  Maximum Subsequence Sum](https://pintia.cn/problem-sets/994805342720868352/problems/994805514284679168) 

> Dp, 最大公共子串

```

```



###  [ 1008  Elevator](https://pintia.cn/problem-sets/994805342720868352/problems/994805511923286016) 

> 模拟

```c++
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100+5;
int order[MAXN];

int main(){
    int N;
    // 所有用时
    int t=0;
    // 上一个位置
    int ptr=0;
    cin >> N;
    for(int i = 0; i<N; i++){
        cin >> order[i];        
    }

    for(int i = 0; i<N; i++){
        int res = order[i] - ptr;
        ptr = order[i];
        if(res>0) t+= 6*res + 5;
        else t += 4*abs(res) + 5;
    }
    
    cout << t << endl;
    return 0;
}
```

### [ 1009  Product of Polynomials](https://pintia.cn/problem-sets/994805342720868352/problems/994805509540921344)

> 模拟题

### [ 1010 Radix](https://pintia.cn/problem-sets/994805342720868352/problems/994805507225665536)

> 模拟题

```c++
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100+5;
int order[MAXN];

int main(){
    int N1, N2, tag, radix;
    
    


    cout <<"Impossible" <<endl;
    return 0;
}
```



###  [ 1011  World Cup Betting](https://pintia.cn/problem-sets/994805342720868352/problems/994805504927186944) 

> 模拟题

```c++
#include <bits/stdc++.h>
#include<algorithm>
using namespace std;

struct games{
    double one, two, three;
}typedef Gm;

int main() {
    int T=3;
    double win=1;
    std::vector<int> v;

    while(T--){
        double arr[3];
        int maxi = 0;

        scanf("%lf%lf%lf",&arr[0], &arr[1], &arr[2]);
        for (int i = 1; i < 3; ++i){
            if (arr[i] > arr[maxi]){
                maxi = i;
            }
        }
        v.emplace_back(maxi);
        win *= arr[maxi];
    }
    
    std::vector<int>::iterator it;
    for(it= v.begin(); it!= v.end(); it++){
        switch(*it){
            case 0:
                printf("W ");
                break;
            case 1:
                printf("T ");
                break;
            case 2:
                printf("L ");
                break;
        }

    }
    printf("%.2lf\n", (win*0.65 - 1)*2 );
    return 0;
}


```







###  [ 1012  The Best Rank](https://pintia.cn/problem-sets/994805342720868352/problems/994805502658068480) 

```c++
#include <bits/stdc++.h>
#include<algorithm>
using namespace std;
int N, M;

struct Student{
    int id;
    // c m e a
    int g[4], r[4];
    char c[4] = {'A', 'C', 'M', 'E'};
}typedef stu;

std::vector<stu> v;

int num=0;

bool cmp(stu &f1 , stu &f2){
    return f1.g[num] > f2.g[num];
}

void getRank(){

    for (int j = 0; j < 4; ++j){
        sort(v.begin(), v.end(), cmp);
        for (int i = 0; i < N; ++i){
            v.at(i).r[num] = i+1;
        }
        num++;
    }
}


void maxRank(const stu *s){
    int best_rank = 0;
    for (int i = 1; i <4; i++){
        if(s->r[i] < s->r[best_rank]) best_rank = i;
    }
    cout << s->r[best_rank] << " " << s->c[best_rank] << endl;
}

int main() {
    cin >> N >>M;
    // scanf("%d%d", &N, &M);

    for (int i = 0; i < N; ++i){
        stu *s = new stu();
        cin >> s->id >> s->g[1] >> s->g[2] >> s->g[3]; 
        s->g[0] = (s->g[1] + s->g[2]+ s->g[3])/3; 
        v.emplace_back(*s);
    }

    getRank();

    for (int i = 0; i < M; ++i){
        int tmpid;
        bool find=false;
        cin >> tmpid;
        for (int i = 0; i < N; ++i){
            if ( v.at(i).id == tmpid) {
                maxRank(&v.at(i));
                find=true;
            }
        }
        if (!find) cout << "N/A" << endl;
    }
    return 0;
}


```



### [ 1013  Battle Over Cities](https://pintia.cn/problem-sets/994805342720868352/problems/994805500414115840)

In:给出n个城市，城市间有m条路，k个要检查的城市

Out:假如被检查的城市ki被攻占，则所有与Ki相关的路线全部瘫痪，要使其他城市保持连通，至少需要修缮多少条路?即 **删除图的一个节点，是其他节点成为连通图，至少需要添加多少条线**

解法一:[图的遍历:DFS计算连通分量数目]( https://blog.csdn.net/whl_program/article/details/77627856 )==> 计算出连通分量数N。如果想要构成连通图，那么需要添加**res=N-1**条线，即最少需要N-1条线。

```c++
#include <iomanip>
#include <bits/stdc++.h>
#include <algorithm>

#define SIZE 1001
using namespace std;

int p[SIZE][SIZE];
bool visit[SIZE];

int n;  // 顶点数

// 找到node下的所有连通节点,把其标记为true
void dfs(int node){
    visit[node] = true;
    for (int i = 1; i <= n; ++i){
        if ( visit[i] == false && p[node][i] == 1){
            dfs(i);
        }
    }
}

int main(int argc, char const *argv[])
{
    // 边数和case数
    int m ,k;
    cin >> n >> m >> k;
    for (int i = 0; i < m; ++i){
        int u, v;
        cin >> u >> v;
        p[u][v] = p[v][u] = 1;
    }

    // case K
    for (int i = 0; i < k; ++i){
        int cnt=0;
        int tmp;
        // 每次都得重置visit 即所有城市未被遍历
        fill(visit, visit+SIZE, false);
        cin >> tmp;
        visit[tmp] = true;	// 不让tmp进入考虑
        for (int j = 1; j <= n; ++j){
            if (visit[j] == false){
                dfs(j); 
                cnt ++;
            }
        }
        cout << cnt-1 << endl;
    }
    return 0;
}
```

解法二:无向图的连通性，可以考虑[并查集]( https://blog.csdn.net/titi2018815/article/details/90313319 )==> 但是需要注意最后结果的处理，并查集后可以知道现在的图分成了几块，但是有一块肯定是被占领的那一个城市，所以结果记得减去这一块，还有，两块地图联通只需要修建一条道路。综上所述，**res=图的块数-2**； 

```c++
#include<stdio.h>
#include<algorithm>
#include<vector>
using namespace std;
 
const int N=1010;
 
int f[N];   //存储boss；用来计算有几个连通分量的
int c;
int n,m,k;
vector<pair<int,int> >a;//存储城市之间的连接(边)；


int getf(int v)//递归查找boss；
{
    if(f[v]==v)
        return v;
    else
        return f[v]=getf(f[v]);
}
 
void merge(int u,int v)//合并；
{
    int t1=getf(u);
    int t2=getf(v);
    if(t1!=t2)//boss节点不同就合并；
    {
        f[t2]=t1;
    }
}
 
void init()//初始化；
{
    for(int i=1; i<=n; i++)
    {
        f[i]=i;//自己的boss是自己；
    }
}
 

// 图被分成了2部分:
// 1.与城市X相连的点(不会进行merge)、 X城市本身 (sum的两个组成部分)
// 2.与X不相连的(会进行merge)
int pan(int x){
    // x为被占领的城市
    int sum=0;
    for(int i=0; i<a.size(); i++){
        if((a[i].first!=x) && (a[i].second!=x))//和城市x相连的路全部断掉；
            merge(a[i].first,a[i].second);//不是和城市x相连的就合并；
    }

    //查看整个图分成了几块；
    for(int i=1; i<=n; i++){
        if(f[i]==i)
            sum++;
    }
    /**
     *  结果记得处理； 
     *  n个节点,需要n-1条边才能构成连通分量;
     *  △还有节点x自己得去掉,这也是跟1的区别(会多数个本身);
     */
    return sum-2;
}//除去被占领的城市，两个城市之间只需要一条路连接；
 
int main()
{
    scanf("%d%d%d",&n,&m,&k);
    a.resize(m);//设置容器的大小；
    for(int i=0; i<m; i++){
        int aa,bb;
        scanf("%d%d",&aa,&bb);
        // a[i]=make_pair(aa,bb);
        a.push_back(make_pair(aa,bb));
    }

    for(int i=1; i<=k; i++){
        // c为被占领的城市
        scanf("%d",&c);
        init();
        int res= pan(c);
        printf("%d\n",res);
    }
    return 0;
}
```



###  [ 1014  Waiting in Line](https://pintia.cn/problem-sets/994805342720868352/problems/994805498207911936) 

> 队列queue的模拟操作，分两部分解决，一部分是在黄线中的M*N个人，直接进行操作，另一部分黄线外的人需要一个个判断，哪个窗口目前是最少的，然后对该窗口进行更新

```c++
#include <bits/stdc++.h>
#include <queue>
using namespace std;

struct windows{
    // 前者为队首顾客出队时间：为了计算让黄线外的人可以计算出哪一个队列先空出人来
    // 后者为队尾顾客出队时间：为了计算入队后加上自己本身的taketime可以计算出自己何时才能被服务完毕
    //     以及得知自己是不是需要被Sorry(如果前一个人服务结束时间超过17:00，自己当前入队的人就是sorry)
    int start_pop_time, end_pop_time;
    // 该窗口排队的人
    queue<int> q;
}typedef win;

int main(){
    // n个窗口， m个黄线内顾客， 总共k个顾客， 求解q个询问
    int n, m, k, q;
    scanf("%d%d%d%d", &n, &m, &k, &q);
    std::vector<int> taketime(k+1);           // 每个cos需要的处理时间
    std::vector<int> quiz(k+1);               // 测验
    std::vector<bool> sorry(k+1, false);      // 是否能被服务
    std::vector<win> window(n+1);             // 窗口
    for(int i=1;i<=k;i++)scanf("%d", &taketime[i]);
    // 解答
    // 指针，当前新被处理的人， 从1号顾客开始
    int index=1;
    // 如果总人数正好在m*n范围内
    for (int i = 1; i <= m; ++i){
        for (int j = 1; j <= n; ++j){
            if (index <= k){
                window[j].q.push(taketime[index]);
                // 如果该窗口处理当前黄线内排队的人已经有大于540的了，那么新加入的必sorry
                if (window[j].end_pop_time>=(17-8)*60) sorry[index] = true;
                // 当前窗口的结束时间更新规则为： 已有的结束时间 + 当前顾客index的服务时间
                window[j].end_pop_time += taketime[index];
                // 第一个pop的时间需要特殊处理一下
                if (i==1) window[j].start_pop_time = window[j].end_pop_time;
                // 在这种情况下， 顾客的结束时间就是该窗口的end_time
                quiz[index] = window[j].end_pop_time;
                // 处理下一个顾客
                index++;
            }
        }
    }

    // 如果k>m*n，那么在这边处理 剩下的index~k位顾客，此时需要不断更新window安排顾客进栈
    while( index <= k){
        int min_time = window[1].start_pop_time, min_win = 1;
        // 黄线外顾客要找到一个对他来说最短的窗口， 编号小的优先
        for (int i = 2; i <= n; ++i){
            if ( window[i].start_pop_time <= min_time){
                min_time = window[i].start_pop_time;
                min_win = i;
            }
        }
        // 将最短窗口的队首顾客出队
        window[min_win].q.pop();
        // 黄线外第一个顾客入队
        window[min_win].q.push(taketime[index]);
        // 更新队首的顾客结束时间
        window[min_win].start_pop_time += window[min_win].q.front();
        // 如果该窗口处理当前黄线内排队的人已经有大于540的了，那么新加入的必sorry
        if (window[min_win].end_pop_time>=(17-8)*60) sorry[index] = true;
        // 当前窗口的结束时间更新为： 已有的时间 + 新进入的顾客的处理时间
        window[min_win].end_pop_time += taketime[index];
        // 在这种情况下， 顾客的结束时间就是该窗口的end_pop_time
        quiz[index] = window[min_win].end_pop_time;
        // 处理下一个顾客
        index++;
    }
    // 回答问题
    for(int i = 1; i <= q; i++) {
        int query, ans;
        scanf("%d", &query);
        ans = quiz[query];
        // 先判断是否sorry， 如果非sorry， 那么就有服务结束时间
        if(sorry[query] == true) printf("Sorry\n");
        // 规范输出结果
        else printf("%02d:%02d\n",(ans + 8*60) / 60, (ans + 8*60) % 60);
    }
    return 0;
}

```

### [1016 Phone Bills](https://pintia.cn/problem-sets/994805342720868352/problems/994805493648703488)

```c++

#include <bits/stdc++.h>
using namespace std;

int main(){
    // 写法一
    string date;
    cin >> date;
    const char *s = date.data();
    int month, day, hour, minute;
    sscanf(s, "%d:%d:%d:%d", &month, &day, &hour, &minute);
    cout << month << "\t"<<day<<"\t"<<hour<<"\t"<<minute<< endl;
    // 写法二
    string date;
    cin >> date;
    stringstream ss(date);
    string month, day, hour, minute;
    getline(ss, month, ':');
    cout << month<<endl;
    getline(ss, day, ':');
    cout << day<<endl;
    getline(ss, hour, ':');
    cout << hour<<endl;
    getline(ss, minute, ':');
    cout << minute<<endl;
    return 0;
}
```

### [1017 Queueing at Bank](https://pintia.cn/problem-sets/994805342720868352/problems/994805491530579968)

> 1. 将达到时间换算成秒（这样可以避免小数），我这里将到达时间以开门时间（8点）为0值，来早的即为负数（绝对值为等待时间），然后进行排序。
> 2. 判断有效人数是否大于0，不是则提前输出0.0（保留一位小数！！）
> 3. 设置windows[k]为窗口可以处理下一个客户的时间，默认值为0



###  [ 1018  Public Bike Management](https://pintia.cn/problem-sets/994805342720868352/problems/994805489282433024) 





###  [ 1019  General Palindromic Number](https://pintia.cn/problem-sets/994805342720868352/problems/994805487143337984) 



### [ 1020  Tree Traversals](https://pintia.cn/problem-sets/994805342720868352/problems/994805485033603072)

> 考察树的遍历。二叉树的前序、中序、后序遍历需要用到栈（**递归**的过程也就是一个栈）(DFS)，层次遍历需要借助**队列**这个数据结构==>(BFS)。
>
> 中序的结构的特点是：**左子树+根结点+右子树**。
> 而后序结构的特点是：**左子树+右子树+根结点**。
>
> 解题思路:后序(postOrder)和先序(preOrder)遍历提供**根节点位置**，然后再中序(inOrder)序列中区分出**左子树和右子树**，递归建树，然后BFS层序遍历。

[柳婼Code](https://blog.csdn.net/CV_Jason/article/details/81394447)

> 二叉树利用数组来完成, 未使用结构体

```C++
#include <iostream>
#include <vector>
using namespace std;
vector<int> post, in, level(100000, -1);

void pre(int root, int start, int end, int index) {
	cout<<"root = "<<root<<" start"<<start<<" end="<<end<<" index="<<index<<endl; 
    if(start > end) return ;
    int i = start;
    // 通过后序根节点找到中序根节点的索引 
    while(i < end && in[i] != post[root]) i++;
    // 对于后序遍历，最后一个结点是根节点 
    level[index] = post[root];
    // 【这段递归是本代码的亮点所在】
	// (root -(end - i + 1)) 后序root地址 - (中序左子树长度)，得到下一次的左子树的后序root地址 
    pre(root - (end - i + 1), start, i - 1, 2 * index + 1);
    // root - 1 后序root地址 左邻接点右子树的根 
    pre(root - 1, i + 1, end, 2 * index + 2);
}

int main() {
    int n, cnt = 0;
    scanf("%d", &n);
    // 预置n个结点 
    post.resize(n);
    in.resize(n);
    for(int i = 0; i < n; i++) scanf("%d", &post[i]);
    for(int i = 0; i < n; i++) scanf("%d", &in[i]);
    pre(n-1, 0, n-1, 0);
    for(int i = 0; i < level.size(); i++) {
    	//  
        if (level[i] != -1) {
            if (cnt != 0) printf(" ");
            printf("%d", level[i]);
            cnt++;
        }
        if (cnt == n) break;
    }
    return 0;
}

```



My

```c++

#include <bits/stdc++.h>
using namespace std;
const int MAXN = 50;

int post[MAXN], in[MAXN];
int n;
int num;    


struct Node{
    int v;
    Node *lchild;
    Node *rchild;
}typedef Node;

/**
 * [根据中序和先序遍历构建树, 参数全为索引]
 * @author mrli 2020-01-28
 * @param  postl [后序左边界]
 * @param  postr [后序右边界]
 * @param  inl   [中序左边界]
 * @param  inr   [中序右边界]
 * @return       [节点]
 */
Node *createTree(int postl, int postr, int inl, int inr){
    // 先序左指针>右指针
    if (postl>postr) return NULL;
    int k;
    // 找到根节点后序的索引值
    for (k = 0; k <= inr; ++k)
        if (in[k] == post[postr]) break;
    // 分界线
    int numLeft = k - inl;
    Node* root = new Node();
    root->v = post[postr];
    root->lchild = createTree(postl, postl+numLeft-1, inl, k-1);
    root->rchild = createTree(postl+numLeft, postr-1, k+1, inr);

    return root;
}

// 对numLeft定义进行了修改
Node *createTree(int postl, int postr, int inl, int inr){
    // 先序左指针>右指针
    if (postl>postr) return NULL;
    int k;
    // 找到根节点中序的索引值: 后序遍历的最后一个为根节点,由于都是不同的数,所以找到值相等的,
    // 此时k即为中序遍历中根的索引值
    for (k = 0; k <= inr; ++k)
        if (in[k] == post[postr]) break;
    // 计算左子树节点个数X ==> 找到分界线, postl+numLeft-1为后序的左子树, post+numLeft为后序右子树.
    // 真正意义上的左子树节点个数numLeft = k - inl - 1
    int numLeft = k - inl -1;
    Node* root = new Node();
    root->v = post[postr];
    // 后序遍历访问左子树时,左边界不变认为postl, 右边界变为postr+numLeft-1,访问右子树时,左边界为postl+numLeft, 右边界为postr-1(去掉了此轮的根节点)
    // postl + numleft 也为第二个根节点索引值
    // k为中序遍历节点的根, 所以访问左子树时左边界改为k-1, 右边界为k+1;访问右子树时左边界为k+1,右边界为inr
    root->lchild = createTree(postl, postl+numLeft, inl, k-1);
    root->rchild = createTree(postl+numLeft+1, postr-1, k+1, inr);

    return root;
}

void BFS(Node* tree){
    queue<Node*> q;
    q.push(tree);
    while(!q.empty()){
        Node* now = q.front();
        q.pop();
        cout << now->v;
        num ++;
        if (num < n) cout <<" ";
        if (now->lchild != NULL) q.push(now->lchild);
        if (now->rchild != NULL) q.push(now->rchild);
    }
}

int main(int argc, char const *argv[])
{
    cin >> n;
    for (int i = 0; i < n; ++i)
        cin >> post[i];
    for (int i = 0; i < n; ++i)
        cin >> in[i];
    Node* tree = createTree(0, n-1, 0, n-1);
    BFS(tree);
    return 0;
}
```

#### [Codeup问题 A: 复原二叉树](http://codeup.cn/problem.php?cid=100000611&pid=0)

```C++
#include <bits/stdc++.h>
using namespace std;
const int MAX = 1e3 + 5;
string pre;
string in;

struct Node{
    char v;
    Node* lc, *rc;
};

Node *createTree(int preL, int preR, int inL, int inR){
    if (preL > preR) return NULL;
    Node *root = new Node();
    root->v = pre[preL];        // 先序找到根节点
    int k;							// 根节点在中序遍历中的
    for (k = inL; k < inR; ++k){	// ▲千万不要多Int会再声明成局部变量
        if (in[k] == pre[preL])   // 找到根节点
            break;
    }

    int numLeft = k-inL;    // 由于不包括K, 所以不需要+1
    // 左子树继续
    root->lc = createTree(preL+1, preL+ numLeft, inL, k-1);
    // 右子树
    root->rc = createTree(preL+numLeft+1, preR, k+1, inR);
    return root;
}

void postOrder(Node *root){
    if(root == NULL) return ;

    postOrder(root->lc);
    postOrder(root->rc);
    cout << root->v;
}

int main(int argc, char const *argv[]){
    while(cin >> pre >> in){
        Node *root = createTree(0, pre.size()-1, 0, in.size()-1);
        postOrder(root);
        cout << endl;        
    }
    return 0;
}
```

▲前提都是不同元素！



#### [Codeup问题 B: 二叉树](http://codeup.cn/problem.php?cid=100000611&pid=1)

根据完全二叉树的性质：左孩子的编号一定是2*m， 右孩子一定是2*m+1。可以写出暴力递归的写法， 但是会超时。

```c++

#include <bits/stdc++.h>
using namespace std;
const int MAX = 1e3 + 5;
int m, n;   // 求解， 总共节点数
int ans;
void cnt(int v){
    if(v > n) return;
    cnt(2*v);
    cnt(2*v+1);
    ans ++ ;
}

int main(int argc, char const *argv[]){
    while(cin >> m >> n){
        if ( m == 0 && n == 0) break;
        ans = 0;
        cnt(m);
        cout << ans << endl;
    }
    return 0;
}
```

改用递推写法：

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAX = 1e3 + 5;

int main(int argc, char const *argv[]){
    int m, n;   // 求解， 总共节点数
    while( cin >> m >> n && n && m){
        int ans = 1;
        int l= 2*m, r = 2*m+1; 
        while(r <= n){          	// 如果当前行的节点数是全的（不是最后一行残缺行）
            ans += r - l + 1;		// 加上该行所有节点
            l = l*2, r = r*2 + 1;	// 继续向下
        }// 当找到最后一行时跳出
        if ( l <= n ){				// 判断最后一行是否有节点
            ans += n - l + 1;		// 有的话加上
        }
        cout << ans << endl;
    }
    return 0;
}
```



#### [Codeup 问题 D: 二叉树遍历](http://codeup.cn/problem.php?cid=100000611&pid=3)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1e3 + 5;
int n;  // 总共的节点数
string str;
int depth=-1;

struct Node{
    char v;
    Node *lc, *rc;
};

Node *createTree(){
    Node *root = new Node;
    depth ++;        
    if(str[depth] == '#') {
        return NULL; 
    }
    root->v = str[depth];
    root->lc = createTree();
    root->rc = createTree();
    return root;
}

void inOrder(Node *root){
    if(root == NULL) return;
    inOrder(root->lc);
    cout << root->v <<" " ;
    inOrder(root->rc);
}

int main(int argc, char const *argv[]){
    while(cin >> str){
        depth = -1;
        Node *root= createTree();
        inOrder(root);
        cout << endl;
    }
    return 0;
}
```

更多Codeup的专题训练放在《算法笔记Codeup题解》中了。

### [ 1021  Deepest Root](https://pintia.cn/problem-sets/994805342720868352/problems/994805482919673856)

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-06-03 15:28:11
 * @LastEditTime: 2020-06-03 15:57:35
 * @Description: 
 */ 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int MAXN = 1e4 + 5;
int arr[MAXN];          // 连通块数量
vector<int> G[MAXN];    // 领接表
int n;                  // 节点数


void init(){
    for (int i = 1; i <= n ; i++) {
       arr[i] = i;
    }
}

// 递归写法
int find(int u){
    if ( find(u) == u) return u;
    else{
        arr[u] = find(arr[u]);
        return arr[u];
    }
}

// 迭代写法
// node为某次具体操作节点, tmp_node为其能找到group头
// 关系类似链表中的p、q, 一个当前, 一个记录下一个或上一个节点
int findFather(int v){
    int node  = v;
    while ( arr[v] != v ) {
        v = arr[v];
    }
    // v此时指向的是该group的头
    // 路径压缩, 把该group中所有成员都指向一个头v
    while ( node != arr[node] ){
        int tmp_node = node;
        node = arr[node];
        arr[tmp_node] = v;
    }
    return v;
}

int countBlock(){
    int ans = 0;
    for (int i = 1; i <= n; i++) {
       if ( arr[i] == i) ans ++;
    }
    return ans;
}


void merge(int u, int v){
    // int fa = find(u);
    // int fb = find(v);
    int fa = findFather(u);
    int fb = findFather(v);
    if ( fa != fb ) arr[fa] = fb; 
}

vector<int> temp, ans;
int maxHeight;
void DFS(int u, int height, int pre){

    if (height > maxHeight){
        temp.clear();
        temp.push_back(u);
        maxHeight = height;
    }else if (height == maxHeight){
        temp.push_back(u);
    }

    for (int v = 0; v < G[u].size(); v++) {
       if (G[u][v] == pre) continue;
       DFS(G[u][v], height+1, u);
    }
}


int main(){
    cin >> n;
    int bian = n-1;
    init();
    while(bian--){
        int u, v;
        cin >> u >>v;
        G[u].push_back(v);
        G[v].push_back(u);
        merge(u, v);
    }

    int block_num = countBlock();
    // 只有一个连通块的N个节点, n-1条边一定是树
    if (block_num == 1){
        DFS(1, 1, -1);
        ans = temp;
        // 随意找ans中的节点
        DFS(ans[0], 1, -1);
        // 取并集
        for (int i = 0; i < temp.size(); i++) {
            ans.push_back(temp[i]);
        }
        sort(ans.begin(), ans.end());
        cout << ans[0] <<endl;
        for (int i = 1; i < ans.size(); i++) {
           if (ans[i] != ans[i-1]) cout << ans[i] << endl;
        }
    }else{
        cout << "Error: " << block_num << " components" <<endl;
    }
    return 0;
}
```

### [1023 Have Fun with Numbers](https://pintia.cn/problem-sets/994805342720868352/problems/994805478658260992)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
unordered_map<char, int> mp;

// 高精度*低精度
string multiply(const string s, int n){
    string ans;
    int t = 0;
    int len = s.length();
    string rev = s;
    reverse(rev.begin(), rev.end());
    
    for (int i = 0; i <  len|| t; i++) {
        if ( i < len ){
            t += (rev[i] - '0') * n;
            mp[rev[i]] += 1;
        }
        ans += char(t%10+'0');
        t /= 10;
    }   
    while( ans.length() > 1 && ans.back() == '0') ans.pop_back();
    reverse(ans.begin(), ans.end());
    return ans;
}


string doubleIt(const string s){
    return multiply(s, 2);
}


bool YesorNo(){
    for(unordered_map<char, int>::iterator it = mp.begin(); it != mp.end() ; it++){
        if (it->second != 0) return false;
    }
    return true;
}

int main(){
    string n;
    cin >> n;
    string doublen =  doubleIt(n);
    for (int i = 0; i < doublen.size(); i++) {
    //    cout << doublen[i] << " " << mp[doublen[i]] << endl;
       mp[ doublen[i] ]--;
    }
    string ans = YesorNo() ? "Yes" : "No";
    cout << ans << endl;
    cout << doublen <<endl;
    return 0;
}
```

unordered_map使用: https://blog.csdn.net/fengbingchun/article/details/52235026

### [Palindromic Number](https://pintia.cn/problem-sets/994805342720868352/problems/994805476473028608)

> 大数相加
>
> 闫总模板: https://www.acwing.com/blog/content/277/

```cpp

#include <bits/stdc++.h>
using namespace std;

string add(string a, string b){
    string ans;
    // 第四个点， 去前置0
    a = a.substr(a.find_first_not_of("0"));
    b = b.substr(b.find_first_not_of("0"));

    reverse(a.begin(), a.end());
    reverse(b.begin(), b.end());
    int t = 0;
    for (int i = 0; i < a.size() || i < a.size() || t; i++) {
        if ( i < a.size() ) t += a[i] - '0';
        if ( i < b.size() ) t += b[i] - '0';
        ans += char(t % 10 + '0');
        t /= 10;
    }
    reverse(ans.begin(), ans.end());
    return ans.substr(ans.find_first_not_of("0"));
}

string getReverseString(string a){
    string s(a.rbegin(), a.rend());
    return s;
}


bool isPalindromic(string n){
    int len = n.length();
    // 第三个测试点： 1也算是回文数==> 如果是个数的话， 那么step为0
    // if (len == 1 || len == 0) return false; 
    for (int i = 0, j = len - 1; i < len; i++, j--) {
       if ( n[i] != n[j]) return false;
    }
    return true;
}

int main(){
    
    string n;
    int T;
    cin >> n >> T;
    string tmp = n;
    int i;
    for (i = 0; i < T; i++) {
       if (isPalindromic(tmp)) {
           break;
       }
       tmp = add(tmp, getReverseString(tmp));
    }
    cout << tmp << endl << i << endl;
    return 0;
}
```



[1025 PAT Ranking](https://pintia.cn/problem-sets/994805342720868352/problems/994805474338127872)

> 通过率为0.27, 题意较简单, 但是处理起来会有坑，也算学习到了吧。
>
> 1. vector的拼接： `v.insert(v.begin(), va.begin(), va.end())`
> 2. sort自定义函数：在嵌套比较时注意if的条件，`return true`的将会被放在前面
> 3. 排名的处理: 排名相同的一致，不同的在其排序索引上+1

```c++
#include <bits/stdc++.h>
using namespace std;
struct rc{
    string ID;      //记录ID
    int grade;      //成绩
    int local_num;  //场次
    int local_rank; //该场次的排名
}typedef rc;


bool cmp(rc &a, rc &b){
    if(a.grade != b.grade)
        return a.grade>b.grade;
    else
        return (a.ID<b.ID);
}

/**或者写成
bool cmp(const node &a, const node &b)
{
    return a.score==b.score ? a.id<b.id : a.score>b.score;
    // 如果得分相等,ID小的在前,如果得分不等,那么得分大的在前
}
*/


int main(int argc, char const *argv[]){
    int N;
    cin >> N;
    std::vector<rc> vall;
    for (int j = 1; j <= N; ++j){
        int K;
        cin >> K;
        std::vector<rc> v;
        for (int i = 0; i < K; ++i){
            rc r;
            cin >> r.ID >> r.grade;
            r.local_num = j;            
            v.push_back(r);
        }
        sort(v.begin(), v.end(), cmp);  // 先将当局排名算出
        int rank = 1;
        int rankv = v[0].grade;

        v[0].local_rank = rank;
        for (int i = 1; i < v.size(); ++i){
            // 思路
            // if(rankv == v[i].grade){
            //     v[i].local_rank = rank;
            // }else{
            //     rank = i+1;
            //     v[i].local_rank = rank;
            // }
            /* 简洁写法 */
            if(rankv != v[i].grade){
                rank = i+1;
            }
            v[i].local_rank = rank;
            rankv = v[i].grade;         

        }
        vall.insert(vall.end(), v.begin(), v.end());    // 将每个场次的结果拼起来
    }

    sort(vall.begin(), vall.end(), cmp);                // 对总结果进行排序算出finalRank
    int Ssize =  vall.size();
    cout  << Ssize << endl;
    int finalRank = 1;          // 用来处理排名相同的情况
    int rankv = vall[0].grade;  // 用来确定何时排名相同

    cout << vall.at(0).ID << " "<<finalRank << " "<<vall.at(0).local_num << " "<<vall.at(0).local_rank << endl;
    for (int i = 1; i < Ssize; ++i){
        // 这样看思路更清晰一点
        // if(rankv == vall[i].grade){
        //     cout << vall.at(i).ID <<" "<< finalRank <<" "<< vall.at(i).local_num <<" "<< vall.at(i).local_rank << endl;
        // }else{
        //     finalRank = i+1;
        //     cout << vall.at(i).ID <<" "<< finalRank <<" "<< vall.at(i).local_num <<" "<< vall.at(i).local_rank << endl;
        // }
        /* 简洁写法 */
        if(rankv != vall[i].grade){
            finalRank = i+1;
        }
        cout << vall.at(i).ID <<" "<< finalRank <<" "<< vall.at(i).local_num <<" "<< vall.at(i).local_rank << endl;
        rankv = vall[i].grade;
    }
    return 0;
}
```

▲我这边判断两个rc是否相同，是用rankv和finalrank来记录上一次的结果的，其实还能在for里面判断v.[i].grade == v[i-1].grade

### [ 1027 Colors in Mars](https://pintia.cn/problem-sets/994805342720868352/problems/994805470349344768)

> 考察点： 进制的转换

```c++
#include <bits/stdc++.h>
using namespace std;

string int2string(int ans){
    switch(ans){
        case 10:
            return "A";
        case 11:
            return "B";
        case 12:
            return "C";
        default:
            return to_string(ans);
    }
}

string decimal2radix(int n){
    if(n<13) return "0"+int2string(n);

    int tmp = n;
    string res;
    while(tmp>0){
        res.insert(0, int2string(tmp%13));
        tmp /= 13;
    }
    return res;
}


int main(int argc, char const *argv[]){
    int r, g, b;
    cin >> r >> g >> b ;
    cout << "#" << decimal2radix(r) << decimal2radix(g) << decimal2radix(b) << endl;
    return 0;
}
```

[1034 Head of a Gang](https://pintia.cn/problem-sets/994805342720868352/problems/994805456881434624)

> 题目很难理解。首先要明确的是本题为无向图。
>
> 涉及一个点权的维护， 在增加一条边的时候，把这个边的两端点都加上相应的权值。
> 需要做的是： DFS遍历每个连通块， 获得点权最大的点、 成员个数（>2)、 总边权（>K)
>
> 本题也可以使用并查集解决。在使用并查集时，只要注意合并函数中需要总是保持点权更大的结点为集合的根结点（原先的合并函数是随意指定其中一个根结点为合并后集合的根结点），就能符合题目的要求。而为了达到题目对总边权与成员人数的要求，需要定义两个数组：一个数组用来存放以当前结点为根结点的集合的总边权；另一个数组用来存放以当前结点为根结点的集合中的成员人数。这样当所有通话记录合并处理完毕后，这两个数组就自动存放了每个集合的总边权和成员人数，再根据题意进行筛选即可

```c++

#include <bits/stdc++.h>
using namespace std;
const int MAXN = 2e3 + 5;
const int INF = 0x3f3f3f3f;

// map<stirng, int>可以直接按字典序排序； 如果要自己定义结构体， 得实现排序功能
std::map<string, int> gang;
std::map<int, string> int2string;	// 通过遍历后找到的节点编号ID来反着找到字符串ID
std::map<string, int> string2int;	// 用来检索当前字符串ID是否存在
int G[MAXN][MAXN];                  // 邻接矩阵
int ts[MAXN] = {0};                 // 权值数组, 记录每个点的点权
bool visited[MAXN] = {false};

int numPerson = 0;                  // 结点编号, 最终为一共有的节点数
int N, K;


void dfs(int u, int &head, int &numMember, int &totalValue){
    visited[u] = true;
    numMember ++;
    if ( ts[u] > ts[head] ){    //  如果当前访问节点的点权大于head的点权，则更换头目
        head = u;
    }

    for (int v = 0; v < numPerson; ++v){
        if(G[u][v] > 0){        // 存在边可达
            totalValue += G[u][v]; // 连通块的总边权增加该边权
            G[u][v] = G[v][u] = 0; // 删除此边防止回头
            if(visited[v] == false){
                dfs(v, head, numMember, totalValue);
            }
        }
    }
}


void dfsTravel(){
    for (int i = 0; i < numPerson; ++i){
        if (visited[i] == false){
            int head = i, numMember = 0, totalValue = 0;

            dfs(i, head, numMember, totalValue);
            if(numMember > 2 && totalValue > K){
                gang[int2string[head]] = numMember;
            }
        }
    }


}

// 将输入的字符编号转换为数字编号
int change(string tmp){
    if ( string2int.find(tmp) != string2int.end() ){        // 如果存在
        return string2int[tmp];
    }else{													// 如果不存在
        string2int[tmp] = numPerson;
        int2string[numPerson] = tmp;
        return numPerson++;									// 放回编号
    }
}


int main(int argc, char const *argv[]){
    cin >> N >>K;
    int t;
    for (int i = 0; i < N; ++i){
        string a, b;
        cin >> a >> b >> t;
        int id1 = change(a);             // 将结点字符转换为数字编号
        int id2 = change(b);
        ts[id1] += t;
        ts[id2] += t;
        G[id1][id2] += t;
        G[id2][id1] += t;				// 用vector(int) G[MAXN]不行->vector没初始化不允许相加
    }

    dfsTravel();
    cout << gang.size() <<endl;
    for (std::map<string, int>::iterator i = gang.begin(); i != gang.end(); ++i){
        cout << i->first <<" " << i->second <<endl;
    }
    return 0;
}
```

▲. map<type1，type2>是自动按照 type1从小到大进行排序的，因此使用map<string，int>建立头目姓名与成员人数的关系便于输出结果。当然，也可以使用结构体来存放头目姓名与成员人数，如下所示

```c++
struct Gang{
    string head;
    int numMember;
}Gangarr[MAXN];

// 按字典序排序， map有这个功能
bool cmp(Gang &a, Gang &b){
    return a.head < b.head;
}
```

补充： vector初始化

```c++
// 一位初始化
	// 指定初始化
std::vector<int> v = {2, 3, 4, 5};
	// 5个0
std::vector<int> v(5);
	// 5个2
std::vector<int> v(5, 2);
...

// 二维初始化
std::vector<int> v[MAXN]= {{2, 4, 1}, {3, 5, 7}};

```





```cpp

#include <bits/stdc++.h>
const int N = 1e5 + 5;

struct staticList{
    int nxt;
    char val;
    bool visited;
    // 结构体数组每个元素都会经过这个初始化过程, 因此这步就可以直接初始化
    staticList(): visited(false){};
}arr[N];


using namespace std;
int main(){
    int start1, start2, n;
    scanf("%d %d %d", &start1, &start2, &n);
    int t = n;
    int addr, nexta;
    char c;
    while(t--){
        scanf("%d %c %d", &addr, &c, &nexta);
        arr[addr].val = c;
        arr[addr].nxt = nexta;
    }

    int ans;
    // 注意这边结束结束条件是ans!=-1
    // 如果写成arr[addr].nxt != -1, 第4个测试点会超时
    // 原因是如果start就为-1, 那么会调到0, 0->0, 
    // 实际上道理就跟写链表一样: while(head != NULL)
    for (ans = start1; ans != -1; ans = arr[ans].nxt) {
        arr[ans].visited = true;
    }


    for (ans = start2; ans != -1; ans = arr[ans].nxt) {
        if (arr[ans].visited) break;
    }



    // if (arr[ans].nxt != -1)
    if (ans != -1)
        printf("%05d\n", ans);
    else
        printf("-1\n");
    return 0;
}
```



### [**1028** **List Sorting** (25分)](https://pintia.cn/problem-sets/994805342720868352/problems/994805468327690240)

> 简单排序, 但是不能用cin, 不然会超时。用`ios::sync_with_stdio`可以过

```cpp
#include <bits/stdc++.h>
using namespace std;

struct record{
    string id;
    string name;
    int grade;
    record(){}
    record(string idx, string namex, int gradex): id(idx), name(namex), grade(gradex){}
}typedef  record;

bool cmp1(const record &r1, const record &r2){
    return r1.id < r2.id;
}


bool cmp2(const record &r1, const record &r2){
    if (r1.name == r2.name){
        return r1.id < r2.id;
    }
    return r1.name < r2.name;
}


bool cmp3(const record &r1, const record &r2){
    if (r1.grade == r2.grade)
        return r1.id < r2.id;
    return r1.grade < r2.grade;
}


int main(){
    int n, c;
    ios::sync_with_stdio(false);
    cin >> n >>c;
    vector<record> v;

    for (int i = 0; i < n; i++) {
        string idx, name;
        int grade;
        cin >> idx >> name >> grade;
        record r = record(idx, name, grade);
        v.push_back(r);
    }
    if (c == 1){
        sort(v.begin(), v.end(), cmp1);
    }else if (c == 2){
        sort(v.begin(), v.end(), cmp2);
    }else{
        sort(v.begin(), v.end(), cmp3);
    }

    for (int i = 0; i < v.size(); i++) {
       record r = v.at(i);
       cout << r.id <<" " << r.name << " " <<r.grade <<endl;
    }
    
    return 0;
}
```

### [1029 Median](https://pintia.cn/problem-sets/994805342720868352/problems/994805466364755968)

```c++

 #include <bits/stdc++.h>
 using namespace std;
typedef long long ll;

 int main(){
     ios::sync_with_stdio(false);
     int n, m;
     vector<ll> v1;
     vector<ll> v2;
    ll tmp;

     cin >> n;
     for (int i = 0; i < n; i++) {
         cin >> tmp;
         v1.push_back(tmp);
     }

    cin >> m;
     for (int j = 0; j < m; j++) {
        cin >> tmp;
        v2.push_back(tmp);
     }

    
    int a = 0, b = 0;
    int x = 0;
    int len1 = v1.size(), len2 = v2.size();
    int sumlen = len1 + len2 + 1 >> 1;
    ll ans = -1;
    for (int i = 0, j = 0, k = 0; k < sumlen ; k++) {
        if ( ( i < len1 && j >= len2 ) || ( i < len1 && j < len2 && v1[i] <= v2[j]) ){
            ans = v1[i];
            i++;
        }else{
            ans = v2[j];
            j++;
        }
    }
    cout << ans << endl;

    return 0;
 }
```

> 下面做法是v1, v2在末尾都加了个无穷大的数, 保证不会越界, 在自己遍历完后直接遍历另一个vec。 思路跟[PAT甲级辅导课（试听课）（二）](https://www.bilibili.com/video/BV1w54y1Q7Uc)讲坏掉的键盘[#1084](#1084 Broken Keyboard)一样，防止j++无限的加， 最后b[j]越出边界，添加了一个不会出现的字符#	

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int INF = 0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);
    int n1, n2;
    cin >> n1;
    vector<int> v1, v2;
    int tmp;
    for (int i = 0; i < n1; i++) {
       cin >> tmp;
       v1.push_back(tmp);
    }
    v1.push_back(INF);

    cin >> n2;
    for (int i = 0; i < n2; i++) {
       cin >> tmp;
       v2.push_back(tmp);
    }
    v2.push_back(INF);

    int cnt  = 0;
    int mid = (n1+n2-1)/2;
    int i= 0, j=0;
    while(cnt < mid){
        if ( v1[i] < v2[j]){
            i ++;
        }else{
            j ++;
        }
        cnt ++;
    }

    if (v1[i] < v2[j]) cout << v1[i] << endl;
    else cout << v2[j] << endl;
    return 0;
}

```

### [▲1030 Travel Plan](https://pintia.cn/problem-sets/994805342720868352/problems/994805464397627392)

> 练习dijkstra和 dijkstra+dfs 最经典、合适的一题





```cpp
/*
 * @Author: Mrli
 * @Date: 2020-07-19 15:38:12
 * @LastEditTime: 2020-07-19 16:18:08
 * @Description: 
 */ 

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e3 + 5;
int n, m;

int arr[maxn][maxn];

vector<int> chess;

bool judge(int i){
    // i为行数, 即放的第几个
    // chess[x] 记录的列数
    for (int j = 0; j < i; j++) {
        // 同一列和对角线判断
        if ( ( chess[i]  == chess[j] ) || ( abs(chess[i] - chess[j]) == abs(i - j))) return false;
    }  
    return true;
}

int main(){

    int T;
    cin >> T;
    while(T--){
        cin >>n;
        chess.resize(n);
        bool yes = true;
        for (int i = 0; i < n; i++) {
            cin >> chess[i];
            if(judge(i) == false){
                yes = false;
                // break;
            }
        }   
        if (yes) cout << "YES" <<endl;
        else cout << "NO" << endl;
    }
    return 0;
}

/*
 * @Author: Mrli
 * @Date: 2020-07-19 23:55:28
 * @LastEditTime: 2020-07-20 00:11:19
 * @Description: Dijkstra + DFS
 */ 

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 5e2 + 5;
const int INF = 0x3f3f3f3f;
int n, m, s, d;

int G[maxn][maxn];
int C[maxn][maxn];
int dd[maxn];			// 区别少了个cc[maxn]
bool vis[maxn];

vector<int> pre[maxn];		// pre[maxn]变成了vector数组
vector<int> tempPath, path;	// 多了两个记录路径的vec, 用来dfs比较, path为最终的答案

/**
    只涉及G[][], 不涉及C[][]
*/
void dijkstra(int s){
    fill(dd, dd+maxn, INF);
    dd[s] = 0;

    for (int i = 0; i < n; i++) {           // 循环n个节点
       int u = -1, MIN = INF;
       for (int j = 0; j < n; j++) {
          if (vis[j] == false && dd[j] < MIN){
              MIN = dd[j];
              u = j;
          }
       }
       
       if ( u == -1) return ;
        vis[u] = true;
        for (int v = 0; v < n; v++) {
            if ( vis[v] == false && G[u][v] != INF){
                if ( dd[u] + G[u][v] < dd[v]){
                    dd[v] = dd[u] + G[u][v];
                    pre[v].clear();
                    pre[v].push_back(u);    // 记录v的前驱
                }else if ( dd[u] + G[u][v] == dd[v] ){
                        pre[v].push_back(u);
                }
            }
        }
        
    }
}


int Mincost = INF;

/**
    只涉及C[][], 不涉及G[][]
*/
void dfs(int v){
    if ( v == s){
        tempPath.push_back(v);
        int tmpCost = 0;
        for (int i = tempPath.size() - 1; i > 0 ; i--) {
           int idx = tempPath[i], idnxt = tempPath[i-1];
           tmpCost += C[idx][idnxt];                    
           // 之前的digistra不记录Cost, 只记录路径, DFS时找出最低cost
        }
        if (tmpCost < Mincost){
            Mincost = tmpCost;
            path = tempPath;
        }

        tempPath.pop_back();
        return;
    }

    tempPath.push_back(v);
    for (int i = 0; i < pre[v].size(); i++) {
        dfs(pre[v][i]);
    }
        tempPath.pop_back();

}

int main(){

    cin >> n >> m >> s >> d;
    int u, v, dist, cost;

    fill(G[0], G[0] + maxn* maxn , INF);
    for (int i = 0; i < m; i++) {
       cin >> u >> v >> dist >> cost;
       G[u][v] = G[v][u] = dist;
       C[u][v] = C[v][u] = cost;   
    }

    dijkstra(s);
    dfs(d);
    for (int i = path.size() - 1; i >= 0; i--) {
        printf("%d ", path[i]);
    }
    printf("%d %d\n", dd[d], Mincost);

    return 0;
}
```





### [1031 Hello World for U](https://pintia.cn/problem-sets/994805342720868352/problems/994805462535356416)

> 模拟题

```cpp
/*
* @Author: Mrli
* @Date: 2020-06-26 18:12:39
 * @LastEditTime: 2020-06-26 19:38:37
* @Description: 
*/ 

#include <bits/stdc++.h>
using namespace std;


int main(){
    ios::sync_with_stdio(false);
    string s;
    cin >> s;
    int N = s.size();
    int n1 = ( N + 2 ) / 3, n3 = n1, n2 = ( N + 2 - n1 - n3);
    // cout << n1 << n2 << n3 << endl;

    for (int i = 0; i < n1 -1; i++) {
        cout << s[i];                           // 输出左侧字符
        for (int j = 1; j < n2 - 1; j++) {      
           cout << " ";                         // 输出中间的空格
        }
        cout << s[N - i - 1] << endl;                   // 输出右侧的字符
    }


    for (int i = 0; i < n2; i++) {
       cout << s[n1 - 1 + i ];                              // 输出最底层的字符
    }


    return 0;
}

```



### [1036 Boys vs Girls](https://pintia.cn/problem-sets/994805342720868352/problems/994805453203030016)

> 考点: 结构体； sort比较函数

```c++
#include <bits/stdc++.h>
using namespace std;

struct Record
{
    string name;
    string sex;
    string subject;
    int grade;
}typedef rc;

bool cmp(rc &r1, rc &r2){
    if(r1.sex == "M" && r2.sex == "F") return true;
    else if(r1.sex == "F" && r2.sex == "M") return false;
    else
        return r1.grade < r2.grade;
}

int main(int argc, char const *argv[]){
    int n;
    cin >> n;
    std::vector<rc> v(n);
    for (int i = 0; i < n; ++i){
        rc r;
        cin >> r.name >> r.sex >> r.subject >> r.grade;
        v[i] = r;
    }

    sort(v.begin(), v.end(), cmp);
    // for (std::vector<rc>::iterator i = v.begin(); i != v.end(); ++i)
    //     cout << i->name << " " << i->sex << " " << i->subject << " " << i->grade <<endl;


    std::vector<rc>::iterator boy = v.begin();
    std::vector<rc>::iterator girl = v.end()-1;
    bool absent = false;

    if(girl->sex == "F"){
        cout << girl->name << " " << girl->subject <<endl;
    }
    else{
        cout << "Absent" << endl;
        absent = true;
    }
    if(boy->sex == "M"){
        cout << boy->name << " " << boy->subject <<endl;
    }
    else{
        cout << "Absent" << endl;
        absent = true;
    }

    if(!absent){
        cout << girl->grade - boy->grade << endl;
    }else{
        cout << "NA" <<endl;
    }
    return 0;
}
```



### [1037 Magic Coupon](https://pintia.cn/problem-sets/994805342720868352/problems/994805451374313472)

> 贪心， 证明a> b, c > b存在ad > bc

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-06-26 19:38:35
 * @LastEditTime: 2020-06-26 19:45:59
 * @Description: 
 */ 

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int main(){
    ios::sync_with_stdio(false);
    int nc, np;
    cin >> nc;
    vector<int> v1, v2;
    int tmp;
    for (int i = 0; i < nc; i++) {
        cin >> tmp;
        v1.push_back(tmp);
    }


    cin >> np;
    for (int i = 0; i < np; i++) {
       cin >> tmp;
       v2.push_back(tmp);
    }

    int len1 = v1.size(), len2 = v2.size();

    sort(v1.begin(), v1.end());
    sort(v2.begin(), v2.end());

    int x, y;
    x = 0, y = 0;
    ll ans = 0;

    // while( v1[x] < 0 && v2[y] < 0){
    while( v1[x] < 0 && v2[y] < 0 && x < len1 && y < len2){
        ans += v1[x++] * v2[y++];
    }


    x = len1 - 1, y = len2 - 1;
    // while( v1[x] > 0 && v2[y] > 0){
    while( v1[x] > 0 && v2[y] > 0 && x >= 0 && y >= 0){
        ans += v1[x--] * v2[y--];
    }
    cout << ans << endl;

    return 0;
}

```



### 1038[ Recover the Smallest Number](https://pintia.cn/problem-sets/994805342720868352/problems/994805449625288704)

> 新cmp中通过 string + string 来比, 

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-09-17 13:46:24
 * @LastEditTime: 2020-09-17 14:09:58
 * @Description: 
 */
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
const int maxn = 1e4 + 5;

string arr[maxn];


bool cmp(string &a, string &b){
    return a + b < b + a;
}


int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >>n;
    for (int i = 0; i < n; i++) {
       cin >> arr[i];
    }    

    sort(arr, arr+n, cmp);

    
    string ans;

    for (int i = 0; i < n; i++) {
       ans += arr[i];
    }
    while(ans.length() != 0 && ans[0] == '0')
        ans.erase(ans.begin());
    if(ans.length() == 0) cout << 0;
    cout << ans;
    
    return 0;
}
```

B1023 最小的数

> 贪心

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
int arr[10];

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    for (int i = 0; i < 10; i++) {
       cin >> arr[i];
    }

    for (int i = 1; i < 10; i++) {
        if ( arr[i]  > 0){
            cout << i;
            arr[i] --;
            break;
        }
    }

    for (int i = 0; i < 10; i++) {
        int len = arr[i];
        for (int j = 0; j < len; j++) {
           cout << i;
           arr[i]--;
        }
    }
    cout << endl;
    return 0;
}
```

总结: 上面两题都是根据输入排出一个最小的数。 第一题是不能改变原有的数字顺序， 所以直接用string拼接比较。 第二个是根据给出0-9数字的个数自己去排； 还有第三种， 类似第一题的输入， 需要自己去计算0-9的个数， 然后再像第二题一样去贪心排

### 1040 [ Longest Symmetric String](https://pintia.cn/problem-sets/994805342720868352/problems/994805446102073344)

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-09-17 13:07:21
 * @LastEditTime: 2020-09-17 13:39:11
 * @Description: 
 */
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;


int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);

    string s;
    getline(cin, s);
    int len = s.size();
    int max_ans = 0;
    for (int i = 0; i < len; i++) {
            //    for (int j = len - 1; j >= 1; j--) {			//245ms
       for (int j = len - i; j >= 1 && j - i >= max_ans; j--) {	// 138ms
           string ss = s.substr(i, j);
           string rev(ss.rbegin(), ss.rend());
           if ( ss == rev && max_ans < ss.size() ){
            max_ans = ss.size();
           }
       }
    }
    cout <<max_ans <<endl;
    
    return 0;
}
```



### [ 1041 Be Unique](https://pintia.cn/problem-sets/994805342720868352/problems/994805444361437184)

> 模拟题，难点在找到第一个。此处的`table`为int型数组的哈希表-->可以使用STL中的map代替

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1e5 + 5;
int table[MAXN];
int a[MAXN];

int main(int argc, char const *argv[]){
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i){
        // a[i]来记录不同数字出现的顺序
        cin >> a[i];
        // table记录出现的次数
        table[a[i]] ++ ;
    }

    int ans=-1;
    for (int i = 0; i < n; ++i){
        // 按照出现的顺序遍历, 查看是否有数出现的次数为1
        if ( table[a[i]] == 1){
            // 如果找到出现次数为1的就输出
            ans = a[i];
            break;
        }
    }

    if (ans == -1) cout <<"None" << endl;
    else cout <<ans <<endl;
    return 0;
}
```

### [1042 Shuffling Machine](https://pintia.cn/problem-sets/994805342720868352/problems/994805442671132672)

> 模拟题

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-06-26 22:57:16
 * @LastEditTime: 2020-06-26 23:51:57
 * @Description: 
 */ 


#include <bits/stdc++.h>
#include <iomanip>
using namespace std;

const int n = 54;
string color[] = {"S", "H", "C", "D", "J"};
vector<int> order(n+1, 0);
vector<int> startArr(n+1, 0);
vector<int> endArr(n+1, 0);



void init(){
    for (int i = 1; i <= n; i++) {
       startArr[i] = i;
    }
}    



void mock(){
    for (int i = 0; i < n; i++) {
       endArr[i] = i;
    }
}    

int main(){
    int k;
    cin >> k;

    init();
    for (int i = 1; i <= n; i++) {
        cin >> order[i];
    }


    for (int i = 1; i <= k; i++) {
       for (int j = 1; j <= n; j++) {
           endArr[order[j]]= startArr[j];
       }
       for (int j = 1; j <= n; j++) {
          startArr[j] = endArr[j];
       }
    }

    for (int i = 1; i <= n; i++) {
        
        int endNum = endArr[i] - 1;
       if (i == n ){
           cout << color[ endNum / 13 ] << endNum% 13 + 1;
       }else{
           cout << color[ endNum / 13 ] << endNum % 13 + 1<< " ";
       }
    }    

    return 0;
}



/**
自己写的版本， 当时没注意下标
*/

#include <bits/stdc++.h>
#include <iomanip>
using namespace std;
const int n = 54;
string color[] = {"S", "H", "C", "D", "J"};
vector<int> order(n, 0);
vector<int> startArr(n, 0);
vector<int> endArr(n, 0);

void init(){
    for (int i = 0; i < n; i++) {
       startArr[i] = i;
    }
}    


int main(){
    int k;
    cin >> k;

    init();
    for (int i = 0; i < n; i++) {
        int tmp;
        cin >> tmp;
        order[i] = tmp - 1;
    }


    for (int i = 0; i < k; i++) {
       for (int j = 0; j < n; j++) {
           endArr[order[j]]= startArr[j];
       }
       for (int j = 0; j < n; j++) {
          startArr[j] = endArr[j];
        //   cout << startArr[j] << " ";
       }
    //    cout << endl;
    }

    for (int i = 0; i < n; i++) {
        
        int endNum = endArr[i];
       if (i == n - 1){
           cout << color[ endNum / 13 ] << endNum% 13 + 1;
       }else{
           cout << color[ endNum / 13 ] << endNum % 13 + 1<< " ";
       }
    }    

    return 0;
}


```



### [ 1043 Is It a Binary Search Tree](https://pintia.cn/problem-sets/994805342720868352/problems/994805440976633856)

> 二叉搜索树: 
>
> - 要点: 建树的时候insert函数中如果val==结点val, 那么当前节点该插到右边中
>
> 

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-06-27 10:52:28
 * @LastEditTime: 2020-06-27 16:17:04
 * @Description: 
 */ 



#include <bits/stdc++.h>
using namespace std;
vector<int> origin, pre, preM, post, postM;

struct Node{
    int v;
    Node* lc, *rc;
    Node(){}
    Node(int val){
        v = val;
        lc = rc = NULL;
    }
}typedef Node;


/**
根据BST规则进行插入
*/
void insert(Node * &root, int val){
    if (root == NULL) {
        root = new Node(val);		// 创建树根节点
        return ;					// 这边记得return 结束
    }
    if ( val < root->v ) insert(root->lc, val);
     else insert(root->rc, val);		// ▲key: BST结点相同时放右边
}


void preOrder(Node* root, vector<int> &v){
    if (root == NULL ) return ;

    v.push_back(root->v);
    preOrder(root->lc, v);
    preOrder(root->rc, v);
}


void preMOrder(Node* root, vector<int> &v){
    if (root == NULL ) return ;

    v.push_back(root->v);
    preMOrder(root->rc, v);
    preMOrder(root->lc, v);

}

void postOrder(Node* root, vector<int> &v){
    if (root == NULL ) return ;

    postOrder(root->lc, v);
    postOrder(root->rc, v);
    v.push_back(root->v);

}

void postMOrder(Node* root, vector<int> &v){
    if (root == NULL ) return ;

    postMOrder(root->rc, v);
    postMOrder(root->lc, v);
    v.push_back(root->v);

}


int main(){

    int n;
    int tmp;
    cin >> n;
    Node* root = NULL;
    for (int i = 0; i < n; i++) {
       cin >> tmp;
       origin.push_back(tmp);
       insert(root, tmp);
    }

    preOrder(root, pre);
    // for(auto i: pre){
    //     cout << i << " ";
    // }
    // cout <<endl;

    preMOrder(root, preM);
    postOrder(root, post);
    postMOrder(root, postM);

    if ( origin == pre ){
        cout << "YES" << endl;
        for (int i = 0; i < post.size(); i++) {
           if (i != post.size() - 1) cout << post.at(i) << " ";
           else cout << post.at(i) << endl;
        }
    }else if (origin == preM){
        cout << "YES" << endl;
        for (int i = 0; i < postM.size(); i++) {
           if (i != postM.size() - 1) cout << postM.at(i) << " ";
           else cout << postM.at(i) << endl;
        }
    }else{
        cout << "NO" <<endl;
    }

    return 0;
}

```



### 1044 [Shopping in Mars](https://pintia.cn/problem-sets/994805342720868352/problems/994805439202443264)

> 前缀和 + 二分
>
> 本题思路最大的亮点在于， 用前缀和构造了单调递增的数列， 然后使用二分

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
int s;
vector<int>sum;

// [L, r)找到第一个大于x的位置
int upper_bound(int left, int right, int x){
    int mid;
    int l = left, r = right;
    while( l < r){
        mid = (l+r) >> 1;
        if ( sum[mid] > x){
            r = mid;
        }else{
            l = mid + 1;
        }
    }
    return l;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >> n >> s;
    
    sum.resize(n+1, 0);
    for (int i = 1; i <= n; i++) {
       cin >> sum[i];
       sum[i] = sum[i] + sum[i-1];
    }

    int nearS = INF;        // 为最接近S的最小值
    for (int i = 1; i <= n; i++) {
        // 使用STL函数
       int j = upper_bound(sum.begin()+i, sum.end(), sum[i-1] + s, less<int>()) - sum.begin() ;
       // 教材:法二
        // int j = upper_bound(i, n + 1, sum[i-1]+s);       
       // j为大于等于 sum[i-1] + S的值, 如果能和为s, 那么sum[j-1]就是能对应匹配的值
       if ( sum[j-1] - sum[i-1] == s){
           nearS = s;
           break;
        // 否则sum[j] - sum[i-1] 可能为最接近的
       }else if ( j <=n && sum[j] - sum[i-1] < nearS ){
           nearS = sum[j] - sum[i-1];
       }
    }

    for (int i = 1; i <= n ; i++) {
       int j = upper_bound(sum.begin()+i, sum.end(), sum[i-1] + nearS, less<int>())- sum.begin() ;
        // int j = upper_bound(i, n + 1, sum[i-1]+nearS);

       if ( sum[j-1] - sum[i-1] == nearS){
           cout << i << "-" << j-1 <<endl;
       }
    }
    return 0;
}

```





### [1046 Shortest Distance](https://pintia.cn/problem-sets/994805342720868352/problems/994805435700199424)

```cpp
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;
typedef long long ll;



int main(){

    int N;
    ll d;
    vector<ll> distArr;         // 存放的是第i + 1号到起点的顺时针距离和
    ll sum = 0;
    cin >> N;
    // 书上答案
    // distArr.resize(N + 1);
    // distArr[0] =  0;
    // for (int i = 1; i <= N; i++) {
    //     cin >> distArr[i];
    //     sum += distArr[i];
    //     distArr[i] = distArr[i] + distArr[i- 1];
    // }
    
    distArr.resize(N);
    distArr[0] =  0;
    for (int i = 1; i < N; i++) {
        cin >> distArr[i];
        distArr[i] = distArr[i] + distArr[i-1];
    }
    int lastDist;
    cin >> lastDist;
    sum = lastDist + distArr[N-1];

    int k;
    int s, t;
    cin >> k;
    
    for (int i = 0; i < k; i++) {
       cin >> s >> t;
       if (t < s) swap(s, t);
       ll twoDist = distArr[t - 1] - distArr[s - 1];
       cout << min( twoDist, sum - twoDist ) << endl;
    }

    return 0;
}

```





### [1050 String Subtraction](https://pintia.cn/problem-sets/994805342720868352/problems/994805429018673152)

> 题意:将S1中出现的S2字符全部删除。
>
> 解题：删一个改一个，但是考虑到字符串可以理解为数组，所以修改必然要牵扯到移位。此时有个细节，到底是从头往后遍历还是从后往前遍历。从前往后修改存在的问题是，**如果是判断相等后立马修改,则后面的索引值会错位(因为删除后后面的字符串往前调整,而指针却+1,所以会略过一个字符)**
>
> 突然发现这个移位的功能不需要自己写，string有提供erase函数！：[C++ string字符串修改和替换方法详解](http://c.biancheng.net/view/1449.html)
>
> （1）erase(pos,n); 删除从pos开始的n个字符，比如erase(0,1)就是删除第一个字符
> （2）erase(position);删除position处的一个字符(position是个string类型的**迭代器**),如果是数字会截断索引值为position后字符串
> （3）erase(first,last);删除从first到last之间的字符（first和last都是迭代器）
>
> △因此，我使用的是从后往前用erase函数进行删除；读取行内容使用getline

```c++
#include <bits/stdc++.h>
using namespace std;
int main(int argc, char const *argv[]){
    string origin;
    string out;
    getline(cin, origin);
    getline(cin, out);
    int Lorigin = origin.size();
    int Lout = out.size();
    for (int j = Lorigin-1; j >= 0; --j){
        for (int i = 0; i < Lout; ++i){	// 在string2中找 ==> 找某个是否存在--> hashmap
            if(origin[j] == out[i]){
                origin.erase(j,1);
            }
        }
    }
    cout << origin <<endl;
    return 0;
}
```



```java

public class StringSubtraction1035 {
    public static void main(String[] args) {
        String longs, shorts;
        Scanner scanner = new Scanner(System.in);
        longs = scanner.nextLine();		// 遇到\n结束读取, \n不在string中
        shorts = scanner.nextLine();
        HashMap<Character, Integer> hs = new HashMap<>();
        for (int i = 0; i < shorts.length(); i++) {
            hs.put(shorts.charAt(i), 1);
        }


        for (int i = 0; i < longs.length(); i++) {
            if (!hs.containsKey(longs.charAt(i))) {
                System.out.print(longs.charAt(i));
            }
        }
        System.out.println();
    }
}

```



```c++
//基于平衡树实现：0（logn）
//有序
set<int>a;map<int，int>b;
multiset<int>c;multiset<int，int>d;

//基于哈希表实现：0（1）
//无序
unordered_set<int>e;
unordered_map<int，int>f;
unordered_multiset<int>g;
unordered_multimap<int，int>h;
```

### [1052 Linked List Sorting](https://pintia.cn/problem-sets/994805342720868352/problems/994805425780670464)

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-06-27 16:20:42
 * @LastEditTime: 2020-06-27 17:31:58
 * @Description: 
 */ 

#include <bits/stdc++.h>
#include <algorithm>
using namespace std;
typedef long long ll;
int n;
const int MAXN = 1e6;


struct node{
    int val;
    // addr为了在最后遍历输出结果时输出, nextaddr为了能够按照一开始给定的链表顺序遍历整条链表 
    int addr, nextaddr;
    bool flag;  // 计算哪些节点是在链表上的
    node(){ flag = false; }
};


// node v[MAXN];
vector<node> v(MAXN);

bool cmp(node a, node b){
    if ( a.flag == false || b.flag == false){
        return a.flag > b.flag;
    }else{
        return a.val < b.val;
    }
}


int main(){
    cin >>n;
    int root;
    cin >> root;
    for (int i = 0; i < n; i++) {
        int addr, val, nextaddr;
        cin >> addr >> val >> nextaddr;
        v[addr].addr =addr ;
        v[addr].val = val;
        v[addr].nextaddr = nextaddr;
    }    
    
    
    // 计算有效节点
    int cnt = 0, p = root;
    while( p != -1){
        v[p].flag = true;
        cnt ++ ;
        p = v[p].nextaddr;
    }
    
    if (!cnt){
        printf("0 -1\n");
    }else{
        sort(v.begin(), v.end(), cmp);	// 用vector时
        // sort(v, v+MAXN, cmp);  用数组时
        printf("%d %05d\n", cnt, v[0].addr);
        for (int i = 0; i < cnt; i++) {
            if ( i != cnt - 1)
                printf("%05d %d %05d\n", v[i].addr, v[i].val, v[i+1].addr);
            else
                printf("%05d %d -1\n", v[i].addr, v[i].val);
        }
    }
    return 0;
}
```

### [1053 Path of Equal Weight](https://pintia.cn/problem-sets/994805342720868352/problems/994805424153280512)

```cpp

/*
 * @Author: Mrli
 * @Date: 2020-06-27 16:20:42
 * @LastEditTime: 2020-06-27 18:07:21
 * @Description: 
 */ 

#include <bits/stdc++.h>
#include <algorithm>
using namespace std;
typedef long long ll;
const int MAXN = 100 + 5;

struct Tree{
    int w;
    vector<int> ch;
}v[MAXN];

bool cmp(int a, int b){
    return v[a].w > v[b].w;
}

// 存放路径上节点的id
vector<int> path;

void dfs(int iddex, int nowS, int target){
    if (nowS > target){
        return ;
    }else if( nowS == target){
        if(v[iddex].ch.size() != 0 ) return ;   // 未到叶子节点
        for (int i = 0; i < path.size(); i++) {
            if ( i == path.size() - 1) cout << v[path[i]].w <<endl;
            else cout << v[path[i]].w <<" ";
        }
        return ;
    }
    // nows < target
    for (int i = 0; i < v[iddex].ch.size(); i++) {
       int nxtNode = v[iddex].ch[i];
       path.push_back(nxtNode);
       dfs(nxtNode, nowS + v[nxtNode].w, target);
       path.pop_back();
    }
}

int main(){
    int nn,  notleaf,  s;
    int tmp;
    cin >> nn >> notleaf >> s;
    for (int i = 0; i < nn; i++) {
        cin >> v[i].w;
    }
    int id, k, c;
    for (int j = 0; j < notleaf; j++) {
       cin >> id >> k;
        for (int i = 0; i < k; i++) {
            cin >> c;
            v[id].ch.push_back(c);     
        }
        sort(v[id].ch.begin(), v[id].ch.end(), cmp);
    }

    path.push_back(0);
    dfs(0, v[0].w, s);

    return 0;
}
```

### [1054 The Dominant Color](https://pintia.cn/problem-sets/994805342720868352/problems/994805422639136768)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1000+5;
int main(){
    int M, N;
    map<string, int> mp;
    string tmp;
    cin >> M >> N;
    for(int i=0;i<N;i++){
        for(int j=0;j<M;j++){
            cin >> tmp;
            mp[tmp]++;
        }
    }

    int maxn = -1;
    string ans;
    for(auto m:mp){
        if(m.second > maxn){
            maxn = m.second;
            ans = m.first;
        }
    }
    cout << ans << endl;
    return 0;
}
```



### [1055 The World's Richest](https://pintia.cn/problem-sets/994805342720868352/problems/994805421066272768)





### [1056 Mice and Rice](https://pintia.cn/problem-sets/994805342720868352/problems/994805419468242944)

> 题目有点长，看了题解的解释才懂。在此复述一遍：给你NP个老鼠以及他们的重量W，每NG个老鼠分为一组，不够NG个数的单独算一个组，比较他们每个组的最大值，将**最大值**进入下一轮的比较，同组其余老鼠皆为淘汰，并与其他组同时被淘汰的老鼠排名一致，最后求所有老鼠的**排名**。
>
> 输入解释：第一行分别为NP和NG，第二行是每个老鼠的体重，第三行是每个老鼠的编号。第三行的需要特表说明，如输入样例：6 0 8 7 10 5 9 1 4 2 3， 意思是标号6、0、8为一组，7、10、5为一组，一次往后，即这行说明的是分组顺序
>
> **解题思路：** 这道题的难点在于对数据进行分组，比较并进行排名，其中，**老鼠的排名==该轮比赛分组个数+1**——本轮比较分group个组，意味着有group个优胜者，也就是说，其余所有被淘汰的都在这group之后，即group+1.所以我们直接每轮的分组数即可。

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 10e3 + 5;

struct mice{
    int weight;
    int num;
    int init;
    int rank;
}typedef mice;

int main(int argc, char const *argv[]){
    int NP, NG;
    cin >> NP >> NG;
    
    std::vector<mice> v(NP);    
    queue<int> q;   // 存储下标

    for (int i = 0; i < NP; ++i){
        cin >> v.at(i).weight;
    }

    for (int i = 0; i < NP; ++i){
        int tmp;
        cin >> tmp;
        q.push(tmp);
    }

   int turnP;
   while(q.size() > 1){
       int turnP = q.size();    // 记录每轮的所有人数，q.size()为总人数，+1是结束值
       int turnG = (turnP%NG==0) ? turnP/NG : turnP/NG + 1; // 该轮分组个数
      for (int j = 0; j < turnG; ++j){      // 循环结束为一轮
        std::vector<int> cmp;                // 找出该group中的最大数
        for (int i = 0; i < NG; ++i){       // 将元素加入cmp中
            // j:0-3, NG=3, i=0-2
            if( j * NG + i == turnP) break;   // 当超出含有数量时结束
            // 样例输入NP=11，0-10,到11时无效
            cmp.push_back(q.front());
            q.pop();
        }
		
          /**
          调试部分
          */
        cout << " cmp.size():"<< cmp.size()<< endl;
        for (int i = 0; i < cmp.size(); ++i)
        {
            cout << cmp[i] << "\t";
        }
        cout << endl;

        int maxV = v.at(cmp[0]).weight;      // 当前组中的体重最大值
        int maxI = 0;                        // 当前组中最大值的索引
        cout << "初始最大值:" << maxV << endl;
        for (int i = 0; i < cmp.size(); ++i){// 如果正好能全部分完
             int w = v.at(cmp[i]).weight;
             if ( w > maxV){
                maxV = w;
                maxI = i;
            }
             // }else{ // 被淘汰的,排名为该轮分组数+1
             //    v.at(cmp[i]).rank = turnG + 1;
             // }
                v.at(cmp[i]).rank = turnG + 1;
                cout << "更新rank：" << cmp[i] << "为" << turnG + 1<<endl;

          }
          cout << "最大值：" << maxV << "索引值" << cmp[maxI] << endl;
          // 将胜出的重新加如
          q.push(cmp[maxI]);
          cout << "本gourp加入" << cmp[maxI] << endl;
          cout << "q.size()" << q.size() << endl;
        cmp.clear();
      }
   }// end while when q.size == 1
   int campionIndex = q.front();
   cout << "campionIndex" << campionIndex << endl;
   v[campionIndex].rank = 1;

   cout << "结果：" ; 
   // 按格式输出结果
   cout << v[0].rank;
   for (int i = 1; i < NP; ++i) cout << " " << v[i].rank;
   cout << endl;
    return 0;
}
```

▲真的挺难的一题，首先是题目比较难理解，其次需要找到如果确定每个player的排名规律，最后再是每轮中处理不满一组的情况。

![1056](PAT冲冲冲——甲级/1056.jpg)





```cpp
/*
 * @Author: Mrli
 * @Date: 2020-09-18 23:02:14
 * @LastEditTime: 2020-09-18 23:10:24
 * @Description: int
 */
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
const int maxn = 1e5 + 5;

struct Node{
    int addr;
    int val;
    int nxt;
}arr[maxn];

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    vector<Node> v;
    int st, n, k;
    cin >> st >> n >> k;
    int addr;
    for (int i = 0; i < n; i++) {
       cin >> addr;
       cin >> arr[addr].val >> arr[addr].nxt;
       arr[addr].addr = addr;
    }

    int p = st;
    while( p != -1){
        v.push_back(arr[p]);
        p = arr[p].nxt;
    }

    int group = v.size() / k;
    for (int i = 0; i < group; i++) {
       reverse(v.begin() + i * k, v.begin() + i * k + k);
    }

    for (int i = 0; i < v.size(); i++) {
        printf("%05d %d ", v[i].addr, v[i].val);
       if ( i < v.size() - 1)
            printf("%05d\n", v[i+1].addr);
        else
            printf("-1\n");
    }
    
    return 0;
}
```



### 1057 [ Stack](https://pintia.cn/problem-sets/994805342720868352/problems/994805417945710592)

> 分块思想， 可以实时动态查询topK
>
> 碰到一个问题, `ios::sync_with_stdio(false);`使用后重定向的结果跟直接屏幕输入的结果不一致， [1077 [Kuchiguse]](#1077 [Kuchiguse])(同样也是因为这个原因过不了OJ

```c++
/*
 * @Author: Mrli
 * @Date: 2020-09-17 16:21:00
 * @LastEditTime: 2020-09-20 11:02:11
 * @Description: 
 */
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e5 + 5;
const int sqrN = sqrt(maxn);

const int INF = 0x3f3f3f3f;
int n;

stack<int> st;
int table[maxn];
int block[sqrN];

void peekMedian(int k){
    int sum = 0;
    int idx = 0;
    while( sum + block[idx] < k){
        sum += block[idx];
        idx += 1;
    }
    int num = idx * sqrN;
    while( sum + table[num] < k){
        sum += table[num];
        num += 1;
    }
    printf("%d\n", num);    
}


void Push(int x){
    st.push(x);
    block[ x / sqrN ] ++ ;
    table[x] ++;
}

void Pop(){
    int x = st.top();
    st.pop();
    block[ x / sqrN] --;
    table[x] --;
    cout << x <<endl;
}

int main(){
    cin >> n;
    string op;
    while(n -- ){
        cin >> op;
        if (op == "Pop"){
            if ( st.size() == 0) 
                cout << "Invalid" << endl;
            else Pop();
        }else if ( op == "PeekMedian"){
            int len = st.size();
            int k;
            if ( len == 0) 
                cout << "Invalid" << endl;
            else {
                if (len % 2 == 0){
                    k = len / 2;
                }else{
                    k = (len+1) / 2;
                }
                peekMedian(k);
            }
        }else if (op == "Push"){
            int tmp;
            cin >> tmp;
            Push(tmp);
        }
    }
    return 0;
}
```



### [1062 Talent and Virtue](https://pintia.cn/problem-sets/994805342720868352/problems/994805410555346944)

> 题意理解: 将输入的人分为三类，圣人sage，君子nobleman，愚人foolman和小人small man。输出他们的排名，规则如下，列出N行数据，L最低线，H最高限。sage为virtue品德和talent才能分数都高于H，他们之间的排名通过两者总分来区分；nobleman为talent才能低于H，但是virtue高于H的，同样也通过总分来区分，但是他们排在sage之后；如果两项得分都低于H，并且virtue不低于talent的为foolman，他们排在nobleman之后；剩下过了L线的人都排在foolman之后。（必须两个分数都高于L才能被显示）

```c++
#include <bits/stdc++.h>
using namespace std;
int n, L, H;

struct record{
    string ID;
    int de;
    int cai;
    record(string id, int d, int c): ID(id), de(d), cai(c){}
    // record(int id, int d, int c): ID(id), de(d), cai(c){}
};

bool cmp(const record &a, const record &b){
    if ( ( a.cai + a.de ) != ( b.cai + b.de )) return ( a.cai + a.de ) > ( b.cai + b.de );
    else if ( a.de != b.de) return a.de > b.de;
    else return a.ID < b.ID;
}

int main() {
    ios::sync_with_stdio(false);
    cin >> n >> L >> H;
    vector<record> v[4];
    int total = n;
    for (int i = 0; i < n; i++) {
        string tmpid;
        // int tmpid;
        int tmpde, tmpcai;
        cin >> tmpid >> tmpde >> tmpcai;
        record r = record(tmpid, tmpde, tmpcai);
        if (tmpde < L || tmpcai < L){
            total --;
        }else if ( tmpde >= H && tmpcai >= H){
            v[0].push_back(r);
        }else if ( tmpde >= H && tmpcai < H){
            v[1].push_back(r);
        }else if (tmpde < H && tmpcai < H && tmpde >= tmpcai){
            v[2].push_back(r);
        }else{
            v[3].push_back(r);
        }
    }    

    cout << total <<endl;
    for (int i = 0; i < 4; i++) {
       sort(v[i].begin(), v[i].end(), cmp);
       for (int j = 0; j < v[i].size(); j++) {
          cout << v[i][j].ID << " " << v[i][j].de << " " << v[i][j].cai<< endl;
       }
    }
    return 0;
}
```





### 1063[ Set Similarity](https://pintia.cn/problem-sets/994805342720868352/problems/994805409175420928)

> Nc表示两个集合中**相同的元素**(the number of distinct common numbers shared by the two sets)，Nt表示两个集合**总的不相同元素**的个数，

```c++
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;


int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >>n;
    // set<int> v[n];
    vector<int> arr(n+1);
    vector< set<int> > v(n+1);
    int m;
    for (int i = 1; i <= n; i++) {
       cin >> m;
       arr[i] = m;
       int tmp;
       for (int j = 0; j < m; j++) {
           cin >> tmp;
           v[i].insert(tmp);
       }
    }

    int k;
    cin >> k;
    for (int i = 0; i < k; i++) {
        int a, b;
        cin >> a >> b;

        set<int> res;
        set_intersection(v[a].begin(), v[a].end(), 
                                    v[b].begin(), v[b].end(), insert_iterator<set<int>>(res, res.begin()) );
        int total = v[a].size() + v[b].size() - res.size();
        
        printf("%.1lf%\n", res.size()*1.0 / total * 1.0 *100);
    }
    return 0;
}
```



### [1064 Complete Binary Search Tree](https://pintia.cn/problem-sets/994805342720868352/problems/994805407749357568)

> 一棵排序二叉树的中序遍历就是这一组数的递增序列。这边是完全二叉树，假设从0开始，那么节点i的左孩子的标号就是2*i+1，右孩子的标号就是2*(i+1)。先将这组数按照递增来排序，然后用中序遍历复原这棵完全排序二叉树，最后直接输出。

```c++
#include <bits/stdc++.h>
using namespace std;
vector<int> v;
vector<int> in;
int n;

int k = 0;
// 中序遍历复原这棵完全排序二叉树， 顺序输出in就是层次遍历
void inOrder(int root){
    if (root >= n) return ;
    inOrder(2*root + 1);
    in[root] = v[k++];
    // printf("%d ====%d\n",root,k-1);
    inOrder(2*root + 2);
}

int main(){
    cin >> n;
    v.resize(n);
    in.resize(n);
    
    for (int i = 0; i < n; i++) {
       cin >> v[i];
    }

    sort(v.begin(), v.end());

    inOrder(0);
    if (n > 1){
        cout << in[0] ;
        for (int i = 1; i < n; i++) {
            cout << " " << in[i];
        }
        cout << endl;
    }else{
        cout << in[0] <<endl;
    }
    return 0;
}
```

### 1077 [Kuchiguse](https://pintia.cn/problem-sets/994805342720868352/problems/994805390896644096)

> 碰到一个问题, `ios::sync_with_stdio(false);`使用后重定向的结果跟直接屏幕输入的结果不一致， [1057 [ Stack]](#1057 [ Stack])同样也是因为这个原因过不了OJ

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;


int main(){
    scanf("%d", &n);
    // cin跟getline(cin, s)配合会出问题， 因此用cin分开
    
    getchar();
     // 这边得用getchar吃掉换行符
    vector<string> v(n);
    int minLen = INF;
    for (int i = 0; i < n; i++) {
       getline(cin, v[i]);
       reverse(v[i].begin(), v[i].end());
       if (v[i].size() < minLen) minLen = v[i].size();
    }

    string ans = "";
    for (int i = 0; i < minLen; i++) {
        char ch = v[0][i];
        bool allmatch = true;
       for (int j = 1; j < n; j++) {
          if ( v[j][i] != ch){
              allmatch = false;
              break;
          }
       }
       if (allmatch == true) {
           ans = ch + ans;
       }

       else break;
    }
    if (ans.empty()) cout << "nai" << endl;
    else cout << ans << endl;
    return 0;
}
```





### [ 1078 Hashing](https://pintia.cn/problem-sets/994805342720868352/problems/994805389634158592)

> Hash题，一开始审题错误，是平方探测法。
>
> 不算难，转化大小很简单，主要在这个平方探测上面。 **是(key + step \* step) % size 而不是(key % size + step \* step)**， 知道了这个，就比较好办了。

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 10e6;
int M, N;
std::vector<int> in;    // 记录输入, 查询相应位置是否有数
std::vector<int> v;     // 用来记录相应数的索引位置, 输出值: v[0]就是第一个数的hash值

bool isprime(int n){
    if ( n== 1) return false;
    if ( n == 2 || n == 3) return true;
    if ( (n%6!= 1) && (n%6!= 5)) return false;
    int tmp = sqrt(n);
    for (int i = 5; i <= tmp; i+=6){
        if ( n % i ==0 || n%(i+2) == 0) return false;
    }
    return true;
}

int main(int argc, char const *argv[]){
    cin >> M >> N;
    while(!isprime(M)) M++;	// ▲将数扩大到最小质数
    // cout << "最小的质数为:" << M << endl;
    v.resize(N);
    in.resize(M);   // 这句很重要
    for (int i = 0; i < N; ++i){
        int tmp;
        cin >> tmp;
        bool yes = false;
        for (int j = 0; j < M; ++j){
            int index = (tmp+j*j)%M;
            if ( in[index] == 0){
                v[i] = index;
                in[index] = tmp;
                yes = true;
                break;
            }
        }        
        if (!yes) v[i] = -1;
        /** 为二次再散列法*/
        // if(in[index] == 0){ // 如果没有冲突, 则安放
        //     v[i] = index;
        //     in[index] = tmp;
        //     // cout << "yes" <<endl;
        // }else{
        //     // cout << "no" <<endl;
        //     // 二次
        //     int new_index = index%M;
        //     if (in[new_index] ==0){
        //         v[i] = new_index;
        //         in[new_index] = tmp;
        //     }else{
        //         v[i] = -1;
        //     }
        // }
    }

    // 输出处理
    if(v[0] == -1)cout << "-";
    else cout << v[0];
    for (int i = 1; i < N; ++i){
        if(v[i] == -1) cout << " -";
        else cout << " " << v[i];
    }
    cout <<endl;
    return 0;
}
```



### [1067 Sort with Swap(0, i)](https://pintia.cn/problem-sets/994805342720868352/problems/994805403651522560)

> 贪心: 将0与0当前所在位置下标的数交换, 如0在4位置, 那么就与4交换。而一个数一旦被交换过后就不会再需要被交换了

```cpp
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;

int main(){
    int n ;
    int ans = 0;
    scanf("%d", &n);
    vector<int> v;
    v.resize(n);
    int left = n - 1;       // 除0以外已在自己位置上的
    int k = 1;              // 尚未归位的位置, 默认从第0+1位置
    for (int i = 0; i < n; i++) {
        int pos;
        scanf("%d", &pos);
        v[pos] = i;         		// pos位置为i
        if (pos == i && pos != 0){	// left定义就是除0外未归位的
            left --;
        }
    }

    while(left > 0){
        if (v[0] == 0){
            while(k < n){
                if (v[k] != k){				// 找到下个未归位的下标
                    swap(v[0], v[k]);
                    ans ++;
                    break;                   
                }
                k++;
            }
        }

        while(v[0] != 0){				// 0不在0位上
            swap(v[0], v[v[0]]);
            ans ++;
            left --;
        }
    }
    printf("%d\n", ans);

    return 0;
}

```



### [1069 The Black Hole of Numbers](https://pintia.cn/problem-sets/994805342720868352/problems/994805400954585088)

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;

string sub(string a, string b){
    int ansn = stoi(a) - stoi(b);
    string s = to_string(ansn);
    while(s.size()!=4) s = "0" + s;
    return s;
}

void solution(string n){
    string s = n;
    string b = n;
    string tmpans;
    while(true){
        sort(s.begin(), s.end(), greater<char>());   // string比较用char                    
        string revs(s.rbegin(), s.rend());    
        tmpans = sub(s, revs);
        cout << s << " - " << revs << " = " << tmpans << endl;
        s = tmpans;
        if (tmpans == "0000" || tmpans == "6174") break;
    }
}

int main(){
    string s;
    cin >> s;
    while( s.size() < 4) s = "0" + s;		// 第2、3、4个点
    solution(s);
    return 0;
}

```



### [1070 Mooncake](https://pintia.cn/problem-sets/994805342720868352/problems/994805399578853376)

> 最简单的贪心: 拿单价最高的月饼种类

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;



struct mooncake{
    double tons;			// 题目只是说正数, 没说正数, 第3个测试样例会卡这个
    double totalprice;
    double singleprice;
};


bool cmp(const mooncake &a, const mooncake &b){
    return a.singleprice > b.singleprice;
}

int main(){
    int n, d;
    scanf("%d%d", &n, &d);
    vector<mooncake> v(n);
    for (int i = 0; i < n; i++) {
       scanf("%lf", &v[i].tons);
    }

    for (int i = 0; i < n; i++) {
       scanf("%lf", &v[i].totalprice);			// 输入用%lf, 输出用%f
       v[i].singleprice = v[i].totalprice / v[i].tons;
    }
    sort(v.begin(), v.end(), cmp);
    double maxProfit = 0;

    int len = v.size();
    for (int i = 0; i < len ; i++) {
        if ( d ){
            if ( d > v[i].tons ) {
                d -= v[i].tons;
                maxProfit += v[i].totalprice;
            }else{
                maxProfit += v[i].singleprice * d;
                d = 0;
            }
        }else break;
    }
    printf("%.2f\n", maxProfit);

    return 0;
}

```



### 1073[ Scientific Notation](https://pintia.cn/problem-sets/994805342720868352/problems/994805395707510784)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;

// 测试样例
// -3.1415E+004
// -3.1415926E+4
// +3.1415926E-01
// -3.1415926E-0005
// -2.32E+0

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    string s;
    cin >> s;
    int len = s.size();

    // s[0] 符号位, s[1]整数位, s[2]小数点

    int dot = 2;
    int epos = s.find("E");
    int expSigPos = epos + 1;
    int exp = 0;

    // 越过指数符号位, 计算指数
    for (int i = expSigPos + 1; i < len; i++) {
       exp = exp*10 + (s[i] - '0');
    }

    if ( exp == 0){
        cout << "1";
        return 0;
    }

    if ( s[0] == '-') cout << "-";
    // cout <<exp << endl;
    if ( s[expSigPos] == '-'){      // 负指数
        cout << "0.";
        for (int i = 0; i < exp - 1; i++) {     // 补0
           cout << "0";
        }
        for (int i = 1; i < epos; i++) {
            if ( i == dot) continue;
           cout << s[i];
        }
    }else{
        // 小数点和E之间数字的个数 (dot, epos)
        int nums = epos - dot - 1;

        // 是否有小数点
            // 无小数点情况
        if ( exp - nums >= 0){  
            for (int i = 1; i < epos; i++) {
               if ( i == dot) continue;
               cout << s[i];
            }

            for (int j = 0; j < exp - nums; j++) {     // 
               cout << "0";
            }
        }else{          // 第4个样例
            // 有小数点情况
            for (int i = 1; i < epos; i++) {
               if ( i == dot) continue; // 略过小数点
               if ( i == exp + dot + 1) cout <<".";
               cout << s[i];            // 出错过一次, 这边不应该是else
            }
        }
    }
    cout << endl;
    return 0;
}
```





### [1076 Forwards on Weibo](https://pintia.cn/problem-sets/994805342720868352/problems/994805392092020736)

> 一道图的基础遍历题。英文题目有点难懂。意思是N个点，L层深度最多能遍历到几个点。由于有关注和被关注的关系，所以是有向图。接下来有N行，分别表示用户i有M[i]个关注，因此M[i]个user[j]转发的文章可以被用户i再次转发。最后一行是回答K个问题，分别是K[i]发文章在L的限制内最多被几个人转发

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 10e4 + 5; 

struct Node
{
    int id;         // 当前节点的编号
    int layer;      // 当前的层数
}typedef Node;

std::vector<Node> G[MAXN];
bool visited[MAXN] = {false};

/**
 * [BFS description]
 * @author mrli 2020-02-21
 * @param  s [起点]
 * @param  L [最大深度]
 */
int BFS(int s, int L){
    int ans = 0;
    queue<Node> q;

    Node tmp;
    tmp.id = s;
    tmp.layer = 0;
    q.push(tmp);        // 将起点压入队列
    visited[s] = true;  // 记得对起点做设置
    
    while(!q.empty()){
        tmp = q.front();
        q.pop();
        int u = tmp.id;
        for (int v = 0; v < G[u].size(); ++v){  // BFS层次遍历该节点的所有结点
            Node next = G[u][v];
            next.layer = tmp.layer + 1;		// 用BFS的形式来维护深度
            if( visited[next.id] == false && next.layer <= L ){   // 如果是第一次被访问, 且在L深度内, 则答案++
                q.push(next);    
                visited[next.id] = true;	// △对访问过的设置不能忘记
                ans ++ ;
                // cout << "next.id: " << next.id <<'\t';
            }
        }

    }
    return ans;
}

int main(int argc, char const *argv[]){
    int N, L;
    cin >> N >> L;
    Node user;

    for (int i = 1; i <= N; ++i){    // 编号从1开始->根据题目K问题的形式确定 
        user.id = i;
        int follownum;  // 跟随j的用户人数
        cin >> follownum;
        for (int j = 0; j < follownum; ++j){
            int follow;
            cin >> follow;
            G[follow].push_back(user);
        }
    }

    // answer to the question
    int numQuery, s;
    cin >> numQuery;
    for (int i = 0; i < numQuery; ++i){
        cin >> s;
        memset(visited, false, sizeof(visited));
        int ans = BFS(s, L);
        cout << ans <<endl;
    }
    return 0;
}
```



###  [1081  Rational Sum](https://pintia.cn/problem-sets/994805342720868352/problems/994805386161274880) 

> 分数计算的加强版，多个分数相加。我采用了一次性计算，其实可以直接用乙级的做法，两个两个依次计算。
>
> ▲牛客网和PTA的样例点真的不一样，PTA上我有一个点过不了，但牛客的都能过

```c++
#include <bits/stdc++.h>
typedef long long ll;
#define INF 0x3f3f3f
using namespace std;

struct Fenshu{
    ll fenmu;
    ll fenzi;
}typedef Fenshu;

// 找到分子和分母的最大公因数
ll biggestNum(ll a, ll b){
    if (b==0) return a;
    return biggestNum(b,a%b);
}

// 找到分子和分母的最小公倍数
ll smallestNum(ll a,ll b){
    return a*b/biggestNum(a, b);
}


int main(int argc, char const *argv[])
{
    int n;
    std::vector<Fenshu> v;
    while(scanf("%d",&n) != EOF){
        for (int i = 0; i < n; ++i){
            Fenshu *fs = new Fenshu();
            scanf("%lld/%lld",&fs->fenzi,&fs->fenmu);
            v.emplace_back(*fs);
        }
        // 找到所有分母的最小公倍数
        ll mul = v.begin()->fenmu;
        for (std::vector<Fenshu>::iterator i = v.begin(); i != v.end(); ++i){
            std::vector<Fenshu>::iterator nx = std::next(i,1);
            if (nx != v.end()){
                mul = smallestNum(mul, nx->fenmu);
                // printf("i->fenmu:%lld, nx->fenmu:%lld, tmp_mul:%lld\n", i->fenmu, nx->fenmu, tmp_mul);
				// 错误尝试写法：思路错了:变成了找到两个分母最小公倍数中最大的
                // if (mul < tmp_mul) {
                    // printf("tmp_biggest:%lld\n", tmp_biggest);
                    // mul = tmp_mul;
                // }
            }
        }
        // 相加再约分
        ll sum = 0;
        for (std::vector<Fenshu>::iterator i = v.begin(); i != v.end(); ++i){
            // printf("%lld,%lld\n",i->fenzi, i->fenmu );
            sum +=  i->fenzi*mul/(i->fenmu);
        }

        if (sum == 0){  // 采坑1:除0问题
            printf("0\n");
        }else{
            // 由于读取的设定,所以其实只有分子会是负数, yue也可能是负数
            ll yue = abs(biggestNum(sum, mul));
            ll res_fenzi = sum/yue;
            ll res_fenmu = mul/yue;
            // printf("sum:%lld\n", sum);
            // printf("%lld,%lld/%lld\n", yue, res_fenzi, res_fenmu);
            // printf("biggest:%lld\n", mul);
            // 需要化成真分数-->采坑2:分子和为负数
            if ( abs(res_fenzi) > res_fenmu) {
                if (res_fenzi%res_fenmu == 0) printf("%lld\n", res_fenzi/res_fenmu);
                else printf("%lld %lld/%lld\n", res_fenzi/res_fenmu, res_fenzi%res_fenmu, res_fenmu);
            }else
                printf("%lld/%lld\n", res_fenzi, res_fenmu);

        }
        v.clear();
    }
    return 0;
}
```

▲判断迭代器是否为空:就是拿返回的迭代器与.end()作比较。

踩坑记录：

- 负数求余仍是负数,0求余任何数为0
- 分子为负数、0、正数的时候都得分别考虑
- 找到所有分母的最小公倍数==>写成了找到两个分母最小公倍数中最大的

**浮点错误的意思-PAT 、OJ**

- 是否可能出现了一个数除以0的情况
- 是否可能出现了一个数取余0的情况
- 是否发生了数据溢出而导致的除以0或者取余0的情况



###  [1083 List Grades (25)](https://www.nowcoder.com/pat/1/problem/4313) 

> 模拟题
>
> 考了输入输出+排序： 切割数据、操作符重载

```c++
#include <bits/stdc++.h>
#include <sstream>
#include <algorithm>

#ifndef N
#define N 1000
#endif

using namespace std;

struct Record{
    string name;
    string id;
    int  grade;

    // 操作符重载
    bool operator <(const Record &that) const{
        return grade > that.grade;
    }
}typedef Record;

int main(){
    
    string row;
    std::vector<Record> v;
    int n;
    int a,b;
    scanf("%d",&n);
    getchar();
    for(int i=0;i<n;i++){
        // 获得行信息
        getline(cin, row);
        stringstream ss(row);
		// 分割出name
        string name;
        getline(ss, name, ' ');
		// 分割出id
        string id;
        getline(ss, id, ' ');
		// 通过stringstream分割出grade: int
        int grade;
        ss >> grade;
		// 存到vector中
        Record* r = new Record();
        r->name = name;
        r->id = id;
        r->grade = grade;
        v.emplace_back(*r);
    }
	// 根据grade进行从大到小排序
    sort(v.begin(), v.end());
    scanf("%d%d", &a, &b);
    bool none = true; // 是否有在区间中人
    for (int i = 0; i < n; ++i){
        if (v[i].grade >= a && v[i].grade <= b){
            none = false;
            cout << v[i].name << " " << v[i].id << endl;
        }
    }
    // 如果没有符合条件的人
    if (none) cout << "NONE" << endl;
    return 0;
}
```

在此巩固复习一下"操作符重载的知识":

> 1.为了实现对自定义类型的加减操作。
>
> 2. 实现一个操作符重载的方式通常分为两种情况： 
>
> - 将操作符重载实现为类的成员函数；
>
>   - 使用` O.operator#(); `
>
> - 操作符重载实现为非类的成员函数（即全局函数）。
>
>   - 使用 `operator#(O); `
>
>   区别在于，成员函数默认有this指针；后者需要为每次操作传递两个参数 

△大多数操作符都能重载，不能的为如下几个：`：`、`::`、`.*`、`?:`、`sizeof`

▲重载运算符函数可以对运算符作出新的解释，但原有基本语义不变：
>不改变运算符的优先级
>不改变运算符的结合性
>不改变运算符所需要的操作数
>不能创建新的运算符

△一个运算符被重载后，原有意义没有失去，只是定义了相对一特定类的一个新运算符



++前缀、后缀

```c++
class Time{
    public:
	// 重载前缀递增运算符（ ++ ）： 不传参
      Time operator++ ()  
      {
         ++minutes;          // 对象加 1
         if(minutes >= 60)  
         {
            ++hours;
            minutes -= 60;
         }
         return Time(hours, minutes);
      }
      // 重载后缀递增运算符（ ++ ）: 传参
      Time operator++( int )         
      {
         // 保存原始值
         Time T(hours, minutes);
         // 对象加 1
         ++minutes;                    
         if(minutes >= 60)
         {
            ++hours;
            minutes -= 60;
         }
         // 返回旧的原始值
         return T; 
      }
}

```



io操作符：

> ▲`<<`操作符只能通过友元来实现
>
> A： 如果要重载<<操作符输出结果，一般的写法是cout<<s;也即是说左侧不是**成员函数**或**类可以通过this指针调用的量**，这就造成必须使用两个参数的成员操作符重载，把第一个参数作为<<左侧参数，第二个参数做为<<右侧参数输入，然而会发现如： ` ostream& operator<<(ostream& out, MyString& s);*//报错，error:此运算符的参数太多* `

```c++
class xxx{
    friend ostream& operator<<(ostream &out, const Complax &c1);
}

ostream& operator<<(ostream &out, const Complax &c1){
    out << "c1.a = " << c1.a << "\t c1.b = " << c1.b << endl;
}
```

### 

### [1084 Broken Keyboard](https://pintia.cn/problem-sets/994805342720868352/problems/994805382902300672)

> 思路是遍历下面的短的字符串，然后用指针再遍历长的，用index指针来手动控制；边遍历边输出，就可以解决先后问题，否则用set会排序；用set记录是否已经输出过；
>
> 踩的坑点： `set.find() == set.end()`表示不存在，写IF条件的时候想反了，检查了半天；如果长的已经把短的所有都跑遍后，之后还有需要吧index继续跑完slen-inlen的长度

```c++
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 100+5;
std::set<char> cS;

int main(int argc, char const *argv[]){
    string s;
    cin >> s;
    string in;
    cin >> in;
    int slen = s.length();
    int inlen = in.length();
    
    int index=0;	// 记录当前遍历到的位置
    for (int i = 0; i < inlen; ++i){
        while(  s[index] != in[i] ){
                char now = toupper( s[index] );
                if( cS.find(now) == cS.end()){   
                    cS.insert(now);
                    cout << now;
                }
            index++;
        }
        
        index++;
    }
    while(index != slen){
            char now = toupper( s[index] );
            if( cS.find(now) == cS.end()){   
                cS.insert(now);
                cout << now;
            }
        index++;
    }
    cout <<endl;
    return 0;
}
```



```java

import java.lang.reflect.Array;
import java.util.Arrays;
import java.util.Scanner;

/**
 * @Program: testIDEA
 * @Description:
 * @Author: MrLi
 * @Create: 2020-05-26 15:28
 **/

public class Main {
    public static void main(String[] args) {
        String a, b;
        Scanner scanner = new Scanner(System.in);

        a = scanner.next();
        b = scanner.next();
        b += "#";			// 如果b为全空, 那么直接就会越界, 因此末尾补充一个, 这题由于上下有关联所以没关系, 但是对此类问题都可以养成这种习惯
        boolean[] chars = new boolean[256];
        Arrays.fill(chars, Boolean.FALSE);
        for (int i = 0, j = 0; i < a.length(); i++) {
            char x = Character.toUpperCase(a.charAt(i));
            char y = Character.toUpperCase(b.charAt(j));
            if (x == y) j ++ ;
            else{
                if (!chars[x]){
                    System.out.print(x);
                    chars[x] = true;
                }
            }
        }
    }
}

```



### [1086 Tree Traversals Again](https://pintia.cn/problem-sets/994805342720868352/problems/994805380754817024)

> 建树老套路了, 只不过要识别出, 入栈对应先序, 出栈对应中序

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

vector<int> pre;
vector<int> in;
int noden;              // 节点个数

struct node{
    int val;
    node *lc, *rc;
    node(){}
    node (int v){
        val = v;
        lc = rc = NULL;
    }
};

node *createTree(int preL, int preR, int inL, int inR){
    if (preL > preR) return NULL;           // 这边忘记了写错成preL > inL了
    
    int key = pre[preL];
    int k;
    for (k = inL; k <= inR; k++) {   // 找到根节点在中序中的位置
       if (in[k] == key){
           break;
       }
    }
    // cout << "key: " << key  << " k: " << k+1 << endl;
    node *root = new node(key);
    root->val = key;
    int leftNum = k - inL;  // 当前节点左边有多少节点个数

    root->lc = createTree(preL+1, preL+leftNum, inL, k - 1);
    root->rc = createTree(preL+leftNum+1, preR, k+1, inR);

    return root;
}


int coutNum = 0;    //   已经输出的节点数
void postOrder(node *root){
    if (root == NULL) return;
    
    postOrder(root->lc);
    postOrder(root->rc);
    // 官方输出格式
    // cout << root->val;
    // coutNum ++;
    // if (coutNum < noden){
    //     cout <<" " ;
    // }else{
    //     cout <<endl;
    // }
    coutNum ++;
    if (coutNum < noden){
        cout << root->val << " ";
    }else{
        cout << root->val <<endl;
    }
}

int main(){

    int nodenum;            // push操作数
    int num;                // pop结果
    string op;
    stack<int> st;

    cin >> noden;
    for (int i = 0; i < 2 * noden; i++) {
       cin >> op;
       if (op == "Push"){
           cin >> nodenum;
           pre.push_back(nodenum);
           st.push(nodenum);
       }else if (op == "Pop"){
           num = st.top();
           st.pop();
           in.push_back(num);
       }
    }


    node *root = createTree(0, pre.size() - 1,0, in.size() - 1);
    postOrder(root);
    return 0;
}

```





### [1091 Speech Patterns](https://pintia.cn/problem-sets/994805342720868352/problems/994805398257647616)

> 题目不难， 主要是库函数的应用`isdigit()`、`isalpha`以及`isalnum`，还有map的应用
>
> 坑点比较多： 1.“”（即空会被记入map计算）；2.当已经是最后一个字母时要把最后一个单词记入（想了很久没过最后一个测试点）
>
> 本来以为要统计的是""内的内容，结果好像不需要

```c++

#include <bits/stdc++.h>
using namespace std;
const int MAXN = 0x3f3f3f3f;
const int MOD = 1000000007;
std::map<string, int> smap;

int main(int argc, char const *argv[]){
    string s;
    getline(cin, s);
    int slen = s.length();
    string nows;
    // bool start = false;
    for (int i = 0; i < slen; ++i)
    {
        char now = s[i];
        // if (now == '"') start = !start;
        // if (start){
            if (isalnum(now)){
                nows += tolower(now);
            }
            if ( !isalnum(now) || i == slen-1){
                if(!nows.empty()){	// nows.size()会更好
                    smap[nows] += 1;
                    nows.clear();
                }
            }
        // }
    }
    

    int maxn = -MAXN;
    string ans;
    for (std::map<string, int>::iterator i = smap.begin(); i != smap.end(); ++i){
        if (  i->second > maxn){
            maxn = i->second;
            ans = i->first;
        }
    }
    cout << ans << " " <<  maxn << endl;
    return 0;
}
```





### [1093 Count PAT's](https://pintia.cn/problem-sets/994805342720868352/problems/994805373582557184)

> 卡时限，普通思路会超时， 题解给的思路是找A然后计算前P的个数N，后T的个数M，然后得出M*N个，时间复杂度为O(n2)

个人的思路(过2个点，还有3个超时)

```c++
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 0x3f3f3f3f;
const int MOD = 1000000007;

std::vector<int> P;
std::vector<int> A;
std::vector<int> T;


int main(int argc, char const *argv[])
{
    string s;
    cin >> s;
    int l;
    l = s.length();
    for (int i = 0; i < l; ++i)
    {
        if (s[i]=='P') P.push_back(i);
        else if (s[i]=='A') A.push_back(i);
        else if (s[i]=='T') T.push_back(i);
        else ;
    }

    int pl = P.size();
    int al = A.size();
    int tl = T.size();
    int ans = 0;
    for (int i = 0; i < pl; ++i){
        for (int j = 0; j < al; ++j){
            if (P.at(i)<A.at(j)){
                for (int k = 0; k < tl; ++k)
                {
                    if (A.at(j) < T.at(k)){
                        ans = (ans+1) %MOD;
                        // break;
                    }        
                }    
            }
        }
    }
    cout <<ans<< endl;
    return 0;
}

```

[ Deduplication on a Linked List](https://pintia.cn/problem-sets/994805342720868352/problems/994805369774129152)

```cpp

/*
 * @Author: Mrli
 * @Date: 2020-06-27 10:52:28
 * @LastEditTime: 2020-06-30 23:58:52
 * @Description: 
 */ 

#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1e5 + 5;
vector<bool> visited(MAXN);


struct Node{
    int addr;
    int val;
    int nextaddr;
}LinkList[MAXN];

vector<Node> leftvec;
vector<Node> removed;

int main(){
    int root, N;
    scanf("%d%d", &root, &N);

        int addr;
    for (int i = 0; i < N; i++) {
        scanf("%d", &addr);
        scanf("%d%d", &LinkList[addr].val, &LinkList[addr].nextaddr);
        LinkList[addr].addr = addr;
    }

    int start = root;
    while(start != -1){
        int value = int(abs(LinkList[start].val));
        if ( !visited[ value ])
            leftvec.push_back(LinkList[start]);
        else
            removed.push_back(LinkList[start]);
        visited[value] = true;
        start = LinkList[start].nextaddr;

    }

    int leftLen = leftvec.size();
    int removedLen = removed.size();
    for (int i = 0; i < leftLen; i++) {
       if ( i != leftLen - 1){
           printf("%05d %d %05d\n", leftvec[i].addr, leftvec[i].val, leftvec[i+1].addr);
       }else{
           printf("%05d %d -1\n", leftvec[i].addr, leftvec[i].val);
       }
    }
    for (int i = 0; i < removedLen;i++) {
       if ( i != removedLen - 1){
           printf("%05d %d %05d\n", removed[i].addr, removed[i].val, removed[i+1].addr);
       }else{
           printf("%05d %d -1\n", removed[i].addr, removed[i].val);
       }
    }
    
    return 0;
}

```

#### 链表套路

> 写了几个静态链表的题了, 总结下套路:

- 判断给的节点是否在链表上——增加一个flag的字段。 如果不需要一般静态链表的节点定义为

  ```cpp
  struct Node{
      int addr;
      int val;
      int nextaddr;
  }LinkList[MAXN];
  ```

  

- 读取套路

  ```cpp
      for (int i = 0; i < N; i++) {
          scanf("%d", &addr);
          scanf("%d%d", &LinkList[addr].val, &LinkList[addr].nextaddr);
          LinkList[addr].addr = addr;
      }
  ```

- 遍历

  ```cpp
      int start = root;
      while(start != -1){
          int value = int(abs(LinkList[start].val));
          if ( !visited[ value ])
              leftvec.push_back(LinkList[start]);
          else
              removed.push_back(LinkList[start]);
          visited[value] = true;
          start = LinkList[start].nextaddr;
      }
  ```

- 输出

  ```cpp
      for (int i = 0; i < leftLen; i++) {
         if ( i != leftLen - 1){
             printf("%05d %d %05d\n", leftvec[i].addr, leftvec[i].val, leftvec[i+1].addr);
         }else{
             printf("%05d %d -1\n", leftvec[i].addr, leftvec[i].val);
         }
      }
  ```


### [1096 Consecutive Factors](https://pintia.cn/problem-sets/994805342720868352/problems/994805370650738688)

> 一个比较重要的点: n不能被大于sqrt(n)的整除

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-07-01 23:25:13
 * @LastEditTime: 2020-07-02 00:19:04
 * @Description: 
 */ 
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;
typedef long long ll;


int main(){
    ll n;
    scanf("%lld", &n);
    ll sqrtn = (ll)sqrt(n);
    ll ansI = 0, maxlen = 0;			// 注意这边maxlen不能设置为-1, 因为下面是用0判断的
    for (ll i = 2; i <= sqrtn; i++) {
        ll j = i;
        ll tmp = 1;
        while(true){
            tmp *= j;
            if ( n % tmp != 0) break;
            if (j - i + 1 > maxlen){
                maxlen = j - i + 1;
                ansI = i;
            }
            j ++;
        }
    }
    if ( maxlen == 0) printf("1\n%lld\n", n);
    else{
        printf("%lld\n", maxlen);
        for (ll i = 0; i < maxlen; i++) {
            if ( i < maxlen - 1){
                printf("%lld*", ansI + i);
            }else{
                printf("%lld", ansI + i);
            }

        // 第二种输出
        // for (ll i = 0; i < maxlen; i++) {
        //     printf("%lld", ansI + i);
        //     if ( i < maxlen - 1)   printf("*");
        // }                
        }
    }
    return 0;
}

```



###  [ 1099  Build A Binary Search Tree](https://pintia.cn/problem-sets/994805342720868352/problems/994805367987355648) 

> // 跟1064对比, 这边是真遍历。 v为存储结点信息的树。 那题是完全二叉树， 这题是二叉搜索树， 所以在层次遍历的构造上是不一样的	

```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
struct Node{
    int val;
    int left, right;
    
};
int n;
vector<Node> v;
vector<int> in;


bool cmp(const Node a , const Node b ){
    return a.val < b.val;
}

int k = 0;
// 跟1064对比, 这边是真遍历。 v为存储结点信息的树
void inOrder(int root){
    if (root == -1) return ;
    inOrder(v[root].left);
    v[root].val = in[k++];
    inOrder(v[root].right);
}

void bfs(int root){
    queue<int> q;
    q.push(root);
    k = 0;
    while(!q.empty()){
        int now = q.front();
        q.pop();
        cout << v[k].val;
        k++;
        if ( k < n) cout << " ";
        if (v[now].left != -1) q.push(v[now].left);
        if (v[now].right != -1) q.push(v[now].right);
    }
}

int main(){
    cin >> n;
    v.resize(n);
    in.resize(n);

    for (int i = 0; i < n; i++) {
        cin >> v[i].left >> v[i].right;
    }
    for (int i = 0; i < n; i++) {
       cin >> in[i];
    }


    sort(in.begin(), in.end());
    
    inOrder(0);

    bfs(0);

    return 0;
}
```







### [1102 Invert a Binary Tree](https://pintia.cn/problem-sets/994805342720868352/problems/994805365537882112)

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-07-19 11:28:37
 * @LastEditTime: 2020-07-19 14:22:32
 * @Description: 
 */ 

#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int n;
const int maxn = 1e2;

bool isRoot[maxn];
struct Node{
    int left;
    int right;
}Nodearr[maxn];


int chr2int(char ch){
    if ( ch == '-' ) return -1;
    else {
        isRoot[ ch - '0'] = false;
        return ch - '0';
    }
}


int findRoot(){
    for (int i = 0; i < n; i++) {
       if ( isRoot[i] == true){
           return i;
       }
    }
    return -1;
}





void postOrder(int root){
    if (root == -1) return ;
    // if ( Nodearr[root].left != -1 )
     postOrder(Nodearr[root].left);
    // if ( Nodearr[root].right != -1 )
     postOrder(Nodearr[root].right);
    swap(Nodearr[root].right, Nodearr[root].left);
}


int cnt = 0;
void print(int idx){
    if ( cnt == n-1){
        cout << idx << endl;
    }else {
        cout << idx << " ";
    }
    cnt ++;
}

void inOrder(int root){
    if (root == -1) return ;

    inOrder(Nodearr[root].left);
    print(root);
    inOrder(Nodearr[root].right);
}

void levelOrder(int root){
    queue<int> q;
    q.push(root);
    while( !q.empty() ){
        int now = q.front();
        q.pop();
        print(now);
        if ( Nodearr[now].left != -1 ) q.push(Nodearr[now].left);
        if ( Nodearr[now].right != -1 ) q.push(Nodearr[now].right);
    }
}

int main(){
    scanf("%d", &n);
    fill(isRoot, isRoot+n, true);
    for (int i = 0; i < n; i++) {
        char left, right;
        scanf("%*c%c %c", &left, &right);
        Nodearr[i].left = chr2int(left);
        Nodearr[i].right= chr2int(right);
    }

    int root = findRoot();
    postOrder(root);
    
    levelOrder(root);
    cnt = 0;
    inOrder(root);
    
    return 0;
}
```



### [1107 Social Clusters](https://pintia.cn/problem-sets/994805342720868352/problems/994805361586847744)

> 并查集

```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1000 + 5;

int n;
int arr[maxn];  // 并查集
int h[maxn];    // 爱好
int hx[maxn];    // 爱好

void init(){
    for (int i = 0; i <= n; i++) {
    arr[i] = i ;
    }
}

int getf(int u){
    // 找到u的父亲
    int v   = u;
    while( u != arr[u]){
        u = arr[u];
    }

    // 路径压缩
    while ( arr[v] != v){
        int tmp = v;        // 记录当前v的值
        v = arr[v];         // 记录下一个点->v
        arr[tmp] = u;       // 把当前点的父亲设置为u
    }
    return u;
}


void merge(int u, int v){
    int f1 = getf(u);
    int f2 = getf(v);
    if (f1 != f2){
        arr[f2] = f1;
    }
}


int main(){
    int tmp;
    scanf("%d", &n);
    init();
    for (int i = 1; i <= n; i++) {
    int k;
    scanf("%d:", &k);
    for (int j = 0; j < k; j++) {
        scanf("%d", &tmp);
        if (!h[tmp]) h[tmp] = i;
        else
                merge(h[tmp], i);
        }
    }


    int ans = 0;
    for (int i = 1; i <= n; i++) {
        hx[getf(i)]++;
    }
    for (int i = 1; i <= n; i++) {
       if (hx[i]!=0)ans++;
    }

    // 直接用arr不行, 必须用getf找, 对样例是都能跑通的, 但会挂3个点。网上也都是用getf找的父亲， 而不是用arr
    // for (int i = 1; i <= n; i++) {
    //     cout << "arr[" << i << "]:" << arr[i] << '\t';
    //     cout << "getf(i" << i << "):" << getf(i) << endl;
    // }
    
    //     int ans = 0;									
    //     for (int i = 1; i <= n; i++) {
        //     if ( arr[i]  == i ){
        //         ans ++;
        //     }
    //     hx[arr[i]] ++;
    // }
    cout << ans <<endl;
    sort(hx+1, hx+1+n, greater<int>());

    for (int i = 1; i <= ans; i++)
    {
        cout << hx[i];
        if (i < ans) cout << ' ';
    }
    return 0;
}
```



### 1109[Group Photo](https://pintia.cn/problem-sets/994805342720868352/problems/994805360043343872)

> 排序, 模拟
>
> 思路: 从排好序的vecotr中取元素

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n, k;
const int maxn = 1e4 + 5;

struct Student{
    string name;
    int h;
    Student(){};
    Student(string _name, int _h): name(_name), h(_h){};
}arr[maxn];

// 1. 每行n/k（向下取整）人， 多出的剩下的人数站最后一行
// 2. 后面排的身高 >= 前面排的, 字典序大的前
// 3. 最高的站中间， 两边依次降低
bool cmp(Student &a, Student &b){
    if (a.h != b.h)
        return a.h < b.h;
    return a.name > b.name;
}


// ▲.关键
void printLine(int st, int ed){
    int nums = ed - st + 1;        // 数的个数
    int mid = (nums) / 2 + 1;       // 中间编号, v: [1,ed]
    vector<int> v( nums + 1, 0);

    v[mid] = ed;					// 中间的序号为ed
    int l = mid - 1, r = mid + 1;
    while( l >= 1){
        // 实现交替--ed
        if (l >= 1)     v[l--] = --ed;	// 左侧依次-1
        if (r <= nums)  v[r++] = --ed; 
    }
	
    // 从arr中按照v[i]取出名字
    for (int i = 1; i <= nums; i++) {
        if ( i == 1)
        cout << arr[ v[i] ].name;
        else cout << " " << arr[ v[i] ].name ; 
    }
    cout << endl;
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    cin >> n >> k;
    string name;
    int h;
    for (int i = 1; i <= n; i++) {
        cin >> name >> h;
        arr[i] = Student(name, h);
    }

    sort(arr + 1, arr+ n + 1, cmp);
    int rowNum = floor(n / k);

    for (int i = k; i >= 1; i--) {
        if ( i < k){              
            int st = rowNum * ( i - 1) + 1;
            int ed = rowNum * i;
            // cout << "st: " <<st  << "\t" << "ed:" << ed <<endl;
            printLine(st, ed);
        }else{                           // 第一行
            int st = rowNum * ( i - 1) + 1;
            int ed = n;
            // cout << "st: " <<st  << "\t" << "ed:" << ed <<endl;
            printLine(st, ed);
           }
    }
    
    return 0;
}
```



### 1110 [Complete Binary Tree](https://pintia.cn/problem-sets/994805342720868352/problems/994805359372255232)

> 二叉树的静态写法 + 完全二叉树定义

```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int n;
const int maxn = 20 + 5;

struct Node{
    int l, r;
}arr[maxn];

// 记录最大的下标
int MAX = -1;
// 记录最后一个节点
int ans;

void dfs(int root, int idx){
    if ( idx > MAX){
        ans = root;
        MAX = idx;
    }
    // 按下标递归遍历
    if ( arr[root].l != -1) dfs(arr[root].l, 2*idx + 1);
    if ( arr[root].r != -1) dfs(arr[root].r, 2*idx + 2);
}

int main(){
    ios::sync_with_stdio(false);
    cin.tie(0);
    
    cin >> n;
    string l, r;
    vector<bool> notFather(n, false);
    for (int i = 0; i < n; i++) {
       cin >> l >> r;
       if ( l == "-"){
           arr[i].l = -1;
       }else{
           arr[i].l = stoi(l);
           notFather[stoi(l)] = true;
       }
       if ( r == "-"){
           arr[i].r = -1;
       }else{
           arr[i].r = stoi(r);
           notFather[stoi(r)] = true;
       }
    }
    // 找到根节点
    int root = 0;
    while( notFather[root] == true) root++;
    dfs(root, 0);
    
    if (MAX == n - 1) cout << "YES " << ans << endl;
    else cout << "NO " << root << endl;
    return 0;   
}
```





### [1113 Integer Set Partition](https://pintia.cn/problem-sets/994805342720868352/problems/994805357258326016)

> 大水题
>
> 将输入的数分成两个不相交的集合，在满足个数差最小的条件下，找两个集合所有元素和差最大值。
> 其实是道找规律题：将数进行从小到大排序，然后一分为二，根据奇偶数的不同，n之差直有1和0两种可能，差值最大的情况便是大数-小数

```c++
#include <bits/stdc++.h>
using namespace std;
const int SIZE = 1e5 + 5;
int arr[SIZE];

int main(int argc, char const *argv[]){
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i)
        cin >> arr[i];
    sort(arr, arr+n);
    
    int s1 = 0, s2 = 0 ;
    if( n % 2 == 1){
        for (int i = 0; i < (n-1)/2; ++i)
            s1 += arr[i];
        for (int i = (n-1)/2; i < n; ++i)
            s2 += arr[i];
        cout << 1 << " " << s2 -s1 << endl;
    }else{
        for (int i = 0; i < n/2; ++i)
            s1 += arr[i];
        for (int i = n/2; i < n; ++i)
            s2 += arr[i];
        cout << 0 << " " << s2 -s1 << endl;
    }
    return 0;
}

```

### [1116 Come on! Let's C](https://pintia.cn/problem-sets/994805342720868352/problems/994805355358306304)

```c++

#include <iostream>
#include <cmath>
#include <iomanip>
using namespace std;
const int num = 1e4 + 5;
int arr[num];

bool isPrime(int n){
    // if ( n < 0 ) return false;
    if (n == 1) return false;
    if ( n == 2 || n == 3) return true;
    if ( n % 6 != 5 && n % 6 != 1) return false;
    for (int i = 5; i <= sqrt(n); i+=6) {
    if ( n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
}

string coutList[5] = {
    "Are you kidding?",
    "Mystery Award",
    "Minion",
    "Chocolate",
    "Checked"
};



int main(){
    ios::sync_with_stdio(false);
    int n, k;
    cin >> n;
    int tmp;
    int q;
    for (int i = 1; i <= n; i++) {
        cin >> tmp;
        arr[tmp] = i;
        if (i == 1){
            arr[tmp] = 1;
        }else if( isPrime(i) ){
            arr[tmp] = 2;
        }else{
            arr[tmp] = 3;
        }
    }

    cin >> k;
    for (int j = 0; j < k; j++) {
        cin >> q;
        printf("%04d: %s\n", q,  coutList[arr[q]].c_str() );
        if (arr[q]){
            arr[q] = 4;
        }
    }
    return 0;
}
```



### [1117 Eddington Number](https://pintia.cn/problem-sets/994805342720868352/problems/994805354762715136)

```c++
/*
 * @Author: Mrli
 * @Date: 2020-09-05 09:27:31
 * @LastEditTime: 2020-09-05 09:36:35
 * @Description:    
 */
#include <bits/stdc++.h>
using namespace std;

int main(){

    int n;
    scanf("%d", &n);
    vector<int> v(n);
    for (int i = 0; i < n; i++) {
        scanf("%d", &v[i]);
    }
    sort(v.begin(), v.end(), less<int>());
    // 实质上就是双指针法， ans必须从1开始， 因为他是最长的天数
    // int ans = !v.empty() ? v[0] : 1;  错误写法， 最大距离可以是E， 但是天数不可能为
    int ans = 1;
    for (int i = n - 1; i >= 0 ; ans++, i -- ) {
       if ( ans >= v[i]) break;
    }
    cout << ans - 1 << endl;

    return 0;
}
```



### [1120 Friend Numbers](https://pintia.cn/problem-sets/994805342720868352/problems/994805352925609984)

> 我使用的是Map，但set效果更好

```c++

#include <bits/stdc++.h>
using namespace std;
const int SIZE = 1e5 + 5;
int arr[SIZE];



int main(int argc, char const *argv[]){
    int n;
    cin >> n;
    std::map<int, int> map;
    for (int i = 0; i < n; ++i)
        cin >> arr[i];

    for (int i = 0; i < n; ++i){
        int sum = 0;
        int tmp = arr[i];
        while(tmp>0){
            sum += tmp%10;
            tmp /= 10;
        }
        map[sum] ++;
    }


    std::vector<int> v;
    cout << map.size() << endl;
    for (std::map<int, int>::iterator i = map.begin(); i != map.end(); ++i)
    {
        // end()-1好像不行
        // std::map<int, int>::iterator e = map.end()-1;
        // if(i != e)
            // cout << i->first << ' ';
        // else
            // cout << i->first << endl;
        
        //第二种写法
        // std::map<int, int>::iterator e = map.end()-1;
        // if(i != map.begin())
            // cout << ' ';
        // else
            // cout << i->first << endl;
        
        // 第三种办法
        // 使用flag记录
        
        /*第四种
        if(it==out.begin())
            cout<<*it;
        else
            cout<<" "<<*it;
        */
        
        // 我的笨办法
        v.push_back(i->first);
    }
    for (int i = 0; i < v.size(); ++i)
    {
        if(i!=v.size()-1) cout << v[i] << " ";
        else cout << v[i] << endl;
    }
    return 0;
}
```



### [1128 N Queens Puzzle](https://pintia.cn/problem-sets/994805342720868352/problems/994805348915855360)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e3 + 5;
int n, m;
vector<int> chess;

bool judge(int i){
    // i为行数, 即放的第几个
    // chess[x] 记录的列数
    for (int j = 0; j < i; j++) {
        if ( ( chess[i]  == chess[j] ) || ( abs(chess[i] - chess[j]) == abs(i - j))) return false;
    }  
    return true;
}

int main(){
    int T;
    cin >> T;
    while(T--){
        cin >>n;
        chess.resize(n);
        bool yes = true;
        for (int i = 0; i < n; i++) {
            cin >> chess[i];
            if(judge(i) == false){
                yes = false;
                // break;
            }
        }   
        if (yes) cout << "YES" <<endl;
        else cout << "NO" << endl;
    }
    return 0;
}
```





### [1134 Vertex Cover](https://pintia.cn/problem-sets/994805342720868352/problems/994805346428633088)

```c++
/*
 * @Author: Mrli
 * @Date: 2020-06-25 13:35:38
 * @LastEditTime: 2020-06-25 14:42:20
 * @Description: 
 */ 
#include <bits/stdc++.h>
using namespace std;


/**
两种思路:
- 将边都收集后, 待遍历query点集时每遍历一个点, 就把其相连的边置为已访问, 当遍历完所有点, 再看有没有边时未访问的
- 将边都收集后, 遍历每一条边, 看边的两端是否在query的点集中
*/

// 方法一, 遍历点时将边标记为1, 最后再遍历看看有没有边没访问过
int main(){
    int n, m;
    scanf("%d%d", &n, &m);
    vector<int> vec[n]; // 链表
    vector<int> visited;
    visited.resize(m+1);
    
    for (int i = 1; i <= m; i++) {
        int u, v;
        scanf("%d%d", &u, &v);
        vec[u].push_back(i);
        vec[v].push_back(i);
    }

    int q;
    int x;
    scanf("%d", &x);
    while(x--){
        scanf("%d", &q);
        // 每次将访问结果清空
        fill(visited.begin(), visited.end(), 0);
        for (int j = 0; j < q; j++) {
            int u;
            scanf("%d", &u);
            for (int v = 0; v < vec[u].size(); v++) {
                if (!visited[ vec[u][v] ] ) visited[ vec[u][v] ] = 1;
            }
        }

        bool all = true;
        for (int i = 1; i <= m; i++) {
        if (visited[i] == 0){
            printf("No\n");
            all = false;
            break;
        }
        }
        if( all ) printf("Yes\n");
    }
    return 0;
}


// 方法二, 遍历边, 判断是否有边两端都不在query点集中.
struct node{
    int u;
    int v;
    node(){}
    node(int uu, int vv){
        u = uu;
        v = vv;
    }
};


int main(){
    int n, m;
    scanf("%d%d", &n, &m);
    // 需要有node(){} 参数为空的构造函数, 所以如果声明了带参数的构造函数, 那么需要特地写个空的构造函数
    vector<node> edge;
    edge.resize(m);
    for (int i = 0; i < m; i++) {
       int u, v;
       scanf("%d%d", &edge[i].u, &edge[i].v);
    }

    int x;
    cin >> x;
    while(x--){
        bool all = true;unordered
        int q;
        scanf("%d", &q);
        int tmp;
        unordered_set<int> se;
        for (int i = 0; i < q; i++) {
           scanf("%d", &tmp);
           se.insert(tmp);
        }


        for (int j = 0; j < m; j++) {
           if (se.find(edge[j].u) == se.end() && se.find(edge[j].v) == se.end() ){
               all =false;
               break;
           }
        }
        if (all) printf("Yes\n");
        else printf("No\n");
    }
    return 0;
}

```



### [1146 Topological Order](https://pintia.cn/problem-sets/994805342720868352/problems/994805343043829760)

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const int maxn = 1e4 + 5;

int n, m;

vector<int> ve[maxn];
vector<int> in;         // 入度


void bfs(){
    queue<int> q;
    for (int i = 0; i < n; i++) {
       if ( in[i] == 0){
           q.push(i);
       }
    }
}

int main(){

    cin >> n >> m;
    in.resize(n + 1);

    int u, v;    
    for (int i = 1; i <= m; i++) {
        cin >> u >> v;
        ve[u].push_back(v);
        in[v]++;
    }

    int k;
    vector<int> ans;
    cin >> k;
    bool first = true;
    for (int i = 0; i < k; i++) {
        vector<int> copyin(in.begin(), in.end());
       bool flag = true;
       int tmp;
       for (int j = 0; j < n; j++) {
            cin >> tmp;
            if ( copyin[tmp] ) flag = false;
            for(auto &k: ve[tmp]) copyin[k]--;
            
       }
       if (flag == false) ans.push_back(i);
    }

    int sz = ans.size();
    for (int i = 0; i < sz; i++) {
       if (i == sz - 1) cout << ans[i] << endl;
       else cout << ans[i] << " ";
    }

    return 0;
}
```





### [1154 Vertex Coloring](https://pintia.cn/problem-sets/994805342720868352/problems/1071785301894295552)

```c++
/*
 * @Author: Mrli
 * @Date: 2020-07-18 20:55:38
 * @LastEditTime: 2020-07-18 21:18:07
 * @Description: 
 */ 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;

int n, m;

struct edge{
    int uu,vv;
};


/** 每条边的两个顶点都是不同的颜色， 最多能有多少种颜色 */
int main(){
    cin >> n >> m;
    vector<edge> v(m);
    
    for (int i = 0; i < m; i++) {
        cin >> v[i].uu >> v[i].vv;
    }

    int k;
    cin >> k;
    for (int i = 0; i < k; i++) {
        // 记录顶点颜色
        vector<int> cv(n);
        // 记录颜色个数
        set<int> se;
        for (int j = 0; j < n; j++) {
            cin >> cv[j];
            se.insert(cv[j]);
        }
        
        bool flag = true;
        for (int j = 0; j < m; j++) {
            // cout << v[j].uu << "\t" << v[j].vv << "\t" << cv[ v[j].uu ] <<  "\t" << cv[ v[j].vv ] << endl;
           if ( cv[ v[j].uu ] == cv[ v[j].vv ]){
               flag = false;
               break;
           }
        }
        if (flag){
            cout << se.size() << "-coloring" << endl;
        }else{
            cout << "No" << endl;
        }

    }

    return 0;
}
```



### [1155 Heap Paths](https://pintia.cn/problem-sets/994805342720868352/problems/1071785408849047552)

```cpp
/*
 * @Author: Mrli
 * @Date: 2020-07-18 21:37:50
 * @LastEditTime: 2020-07-18 21:37:50
 * @Description: 
 */ 
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
int n;

vector<int> tree;
bool maxHeap = false;
bool minHeap = false;

void dfs(int root, vector<int> path){
    path.push_back(tree[root]);

    int left = root*2 + 1;
    int right = root*2 + 2;
    if (left >= n) {		// 以一个子节点为根据, 反向探索
        int sz = path.size();
        for (int i = 0; i < sz; i++) {
            cout << path[i];
           if ( i == sz - 1){
               cout << endl;
           }else{
               cout <<" ";
               if ( path[i] < path[i+1] && i+1 < n) minHeap = true;
                else if( path[i] > path[i+1] && i+1 < n) maxHeap = true;
           }
        }
    }else if (left == n-1){		// 判断左子树是否为最后一个节点->是否有右子树
            dfs(left, path);
    }else{
        dfs(right, path);
        dfs(left, path);
    }
}

int main(){
    cin >> n;
    tree.resize(n);
    for (int i = 0; i < n; i++) {
        cin >> tree[i];
    }
    vector<int> path;
    dfs(0, path);    
    
    if ( maxHeap && minHeap )cout <<"Not Heap" <<endl;
    else if (maxHeap) cout << "Max Heap" <<endl;
    else if (minHeap) cout << "Min Heap" <<endl;
    else cout <<"Not Heap" <<endl;

    return 0;
}

```



````c++
/*
* @Author: Mrli
* @Date: 2020-05-26 20:10:10
 * @LastEditTime: 2020-05-26 20:36:44
* @Description: 
*/ 

#include <bits/stdc++.h>
#include <algorithm>

using namespace std;

struct School{
    string name;
    double total;
    int testee;
    School(): name(""), testee(0), total(0.0){}
    bool operator< (const School &that) const{
        if ( abs(total - that.total) > 1e-8 )     // 不相等, 记得加abs绝对值
        // if ( total != that.total )     // 不相等
            return total > that.total;
        if ( testee != that.testee )
            return testee < that.testee;
        return name < that.name;
    }
};

int main(){
    int n;
    cin >> n;
    unordered_map<string, School> hash;
    while(n -- ){
        string sch, id;
        double score ;

        cin >> id >> score >> sch;
        for(auto &s: sch) s = tolower(s);
        
        if ( id[0] == 'B') score /= 1.5;
        else if (id[0]== 'T' ) score *= 1.5;


        hash[sch].testee ++;
        hash[sch].total += score;
        hash[sch].name = sch;
    }  


    vector<School> v;
    for(auto item: hash){
        item.second.total = int( item.second.total + 1e-8);
        v.push_back(item.second);
    }

    sort(v.begin(), v.end());
    cout << v.size() << endl;


    int rank = 1;
    for (int i = 0; i < v.size(); i++) {
        School s = v.at(i);
       if (i == 0) {
           cout << rank << " " << s.name << " " << s.total << " " << s.testee << endl;
       }else{
           if( s.total != v.at(i-1).total) rank = i+1;
           cout << rank << " " << s.name << " " << s.total << " " << s.testee << endl;
       }
    }

    return 0;
}

````



## 附录

[刷PAT好用到哭的函数](https://blog.csdn.net/zxc1364787928/article/details/85757131 )

**好用的函数**

string->int

```c++
// 法一
int grade;
string s = "123";
stringstream ss;
ss >> grade;

// 法二
stoi() // 在cstring中
```

int->string

```c++
// 法一
to_string()// C++11之后才支持
// 法二
itos()	   // cstring中
```





sort中cmp函数编写

```c++
// 从大到小排序
bool cmp(rc &a, rc &b){
    return a.grade > b.grade;
}
// 从小到大排序
bool cmp(rc &a, rc &b){
    return a.grade < b.grade;
}

// 如果为M则排前，F排后； 如果字符相同，就比成绩
// ->排前的return true
bool cmp(rc &r1, rc &r2){
    if(r1.sex == "M" && r2.sex == "F") return true;
    else if(r1.sex == "F" && r2.sex == "M") return false;
    else
        return r1.grade < r2.grade;
}


// 一般情况下,对相等的情况不需要特殊处理,因此</>也可以处理相等的情况,
// 但是一旦题目要求处理相等情况, 那么就需要额外拎出判断, 见1025 PAT Ranking
bool cmp(rc &a, rc &b){
    if(a.grade != b.grade)
        return a.grade>b.grade;
    else
        return (a.ID<b.ID);
}

```

