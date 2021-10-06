## Jam的计数法 （模拟）

```
/*一个字符串，要让字符升序，求它的后5个字符串。
if: 最后一个字符++没有超出限制，输出。
else: 跳到前一个字符++
    if: 没有超出限制（限制要改变：变成基础限制+当前位数{给后面的字符留下选择}）
    else: 跳到前一个字符。
*/
#include<bits/stdc++.h>
using namespace std;
char s[50];
int main() {
    int a, b, w, t = 0;
    cin >>a>>b>>w>>s;
    for(int i=1; i<=5; i++) {
        s[w-1]++;
        for(int j=w-1; ; j--) {
            if(s[j] > 96+b-t) {
                if(j == 0) return 0;
                s[j-1]++;
            }
            else {
                for(int k=j+1; k<w; k++) {
                    s[k] = s[k-1]+1;
                }
                break;
            }
            t++;
        }
        cout << s << endl;
        t = 0;
    }
    return 0;
}
```





传球游戏 （记忆化搜索）

$f[i] [j]$ 表示在第j步传给第i个人的方案数。

```cpp
#include<bits/stdc++.h>
using namespace std;
int f[35][35], n, m;
void dfs(int s, int c) {
    f[s][c] = 0;
    if(c == m && s == 1) f[s][c] = 1;
    if(c != m) {
        if(f[s%n+1][c+1] == -1) dfs(s%n+1, c+1);
        if(f[(s==1?n:s-1)][c+1] == -1) dfs((s==1?n:s-1), c+1);
        f[s][c] = f[s%n+1][c+1] + f[(s==1?n:s-1)][c+1];
    }
}
int main() {
    memset(f, -1, sizeof f);
    cin >> n >> m;
    dfs(1, 0);
    cout << f[1][0];
    return 0;
}
```



$dp[i][j]$ 前i种花用j盆的方案数

记忆化搜索，动态规划
记忆化搜索一定能转化成动态规划，
反之不能
https://www.luogu.com.cn/problem/P1077
https://www.luogu.com.cn/blog/76228/ti-xie-p1077-bai-hua-post


```cpp
#include<bits/stdc++.h>
using namespace std;
int dp[110][110], n, m;
int a[110];
const int mod = 1000007;
int dfs(int x, int y) {
    int ans = 0;
    if(y > m) return 0;
    if(y == m) return 1;
    if(x > n) return 0;
    if(dp[x][y]) return dp[x][y];
    for(int i=0; i<=a[x]; i++) ans = (ans + dfs(x+1, y+i)) % mod;
    dp[x][y] = ans % mod;
    return ans % mod;
}
int main() {
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> a[i];
    cout << dfs(1, 0);
    return 0;
}
```



## dp

$dp[j]$ 表示以j结尾的最长上升子序列

求最长a1<a2<a3..<ai>ai+1>...ak

https://www.luogu.com.cn/problem/P1091
最长上升子序列（参考导弹拦截）
dp思路，以某个数字结尾的最长上升子序列。

  ```
#include<bits/stdc++.h>
using namespace std;
int s[110], f1[110], f2[110];
int main() {
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) cin >> s[i];
    for(int i=n; i>=1; i--) {
        f1[i] = 1;
        for(int j=i+1; j<=n; j++) {
            if(s[i] > s[j]) f1[i] = max(f1[i], f1[j]+1);
        }
    }
    for(int i=1; i<=n; i++) {
        f2[i] = 1;
        for(int j=i-1; j>=1; j--) {
            if(s[i] > s[j]) f2[i] = max(f2[i], f2[j]+1);
        }
    }
    int ans = 0;
    for(int i=1; i<=n; i++) {
        ans = max(ans, f1[i]+f2[i]-1);
    }
    cout << n-ans;
    return 0;
}
  ```





## 简单线性dp（没写出来）
https://www.luogu.com.cn/problem/P1103
https://www.luogu.com.cn/problem/solution/P1103

  ```
#include<bits/stdc++.h>
using namespace std;
struct Y {
    int x, y;
} s[110];
bool cmp(Y a, Y b) {
    return a.x < b. x;
}
int dp[110][110];//j, k, 以j结束，选k个
int main() {
    memset(dp, 0x3f, sizeof dp);
    int n, m;
    cin >> n >> m;
    m = n-m;
    for(int i=1; i<=n; i++) cin >> s[i].x >> s[i].y;
    for(int i=1; i<=n; i++) dp[i][1] = 0;
    sort(s+1, s+1+n, cmp);
    for(int i=2; i<=n; i++) {
        for(int j=1; j<i; j++) {
            for(int k=2; k<=min(i, m); k++) {
                dp[i][k] = min(dp[j][k-1]+abs(s[i].y-s[j].y), dp[i][k]);
            }
        }
    }
    int ans = 0x7ffffff;
    for(int i=m; i<=n; i++) {
        ans = min(ans, dp[i][m]);
    }
    cout << ans;
    return 0;
}
  ```



## P1134 [USACO3.2]阶乘问题

https://www.luogu.com.cn/problem/P1134

去掉可以变成10的2，5，剩下的就直接可以乘，再取余。

```
#include<bits/stdc++.h>
using namespace std;
int solve(int x) {
    int ans = 1, t, u = 0, v = 0;
    for(int i=2; i<=x; i++) {
        t = i;
        if(t % 2 == 0) {
            while(t % 2 == 0) t/=2, u++;
        }
        if(t % 5 == 0) {
            while(t % 5 == 0) t/=5, v++;
        }
        ans *= t;
        ans %= 10;
    }
    for(int i=1; i<=u-v; i++) {
        ans = (ans * 2) % 10;
    }
    return ans;
}
int main() {
    int n;
    cin >> n;
    cout << solve(n);
    return 0;
}
```



## P1158 导弹拦截

排序，贪心，枚举。

https://www.luogu.com.cn/problem/P1158#submit

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5+10;
ll dis(ll x, ll y, ll xx, ll yy) {
    return (x-xx)*(x-xx)+(y-yy)*(y-yy);
}
struct Y {
    ll d1, d2, i;
} D[N];
bool cmp(Y a, Y b) {
    return a.d1 > b.d1;
}
int x[N], y[N];
int main() {
    int xx, yy, xxx, yyy;
    cin >> xx >> yy >> xxx >> yyy;
    ll n, m, ans = 0x7ffffff;
    cin >> n;
    for(int i=1;i<=n; i++) {
        cin >> x[i] >> y[i];
        D[i].d1 = dis(xx, yy, x[i], y[i]);
        D[i].i  = i;
    }
    sort(D+1, D+n+1, cmp);
    for(int i=1; i<=n; i++) {
        m = dis(x[D[i].i], y[D[i].i], xxx, yyy);
        D[i].d2 = max(m, D[i-1].d2);
    }
    for(int i=1; i<=n; i++) {
        // cout << D[i].d1 << ' ' << D[i].d2 << endl;
        ans = min(ans, D[i].d1+D[i-1].d2);
    }
    cout << ans;
    return 0;
}
```





```
#include<bits/stdc++.h>
using namespace std;
int D[10], D1[10], s[5010];
int day[]={0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};

bool change(int x) {
    if((D[1] % 4 == 0 && D[1] % 100 != 0) || D[1] % 400 == 0) day[2]++;
    D[5] += x;
    int t;
    if(D[5] >= 60) t = D[5]/60, D[5] = D[5] - t*60;
    D[4] += t;
    t = 0;
    if(D[4] >= 24) t = D[4]/24, D[4] = D[4] - t*24;
    D[3] += t;
    t = 0;
    while(D[3] > day[D[2]] && D[2] <= 12) D[3] -= day[D[2]], D[2]++;
    if(D[2] > 12) t = D[2]/12, D[2] -= t*12;
    D[1] += t;
    t = 0;
    // for(int i=1; i<=5; i++) cout << D[i] << ' ';
    day[2] = 28;
    for(int i=1; i<=5; i++) {
        if(D[i] > D1[i]) return false;
    }
    return true;
}
int main() {
    int n, ans = 0;
    scanf("%d", &n);
    for(int i=1; i<=n; i++) scanf("%d", &s[i]);
    sort(s+1, s+1+n);
    scanf("%4d-%2d-%2d-%2d:%2d", &D[1], &D[2], &D[3], &D[4], &D[5]);
    scanf("%4d-%2d-%2d-%2d:%2d", &D1[1], &D1[2], &D1[3], &D1[4], &D1[5]);
    // for(int i=1; i<=5; i++) cout << D[i] << ' ';
    for(int i=1; i<=n; i++) {
        if(change(s[i])) ans++;
        else break;
    }
    cout << ans;
    return 0;
}
```



## 二维单调递增的最长序列(二维偏序)

给出x, y。求二维单调递增的最长序列(可交换顺序)

思路：二维平面上的面积覆盖。

按其中一个属性来排序，再求最长上升子序列

https://www.luogu.com.cn/problem/P1233





```
#include<bits/stdc++.h>
using namespace std;
struct Y{
    int x, y;
} s[5010];
bool cmp(Y a, Y b) {
    return a.x > b.x;
}
int f[5010];
int main() {
    int n, ans = 0;
    cin >> n;
    for(int i=1; i<=n; i++) {
        cin >> s[i].x >> s[i].y;
    }
    sort(s+1, s+1+n, cmp);
    for(int i=1; i<=n; i++) {
        // cout << s[i].x << ' ' << s[i].y << endl;
        f[i] = 1;
        for(int j=i-1; j>=1; j--) {
            if(s[i].y > s[j].y) f[i] = max(f[i], f[j]+1);
        }
        ans = max(ans, f[i]);
    }
    cout << ans;
    return 0;
}
```



## 导弹拦截(最长上升子序列，最长不上升子序列)

解法一：树状数组

导弹的高度小于50000，我们可以枚举第i个导弹，看它之前(高度)的最长的不上升子序列的长度，再把这个长度+1放进它的高度这个位置。

每次查询一个高度都是查询 在它的高度之前 且 输入在序列位置 在它前面的的最大长度。

每次加入该位置的最大长度 都会向后面的位置传递它的最大长度，后面位置再做一次比较，取最大值。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 100010;
int s[N], tot = 1, f[N], mx = 0;
#define lowbit(x) (x&(-x))
int query(int x) {
    int res = 0;
    while(x) {
        res = max(res, f[x]);
        x -= lowbit(x);
    } 
    return res;
}

void add(int x, int y) {
    while(x<=mx) {
        f[x] = max(f[x], y);
        x += lowbit(x);
    }
}
int main() {
    //freopen("in.txt", "r", stdin);
    int ans = 0, q;
    while(cin >> s[tot]) mx = max(s[tot], mx), tot++;
    tot--;
    for(int i=tot; i>=1; i--) {
        q = query(s[i]) + 1;
        add(s[i], q);
        ans = max(ans, q);
    } 
    cout << ans << endl;
    ans = 0;
    memset(f, 0, sizeof f);
    for(int i=1; i<=tot; i++) {
        q = query(s[i]-1) + 1;
        add(s[i], q);
        ans = max(ans, q);
    } 
    cout << ans;
    return 0;
}
```





## 单调队列

（单调上升）

每次都用队列的最后一个元素b来和当前元素a比较。

如果a>b，就直接可以把a加到队列的末尾。

如果b>a，就看a可以放在队列的哪个位置（在队列中第一个大于等于a的位置）；

​	注：如果这个位置不是队列的最后一个的话，是不会影响结果的()。但如果是最后一个的话，队列的最大值就减小了（最优）。

```
#include<bits/stdc++.h>
using namespace std;
const int N = 100010;
int s[N], tot=1;
int c[N], d[N];
int main() {
    // freopen("in.txt", "r", stdin);
    int ans = 0;
    while(cin >> s[tot]) tot++;
    tot--;
    
    int t = 1, u = 1;
    c[1] = s[1], d[1] = s[1];
    for(int i=2; i<=tot; i++) {
        if(s[i] <= d[u]) d[++u] = s[i];
        else {
            int p = upper_bound(d+1, d+1+u, s[i], greater<int>()) - d;
            d[p] = s[i];
        }
        if(s[i] > c[t]) c[++t] = s[i];
        else {
            int p = lower_bound(c+1, c+t+1, s[i])-c;
            c[p] = s[i];
        } 
    }
    cout << u << endl << t;
    return 0;
}
```



## P1095 守望者的逃离

https://www.luogu.com.cn/problem/P1095

题意：M法力值，S的距离，T的时间。花费1秒走17m，花费1秒和10法力值的闪现能走60m，花费1秒可以恢复4点法力值。

对于每一秒，我们能够产生距离的是走和闪现，

```
#include<bits/stdc++.h>
using namespace std;
int main() {
    int m, s, t;
    cin >> m >> s >> t;
    int d1 = 0, d2 = 0;//分成两部分，走路·闪现。
    for(int i=1; i<=t; i++) {
        d1 += 17;//走路方案，
        if(m >= 10) m-=10, d2 += 60;//闪现方案。
        else m += 4;//蓝不够的情况
        if(d2 > d1) d1 = d2;//如果d1取两个的最大值。
        if(d1 > s) {//看是否到达终点
            cout << "Yes\n" << i;
            return 0;
        }
    }
    cout << "No\n" << d1;
    return 0;
}
```





## 乌龟棋

https://www.luogu.com.cn/problem/P1541

题意：一行，n个格子，每个格子一个价值。有4种属性的牌，给出m张牌，走1步，走2步，走3步，走4步。

从第1个位置开始，用完m张牌一定可以到第n个位置。问如何安排牌的顺序，使路径上的价值和最大。



$dp[i][j][k][l]，i,j,k,l，分别表示4种牌的使用个数$   

```
int t = s[i+2*j+3*k+4*l+1];
if(i > 0) dp[i][j][k][l] = max(dp[i-1][j][k][l]+t, dp[i][j][k][l]);
if(j > 0) dp[i][j][k][l] = max(dp[i][j-1][k][l]+t, dp[i][j][k][l]);
if(k > 0) dp[i][j][k][l] = max(dp[i][j][k-1][l]+t, dp[i][j][k][l]);
if(l > 0) dp[i][j][k][l] = max(dp[i][j][k][l-1]+t, dp[i][j][k][l]);
```

一个i，j，k，l，可以确定一个位置，我么枚举这个位置可能的前一个位置，取最大值。

```
#include<bits/stdc++.h>
using namespace std;
int s[400], w[5], dp[45][45][45][45];
int main() {
    int n, m, a;
    cin >> n >> m;
    for(int i=1; i<=n; i++) cin >> s[i];
    for(int i=1; i<=m; i++) {cin >> a, w[a]++;}
    dp[0][0][0][0] = s[1];
    for(int i=0; i<=w[1]; i++) {
        for(int j=0; j<=w[2]; j++) {
            for(int k=0; k<=w[3]; k++) {
                for(int l=0; l<=w[4]; l++) {
                    int t = s[i+2*j+3*k+4*l+1];
                    if(i > 0) dp[i][j][k][l] = max(dp[i-1][j][k][l]+t, dp[i][j][k][l]);
                    if(j > 0) dp[i][j][k][l] = max(dp[i][j-1][k][l]+t, dp[i][j][k][l]);
                    if(k > 0) dp[i][j][k][l] = max(dp[i][j][k-1][l]+t, dp[i][j][k][l]);
                    if(l > 0) dp[i][j][k][l] = max(dp[i][j][k][l-1]+t, dp[i][j][k][l]);
                }
            }
        }
    }
    cout << dp[w[1]][w[2]][w[3]][w[4]];
    return 0;
}

```



## 饥饿的奶牛

https://www.luogu.com.cn/problem/P1868

给出n个区间，问最长的不重叠的区间序列长度。

思路：先按起点排序，枚举每个位置。按转移规则取dp值。



```
#include<bits/stdc++.h>
using namespace std;
const int N = 5e5+10; 
const int M = 5e6+10;
struct Y{
    int x, y;
} s[M];
bool cmp(Y a, Y b) {
    if(a.x == b.x) return a.y < b.y;
    return a.x < b.x;
}
int dp[M]; 
int main() {
    int n, m = 0, ans = 0;
    cin >> n;
    for(int i=1; i<=n; i++) {
        cin >>s[i].x >> s[i].y;
        m = max(s[i].y, m);
    }
    sort(s+1, s+n+1, cmp);
    int j = 1;
    for(int i=0; i<=m; i++) {
    	//1
        dp[i] = max(dp[i-1], dp[i]);//不论i是否是给定区间里的位置，我们都要取前面所有位置的最大值。
        while(s[j].x == i && j <= n) {//一定是while(可能存在相同的起点)
        	//转移方程，如果有相同起点的区间，就先把终点小的区间最大值给求出(sort).
        	//每次遇到区间，只会更新区间终点的最大值，所有这样避免重复区间的出现（点的修改，通过1传递）
            dp[s[j].y] = max(dp[s[j].y], dp[s[j].x-1]+s[j].y-s[j].x+1);
            j++;
        }
        ans = max(ans, dp[i]);
    }
    cout << ans;
    return 0;
}
```



## 欧拉

$\phi(\phi(\phi(N))) = \phi^3(N)$, 问使$\phi^x(N)=1$,的最小x。

输入给出$p_i和q_i$ 

提示 $$\varphi(\prod_{i = 1}^m p_i^{q_i}) = \prod_{i = 1}^m (p_i - 1)*p_i^{q_i-1}$$ 

https://www.luogu.com.cn/problem/P2350

其中f数组，dp思想（😔）

（**注：此处【分出的2的个数】指一个数减一后质因数分解，再将其中大于2的数重复此步骤最后剩下2的个数**）

质因子最大不超过十万，因此可以DP预处理出每个质因子一共会分出多少个2：（设i分出2的个数为f[i]）

①f[p]=f[p-1]*f*[*p*]=*f*[*p*−1] (p为质数)

p无法质因数分解，直接减一

②f[a*b]=f[a]+f[b]*f*[*a*∗*b*]=*f*[*a*]+*f*[*b*]

仍没什么好解释的，假想a*b中先分a再分b就行了



```

#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5+10;
int st[N], p[N], f[N], tot;
void init() {
    f[1] = 1;
    for(int i=2; i<N; i++) {
        if(!st[i]) p[tot++] = i, f[i] = f[i-1];
        for(int j=0; j<tot&&i*p[j]<N; j++) {
            st[i*p[j]] = 1, f[i*p[j]] = f[i]+f[p[j]];
            if(i % p[j] == 0) break;
        }
    }
}
int main() {
    init();
    int t;
    cin >> t;
    while(t--) {
        int n;
        ll p, q, ans = 1;
        cin >> n;
        for(int i=1; i<=n; i++) {
            cin >> p >> q;
            if(p == 2) ans--;
            ans += f[p]*q;
        }
        cout << ans << endl;
    }
    return 0;
}
```





```
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5+10;
priority_queue<int> q;
int a[N], b[N], ans[N];
int main() {
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) cin >> a[i];
    for(int i=1; i<=n; i++) cin >> b[i];
    sort(a+1, a+1+n);
    sort(b+1, b+1+n);
    for(int i=1; i<=n; i++) {
        for(int j=1; j<=n; j++) {
            int x = a[i] + b[j];
            if(q.size() < n) q.push(x);
            else {
                if(x < q.top()) {
                    q.pop();
                    q.push(x);
                }
                else break;
            }
        }
    }
    for(int i=n; i>=1; i--) {
        ans[i] = q.top();
        q.pop();
    }
    for(int i=1; i<=n; i++) {
        cout << ans[i] << ' ';
    }
    return 0;
}

```





## 中位数

https://www.luogu.com.cn/problem/P1168

https://www.luogu.com.cn/blog/SeanMoe/solution-p1168（大根堆，小根堆）

```
//vector,二分找位置，直接插入
#include<bits/stdc++.h>
using namespace std;
vector<int> v;
int main() {
	int n, x;
    cin >> n;
    for(int i=1; i<=n; i++) {
        cin >> x;
        v.insert(upper_bound(v.begin(), v.end(), x), x);
        if(i % 2 == 1) cout << v[(1+i)/2-1] << endl;
    }
	return 0;
}
```



## 高精模板

https://www.luogu.com.cn/problem/P1932



## 数字对

思路：辗转相除法拓展

 https://www.luogu.com.cn/problem/P2090

https://www.luogu.com.cn/blog/user28939/solution-p2090

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll N = 1e14;
ll solve(ll x, ll y) {
    if(y == 1) return x-1;
    if(!y) return N;
    return x/y + solve(y, x%y);
}
int main() {
    ll n, ans = N;
    cin >> n;
    for(int i=1; i<n; i++) {
        ans = min(ans, solve(n, i));
    }
    cout << ans << endl;
    return 0;
}
```



## $P1016 旅行家的预算$



https://www.luogu.com.cn/problem/P1016

```
#include<bits/stdc++.h>
using namespace std;
#define Dou double
struct Y{//结构体，x:每升的价格，y:油量
   Dou x, y;
   Y (Dou _x, Dou _y) : x(_x), y(_y) {}
} ;
deque<Y> q;//双端队列

Dou d[10], p[10], ans, nc;//nc表示当前汽油量
int n;

int main() {
   Dou d1, c, d2;
   cin >> d1 >> c >> d2 >> p[0] >> n;

   //读入每个站点的距离和该站点汽油的价格
   for(int i =1; i<=n; i++) {
      cin >> d[i] >> p[i];
      
      //判断是否加满也不能走完这段路程
      if(d[i] - d[i-1] > c*d2) {
         cout << "No Solution\n";
         return 0;
      }
   }
   d[n+1] = d1, nc = c;
   q.push_back(Y(p[0], c));
   ans += p[0]*c;
   for(int i=1; i<=n+1; i++) {
      Dou nd = (d[i]-d[i-1])/d2;//nd需要消耗的汽油量

      /*思路*/
      /*队列中的元素表示：前面经过的加油站中，油的价格和剩余油的量（而且是递增的）*/
      /*这样一来：我们直接枚举到队列为空，或者消耗nd的汽油就跳出循环*/

      while(!q.empty() && nd > 0) {
         Y u = q.front(); q.pop_front();//选最前面的一个站点
         if(u.y > nd) {//如果该站点的汽油足够
            nc -= nd;
            q.push_front(Y(u.x, u.y-nd));//把剩余汽油又放进去
            break;
         }
         //不够的话就全部用完
         nc -= u.y, nd -= u.y;
      }

      //到达终点
      if(i == n+1) {
         //从前往后依次遍历求出多算的钱
         while(!q.empty()) {
            ans -= q.front().x*q.front().y;
            q.pop_front();
         }
         break;
      }
      /*单调队列思想*/
      /*到达这一步的时候说明：已经到达了第i个站点，此时就要重新考虑队列里面要留下的汽油（贪心）*/
      while(!q.empty() && q.back().x > p[i]) {
         //如果p[i]更小就减掉前面剩下的汽油的钱
         ans -= q.back().x * q.back().y;
         //nc减掉前面剩下的油的量
         nc -= q.back().y;
         q.pop_back();
      }
      //把油箱加满，把钱加上。
      ans += (c-nc)*p[i];
      //把这个站点放到队列里面去。
      q.push_back(Y(p[i], c-nc));
      //nc=c表示: 加满了
      nc = c;
   }
   printf("%.2lf\n", ans);
   return 0;
}
```



## 营救

https://www.luogu.com.cn/problem/P1396

```
#include<bits/stdc++.h>
using namespace std;
const int N = 2e4+10;
const int INF = 0x7ffffff;
struct E {
   int to, next, we;
} edge[N];

struct P{
   int w, num;
   inline P(int _x, int _y) : w(_x), num(_y) {}
   inline bool operator < (P const x) const {
      return w > x.w;
   }
};

int head[N>>1], tot = 1, dis[N>>1], vis[N>>1];
int n, m, s, t;
void add(int x, int y, int val) {
   edge[tot].to = y, edge[tot].next = head[x], edge[tot].we = val;
   head[x] = tot++;
}

queue<P> q;

void dijkstra() {
   for(int i=1; i<=n; i++) dis[i] = INF;
   dis[s] = 0;
   q.push(P(0, s));
   while(!q.empty()) {
      P temp = q.front(); q.pop();
      if(vis[temp.num]) continue;
      vis[temp.num] = 1;
      for(int i=head[temp.num]; i; i=edge[i].next) {
         int x = edge[i].to, y = edge[i].we;
         if(dis[x] > dis[temp.num]+y) {
			dis[x] = dis[temp.num]+y;
			q.push(P(x, dis[x]));
		 }
      }
   }
}

int main() {
   int x, y, val;
   cin >> n >> m >> s >> t;
   for(int i=1; i<=m; i++) {
      cin >> x >> y >> val;
      add(x, y, val);
      add(y, x, val);
   }
   dijkstra();
   for(int i=1; i<=n; i++) {
      cout << dis[i] << ' ';
   }
   return 0;
}



#include<bits/stdc++.h>
using namespace std;
const int N = 2e4+10;
struct Y {
   int x, y, val;
   Y (int _x=0, int _y=0, int _val=0):x(_x), y(_y), val(_val){}
   inline bool operator < (const Y & x) const {
      return val < x.val;
   }
} u[N];


int fa[N>>1];

int finds(int x) {
   if(fa[x] == x) return x;
   else return fa[x] = finds(fa[x]);
}
int main() {
   int n, m, s, t, x, y, val;
   cin >>n >> m >> s >> t;
   for(int i=0; i<m; i++) {
      cin >> x >> y >> val;
      u[i] = Y(x, y, val);
   }
   sort(u, u+m);
   for(int i=1; i<=n; i++) fa[i] = i;
   for(int i=0; i<m; i++) {
      int h = finds(u[i].x), g = finds(u[i].y);
      
      if(h != g){
         fa[h] = g;
      }
      if(finds(s) == finds(t)) {
         cout << u[i].val;
         break;
      }
   }
   return 0;
}
```



## [P3810 【模板】三维偏序（陌上花开）](https://www.luogu.com.cn/problem/P3810)

cdq分治：

code

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e5+10;
int cnt[N];
struct Y{
	int x, y, z, w, ans;
} A[N], B[N];

//比较函数1，按x的大小进行排序,相同则...
bool cmp1(Y a, Y b) {
	if(a.x == b.x) {
		if(a.y == b.y) return a.z < b.z;
		else return a.y < b.y;
	}
	else return a.x < b.x;
}

//比较函数2，按y的大小进行排序，相同则...
bool cmp2(Y a, Y b) {
	if(a.y == b.y) return a.z < b.z;
	else return a.y < b.y;
}

//树状数组
struct tree{
	int tr[N<<2], k;
	int lowbit(int x) {return (x&(-x));}
	void add(int x, int u) {
		while(x <= k) {
			tr[x] += u;
			x += lowbit(x);
		}
	}
	int ask(int x) {
		int ans = 0;
		while(x) {
			ans += tr[x];
			x  -= lowbit(x);
		}
		return ans;
	}
} t;

//cdq函数。
void cdq(int l, int r) {
	if(l == r) return ;
	int mid = (l+r)>>1;
	cdq(l, mid); cdq(mid+1, r);//递归
	sort(A+l, A+mid+1, cmp2); sort(A+mid+1, A+r+1, cmp2);//按y进行排序。
	
	//核心
	int i = mid+1, j = l;
	while(i <= r) {
		while(A[j].y <= A[i].y && j <= mid) t.add(A[j].z, A[j].w), j++;
		A[i].ans += t.ask(A[i].z);
		i++;
	}
	for(int i=l; i<j; i++) t.add(A[i].z, -A[i].w);
}

int main() {
	//初始化
	int n, k, u = 0;
	cin >> n >> k;
	t.k = k+10;
	for(int i=1; i<=n; i++) cin >> B[i].x >> B[i].y >> B[i].z;
	sort(B+1, B+1+n, cmp1);
	
	//去重
	int c = 0;
	for(int i=1; i<=n; i++) {
		c++;
		if(B[i].x!=B[i+1].x||B[i].y!=B[i+1].y||B[i].z!=B[i+1].z) 
		A[++u] = B[i], A[u].w = c, c = 0;
	}

	//分治
	cdq(1, u);

	//cnt存结果
	for(int i=1; i<=u; i++) cnt[A[i].ans+A[i].w-1] += A[i].w; 
	
	//输出答案
	for(int i=0; i<n; i++) cout << cnt[i] << endl;
	return 0;
}
```







## [P1145 约瑟夫](https://www.luogu.com.cn/problem/P1145)

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e2+10;
const double eps = 1e-5;
const int inf = 0x7ffffff;
int ans, k, k2;
bool a[N];
bool check(int x) {
    int u = 0;
    memset(a, false, sizeof a);
    for(int j=0; j<k; j++) {
        int n = x%(k2-j);
        if(k2-j <= x) n += k2-j;
        for(int i=1; i<=n; i++) {
            u++;
            while(a[u] == true) u++;
            if(u > k2) u = 1;
        }
        a[u] = true;
        if(u <= k) return false;
    }
    return ans = x;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    cin >> k;
    k2 = k*2;
    for(int m=k; !check(m); m++) ;
    cout << ans;
    return 0;
}

```





## [P1164 小A点菜](https://www.luogu.com.cn/problem/P1164)[简单dp]

求方案数。

$dp[i][j]$表示前i种物品全部花完j元钱的方案数。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1010;
int dp[N][N];
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, m, x;
    cin >> n >> m;
    for(int i=1; i<=n; i++) {
        cin >> x;
        for(int j=1; j<=m; j++) {
            if(j > x) dp[i][j] = dp[i-1][j] + dp[i-1][j-x];
            else if(j == x) dp[i][j] = dp[i-1][j] + 1;
            else dp[i][j] = dp[i-1][j];
        }
    }
    cout << dp[n][m];
    return 0;
}
```





## [P1002 [NOIP2002 普及组] 过河卒](https://www.luogu.com.cn/problem/P1002)[dp]

注意：初始化的时候，第一行如果中间有不能被访问的点，后面的点都不能被访问到。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
const int N = 40;
int vis[N][N];
ll ans[N][N], g, h;
int ex, ey, f[][2] = {1, 2, 2, 1, -1, 2, 2, -1, -2, 1, 1, -2, -1, -2, -2, -1};
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    cin >> g >> h >> ex >> ey;
    vis[ex][ey] = 1;
    for(int i=0; i<8; i++) {
        int u = ex+f[i][0],  v = ey + f[i][1];
        if(u >= 0 && v >= 0) vis[u][v] = 1;
    }
    for(int i=0; i<=g&&!vis[i][0]; i++) ans[i][0] = 1;
    for(int j=0; j<=h&&!vis[0][j]; j++) ans[0][j] = 1;
    for(int i=1; i<=g; i++) {
        for(int j=1; j<=h; j++) {
            if(!vis[i][j]) {
                ans[i][j] = ans[i-1][j] + ans[i][j-1];
            }
            else ans[i][j] = 0;
        }
    }
    cout << ans[g][h];
    return 0;
}
```





## [P3951 [NOIP2017 提高组] 小凯的疑惑 / [蓝桥杯 2013 省] 买不到的数目](https://www.luogu.com.cn/problem/P3951)

题意：有无限个两种互素的硬币，问最大的不能组成的商品的价值。

结论：a*b-a-b；







## [P1113 杂务(dp或拓扑排序)](https://www.luogu.com.cn/problem/P1113)

dp

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
typedef double dou;
const double eps = 1e-4;
const int N = 1e5+10;
int com[N], ans = 0;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
	int n, f, x, u, tmp = 0;
	cin >> n;
	for(int i=1; i<=n; i++) {
		cin >> f >> x >> u;
		tmp = 0;
		while(u != 0) {
			tmp = max(tmp, com[u]);
			cin >> u;
		}		
		com[f] = tmp + x;
		ans = max(com[f], ans);
	}
	cout << ans << endl;
	return 0;
}
```

拓扑排序

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e4+10;
int consum[N], cnt[N], vis[N], dp[N];
vector<int> v[N];
queue<int> q;
void bfs() {
    while(!q.empty()) {
        int x = q.front(); q.pop();
        for(int i=0; i<v[x].size(); i++) {
            int y = v[x][i];
            dp[y] = max(dp[y], dp[x]+consum[y]);
            cnt[y]--;
            if(!cnt[y]) q.push(y);
        }
    }
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) {
        int x, y, z;
        cin >> x >> y;
        consum[x] = y;
        while(cin >> z && z) {
            cnt[x]++;
            v[z].push_back(x);
        }
        if(!cnt[x]) {
            q.push(x);
            dp[x] = consum[x];
        }
    }
    bfs();
    int mx = 0;
    for(int i=1; i<=n; i++) mx = max(dp[i], mx);
    cout << mx << endl;
    return 0;
}
```





## [P1807 最长路](https://www.luogu.com.cn/problem/P1807)

将权值取负，再求最短路。

注意有重边

最短路写法

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef double dou;
typedef pair<int, int> pii;
const double eps = 1e-4;
const int N = 1e3+510;
const int M = 5e4+10;
ll head[N], tot=1, f[N], vis[N];

struct E{
	ll to, next, w;
} e[M];

struct Y {
	ll first, second;
};
struct cmp {//队头到队尾从小到大
    bool operator () (Y &u1, Y &u2) {
        return u1.first > u2.first;
    }
};
priority_queue<Y, vector<Y>,  cmp> q;
void D() {
	q.push({0, 1});
	f[1] = 0;
	while(!q.empty()) {
		Y p = q.top();
		q.pop();
		//有重边时不能用这个。
		//if(vis[p.second]) continue;
		//vis[p.second] = 1;
		for(int i=head[p.second]; i; i=e[i].next) {
			int x = e[i].to, y = e[i].w;
			if(f[x] > f[p.second]+y) {
				f[x] = f[p.second] + y;
				q.push({f[x], x});
			}
		}
	}
}
void add(int x, int y, int val) {
	e[tot].to = y, e[tot].w = val, e[tot].next = head[x];
	head[x] = tot++;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
	ll n, m, u, v, w;
	cin >> n >> m;
	for(int i=1; i<=m; i++) {
		cin >> u >> v >> w;
		add(u, v, -w);
	}
	for(int i=1; i<=n; i++) f[i] = 0x7ffffff;
	D();
	if(f[n] == 0x7ffffff) cout << -1 << endl;
	else cout << -f[n] << endl;
	return 0;
}
```



拓扑排序写法

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef pair<int, int> pii;
const int N = 1510;
const int M = 5e4+10;
int dp[N], cnt[N], f = 0;
vector<pii> e[N];
//拓扑+dp
void bfs(int n) {
    queue<int> q;
    q.push(1);
    while(!q.empty()) {
        int x = q.front(); q.pop();
        if(x == n) f = 1;
        for(int i=0; i<e[x].size(); i++) {
            int y = e[x][i].first, z = e[x][i].second;
            cnt[y]--;
            dp[y] = max(dp[y], dp[x]+z);
            if(cnt[y] == 0) q.push(y);
        }
    }
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, m, x, y, z;
    cin >> n >> m;
    for(int i=1; i<=m; i++) {
        cin >> x >> y >> z;
        e[x].push_back({y, z});
        cnt[y]++;
    }
    //初始化，作用：将与入度为0的点（1除外）相连的点得入度都减一。
    for(int i=1; i<=n; i++) {
        // dp[i] = -1;
        if(!cnt[i] && i != 1) {
            for(int j=0; j<e[i].size(); j++) {
                x = e[i][j].first;
                cnt[x]--;
            }
        }
    }
    // dp[1] = 0;
    bfs(n);
    // for(int i=1; i<=n; i++) cout << f[i] << endl;
    if(f != 0) cout << dp[n];
    else cout << -1 << endl;
    return 0;
}
```



## [P1367 蚂蚁](https://www.luogu.com.cn/problem/P1367)



```cpp
#include<bits/stdc++.h>
using namespace std;

typedef unsigned long long ll;
const int N = 1e5+10;
const int maxn = 2005;
const int INF = 0x3f3f3f3f;
const ll mod = 1e9+7;
struct Y{
	int x, y, z;
} s[N], w[N];
bool cmp(Y a, Y b) {
	return a.x < b.x;
}
bool cmp2(Y a, Y b) {
	return a.z < b.z;
}
int f[N];
int main() { 
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, t;
	cin >> n >> t;
	for(int i=1; i<=n; i++) {
		cin >> s[i].x >> s[i].y;//存入位置和方向
		w[i] = s[i], w[i].z = i;//记录初始位置和序号
		s[i].x += t*s[i].y;//t秒后的位置。
	}
	// for(int i=1; i<=n; i++) cout << s[i].x << ' ' << s[i].y << endl;

	sort(s+1, s+1+n, cmp);//按位置进行排序。
	sort(w+1, w+1+n, cmp);//把初始位置排序
	for(int i=1; i<=n; i++) s[i].z = w[i].z;//相对位置不变，就把序号附上去
	for(int i=1; i<n; i++) {//判断转向蚂蚁
		if(s[i].x == s[i+1].x) s[i].y = s[i+1].y = 0;
	}
	sort(s+1, s+1+n, cmp2);//按序号排序
	for(int i=1; i<=n; i++) {//输出
		cout << s[i].x << ' ' << s[i].y << endl;
	}
    return 0;
}
```





## [P1362 兔子数](https://www.luogu.com.cn/problem/P1362)

s运算：$s(123) =  1+2+3 =6$

兔子数：$s(x)*s(x) = s(x*x)$

求区间[n, m]中有多少个兔子数



结论：兔子数的每一位都要小于4

证明：有一个两位的兔子数u等于$10*x+y$，则$s(u^2)=s(u)*s(u)$

$s(100x^2+10xy+y^2)=x^2+xy+y^2$

只有当$x^2 < 10，xy < 10，y^2 < 10$时才能保证相等

所以：$x < 4， y < 4$。

我们只能在兔子数的基础上再在最前面增加数字使成为兔子数。

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef unsigned long long ll;
const int N = 2e5+10;
const int maxn = 2005;
const int INF = 0x3f3f3f3f;
const ll mod = 1e9+7;
ll c(ll x) {
    ll ans = 0;
    while(x) {
        ans += x % 10;
        x /= 10;
    }
    return ans;
}
//dfs枚举每一位。
ll cal(int x, int n, int m) {
    ll ans = 0;
    for(int i=0; i<4; i++) {
        ll tmp = x*10 + i;
        if(tmp == 0 || c(tmp)*c(tmp) != c(tmp*tmp)) continue;
        if(tmp <= m && tmp >= n) ans++;
        if(tmp <= m/10) ans += cal(tmp, n, m);
    }
    return ans;
}
int main() { 
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    ll n, m;
    cin >> n >> m;
    cout << cal(0, n, m);
    return 0;
}
```





## [P1249 最大乘积（规律+大数乘法）](https://www.luogu.com.cn/problem/P1249)



题意：给出一个数n，求出一组数，使得这一组数的和等于n，且这组数的乘积最大。

思路：从2一直加到n，使这组数的和大于等于n，求出超出了多少，再根据超出的大小来分情况讨论出结果。

[题解](https://www.luogu.com.cn/blog/NKU-AI/solution-p1249)



```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 10000;
int ans[N], s[N]={1}, len=1;

void mul(int x) {
    int t = 0;
    for(int i=0; i<len; i++) {
        s[i] = s[i] * x + t;
        t = s[i] / 10;
        s[i] %= 10;
    }
    if(t > 0) {
        s[len++] = t;
        while(s[len-1] > 10) s[len] = s[len-1]/10, s[len-1] %= 10, len++;
    }
}
int main(){
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, tmp = 0, i = 2;
    cin >> n;
    while(tmp < n) {
        ans[i] = i;
        tmp += i, i++;
    }
    if(tmp > n) {
        if(tmp - n == 1) {
            for(int k=2; k<i; k++) ans[k]++;
            i--, ans[i-1]++;
        }
        else {
            ans[tmp-n] = 1;
        }
    } 
    for(int j=2; j<i; j++) {
        mul(ans[j]);
        if(ans[j] == 1) continue;
        if(j == i-1) cout << ans[j];
        else cout << ans[j] << ' ';
    }
    cout << endl;
    for(int j=len-1; j>=0; j--) cout << s[j];
    return 0;
}

```





## [P1762 偶数](https://www.luogu.com.cn/problem/P1762)



规律：$2^k$，奇数个数一定是$2^k$，且前$n$行奇数个数和为$3^{k-1}$。并且一个数分解成$2^k$的形式，可以根据规律快速求出解。

举例：**2333 = 2048+256+16+8+4+1**

$ans(2333)=2048\times2^0+256\times2^1+16\times2^2+8\times2^3+4\times2^4+1\times2^5=190985$ 



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mod = 1000003;
const int inv2 = (mod+1)/2;
ll a[55], b[55];
int main() {
    ll n, ans = 0;
    cin >> n;
    ll m = n, k = 1, d = 1ll<<50, t = 50;
    while(n) {
        if(n >= d) n -= d, a[++a[0]] = t;
        d /= 2;
        t--;
    }
    b[0] = 1;
    for(int i=1; i<=a[1]; i++) b[i] = (b[i-1]*3) % mod;
    for(int i=1; i<=a[0]; i++) ans +=b[a[i]]*(ll)(k<<(i-1));
    ll sum = ((k+m%mod)*(m%mod)/2) % mod;
    ans %= mod;
    cout << (sum - ans + mod) % mod;
    return 0;
}
```



## *P4409 [ZJOI2006]皇帝的烦恼

结论 $or$ dp+二分

结论$ans = max(a_i+a_{i+1}, \lceil\frac {\sum a_i}{\lfloor\frac n2\rfloor}\rceil)$



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 2e4+10;
ll a[N];
bool check(int n, ll x) {
    for(int i=0; i<n; i++) {
        if(x-a[i] < a[(i+1) % n]) return false;
    }
    return true;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif  
    int n;
    cin >> n;
    ll l = 0, r = 0, mid;
    for(int i=0; i<n; i++) {
        cin >> a[i];
        r += a[i];
    }
    ll ans = 0;
    for(int i=0; i<n; i++) {
        ans = max(ans, a[i]+a[(i+1)%n]);
    }
    cout << max(ans, (ll)ceil(1.0*r/(n/2))) << endl;
    return 0;
}
```

















# [P6217 简单数论题](https://www.luogu.com.cn/problem/P6217)




$$
\begin{align}
&\prod_{{i=l}}^rlcm(a_i,x)\\
=&\prod_{i=l}^r\frac{a_ix}{gcd(a_i,x)}\\
=&\frac{\prod\limits_{i=l}^ra_ix}{\prod\limits_{i=l}^rgcd(a_i,x)}\\
\end{align}
$$
公式上半部分预处理

下半部分
$$
\begin{cases}
x&=&\prod_{i=1}^kp_{xi}^{q_{xi}}\\
a_i&=&\prod_{j=1}^kp_{ij}^{q_{ij}}
\end{cases}\\

\begin{align}
&\prod_{i=l}^rgcd(a_i,x)\\
=&\prod_{i=l}^r\prod_{j=1}^kp_{xj}^{min(q_{ij},q_{xj})}\\
=&\prod_{i=1}^kp_{xi}^{\sum\limits_{j=l}^rmin(q_{ji},q_{xi})}\\
=&\prod_{i=1}^kp_{xi}^{\sum\limits_{d=1}^{q_{xi}}\sum\limits_{j=l}^r[d\le q_{ji}]}\\
=&\prod_{i=1}^kp_{xi}^{\sum\limits_{d=1}^{q_{xi}}\sum\limits_{j=l}^r[p_{xi}^d|p_{xi}^{q_{ji}}]}\\
=&\prod_{i=1}^kp_{xi}^{\sum\limits_{d=1}^{q_{xi}}\sum\limits_{j=l}^r[p_{xi}^d|a_j]}\\
\end{align}
$$


思路：先预处理出每个$a_i$是哪些$p_{ij}$的倍数，用数组存下下标，对于一个区间$[l,r]$，我们只需要求出$x$的某个贡献$p_{xj}^k(k\in(1,q_{xj}))$

中包含在$[l,r]$区间的下标有多少个。



```CPP
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
typedef unsigned long long ull;
const int N = 2e5+10;
const int mod = 1e9+7;
ll fac[N];
int p[N], last[N], tot;
bool st[N];
vector<int> pr[N];
void init() {
	fac[0] = last[1] = 1;
	for(int i=2; i<N; i++) {
		if(!st[i]) p[tot++] = i, last[i] = i;
		for(int j=0; j<tot&&1ll*i*p[j]<N; j++) {
			st[i*p[j]] = 1;
			last[i*p[j]] = p[j];
			if(i % p[j] == 0) break;
		}
	}
}
ll qpow(ll x, ll y) {
	ll ans = 1;
	while(y) {
		if(y & 1) ans = ans * x % mod;
		x = x * x % mod;
		y >>= 1;
	}
	return ans;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
	init();
	int n, m, x;
	scanf("%d%d", &n, &m);
	for(int i=1; i<=n; i++) {
		scanf("%d", &x);
		fac[i] = fac[i-1] * x % mod;
		int tmp = x;
		while(tmp != 1) {
			int u = last[tmp], v = 1;
			while(tmp % u == 0) {
				tmp /= u;
				v *= u;
				pr[v].push_back(i);
			}
		}
	}
	int l, r;
	for(int i=1; i<=m; i++) {
		scanf("%d%d%d", &l, &r, &x);
		int tmp = x;
		ll res = 1;
		while(tmp != 1) {
			int u = last[tmp], v = 1, cnt = 0;
			while(tmp % u == 0) {
				tmp /= u;
				v *= u;
				cnt += upper_bound(pr[v].begin(), pr[v].end(), r)-lower_bound(pr[v].begin(), pr[v].end(), l);
			}
			res = res * qpow(u, cnt) % mod;
		}
		printf("%lld\n", fac[r]*qpow(fac[l-1], mod-2)%mod*qpow(x, r-l+1)%mod*qpow(res, mod-2)%mod);
	}
	return 0;
}
```









