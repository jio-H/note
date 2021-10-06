## [ C. String Equality](https://codeforces.com/problemset/problem/1451/C)



```
#include <cstdio>
#include <cstring>
#include <vector>
#include <algorithm>
#include <queue>
#include <cmath>
using namespace std;
const int maxn = 1e6 + 7;
int a[30],b[30];
char s1[maxn],s2[maxn];
int main() {
    int T;scanf("%d",&T);
    while(T--) {
        int n,k;scanf("%d%d",&n,&k);
        scanf("%s%s",s1 + 1,s2 + 1);
        memset(a,0,sizeof(a));
        memset(b,0,sizeof(b));
        for(int i = 1;i <= n;i++) {
            a[s1[i] - 'a']++;
            b[s2[i] - 'a']++;
        }
        int cnt = 0;
        int flag = 1;
        for(int i = 0;i < 26;i++) {
            int num = a[i] - b[i];
            if(num % k != 0) {
                flag = 0;break;
            }
            if(i == 25 && num > 0) {
                flag = 0;break;
            }
            cnt += num / k;
            if(cnt < 0) {
                flag = 0;break;
            }
        }
        if(cnt != 0 || flag == 0) {
            printf("No\n");
        } else {
            printf("Yes\n");
        }
    }
    return 0;
}

```



## [B. Suffix Operations](https://codeforces.com/problemset/problem/1453/B)



```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 2e5+10;
int read() {
	int f = 1, ans = 0;
	char ch = getchar();
	while(ch < '1' || ch > '9') if(ch == '-') f = -1, ch = getchar();
	while(ch >= '1' && ch <= '9') ans += ans*10+ch-48, ch = getchar();
	return ans*f;
}
long long a[N];

int main() {
	int t;
    cin >> t;
    while(t--) {
        int n; cin >> n;
        for(int i=1; i<=n; i++) cin >> a[i];
        long long sum = 0, ans1 = 0, ans2 = 0, mx = 0;
        for(int i=n-1; i>=1; i--) {
            sum += abs(a[i] - a[i+1]);
            if(i+2 <= n) 
                mx = max(mx, abs( abs(a[i]-a[i+1])+abs(a[i+1]-a[i+2])-abs(a[i]-a[i+2]) ));
        }
        for(int i=n-1; i>=2; i--) ans1 += abs(a[i] - a[i+1]);
        for(int i=n-2; i>=1; i--) ans2 += abs(a[i] - a[i+1]);
        cout << min(sum-mx, min(ans1, ans2)) << endl;
    }
	return 0;
}
```



## [A1. Binary Table (Easy Version)](https://codeforces.com/problemset/problem/1439/A1)

思路：对一个$2\times2$的矩阵，我们可以通过三次操作达到只改变其中一个数的效果，然后可以直接枚举每个位置（注意边界的处理）（题目提示，在$3nm$次操作内必定有解）。

```cpp
#include<bits/stdc++.h>
using namespace std;
string s[110];
struct Y{
    int x1, y1, x2, y2, x3, y3;
    Y (int _x1,int _y1,int _x2,int _y2,int _x3,int _y3):x1(_x1),y1(_y1),x2(_x2),y2(_y2),x3(_x3),y3(_y3) {}
};
vector<Y> p;
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int t;
    cin >> t;
    while(t--) {
        int n, m;
        cin >> n >> m;
        for(int i=0; i<n; i++) cin >> s[i];
        for(int x=0; x<n-1; x++) {
            for(int y=0; y<m-1; y++) {
                if(s[x][y] == '1') {
                    s[x][y] = '0';
                    p.push_back(Y(x,y,x+1,y,x,y+1));
                    p.push_back(Y(x,y,x+1,y+1,x,y+1));
                    p.push_back(Y(x,y,x+1,y+1,x+1,y));
                }
            }
        }
        for(int i=0; i<n-1; i++) {
            if(s[i][m-1] == '1') {
                s[i][m-1] = '0';
                p.push_back(Y(i,m-1,i,m-2,i+1,m-1));
                p.push_back(Y(i,m-1,i+1,m-1,i+1,m-2));
                p.push_back(Y(i,m-1,i,m-2,i+1,m-2));
            }
        }
        for(int i=0; i<m-1; i++) {
            if(s[n-1][i] == '1') {
                s[n-1][i] = '0';
                p.push_back(Y(n-1,i,n-2,i,n-1,i+1));
                p.push_back(Y(n-1,i,n-1,i+1,n-2,i+1));
                p.push_back(Y(n-1,i,n-2,i,n-2,i+1));
            }
        }
        if(s[n-1][m-1] == '1') {
            s[n-1][m-1] = '0';
            p.push_back(Y(n-1,m-1,n-2,m-1,n-1,m-2));
            p.push_back(Y(n-1,m-1,n-1,m-2,n-2,m-2));
            p.push_back(Y(n-1,m-1,n-2,m-1,n-2,m-2));
        }
        cout << p.size() << endl;
        for(int i=0; i<p.size(); i++) {
            printf("%d %d %d %d %d %d\n",p[i].x1+1,p[i].y1+1,p[i].x2+1,p[i].y2+1,p[i].x3+1,p[i].y3+1);
        }
        p.clear();
    }
    
    return 0;
}
```







## [A. k-Amazing Numbers](https://codeforces.com/problemset/problem/1416/A)

[题解](https://blog.csdn.net/ACkingdom/article/details/108848337)

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 3e5+10;
int a[N], x[N], y[N], z[N];
/*a数组存放输入的n个数，x数组存放两个相同元素之间间距的最大值，
y数组是中间值，用来存放上一个a[i]的位置，z数组用来存放k为i的最小值*/
int main() {
    int t;
    cin >> t;
    while(t--) {
        memset(x, 0, sizeof x);
        memset(y, 0, sizeof y);
        memset(z, 0, sizeof z);
        int n;
        cin >> n;
        //处理x数组
        for(int i=1; i<=n; i++) {
            cin >> a[i];
            x[a[i]] = max(i-y[a[i]], x[a[i]]);
            y[a[i]] = i;    
        }
        for(int i=1; i<=n; i++) {
            x[a[i]] = max(n-y[a[i]]+1, x[a[i]]);
        }

        //处理z数组。
        for(int i=1; i<=n; i++){
            if(z[x[i]]==0) z[x[i]] = i;
            else z[x[i]] = min(i, z[x[i]]);
        }

        //输出答案。
        int ans = -1;
        for(int i=1; i<=n; i++) {
            if(z[i]==0&&ans==-1) cout<<-1<<' ';
            else if(ans == -1) {
                ans = z[i];
                cout << ans << ' ';
            }
            else if(z[i] == 0) cout << ans << ' ';
            else {
                ans = min(ans, z[i]);
                cout << ans << ' ';
            }
        }
        cout << endl;
    }
    return 0;
}
```



## [C. Good String](https://codeforces.com/problemset/problem/1389/C)

[题解](https://www.cnblogs.com/2018slgys/p/13405065.html)



思路：只有两种情况可以满足条件，$xxxxxxxx$或$xyxyxyxy(偶数长度)$。

所以$cnt1$是求$xxxxx$的长度，$cnt2$是求$xyxyxyxy$的长度，我们取一个最大值，就可以得到最小擦去数。

```cpp
#include<bits/stdc++.h>
using namespace std;
int s[100010];
int cnt1(int n, int x) {
    int ans = 0;
    for(int i=0; i<n; i++) {
		if(s[i] == x) ans++;
    }
    return ans;
}
int cnt2(int n, int x, int y) {
    int ans = 0;
	for(int i=0; i<n; i++) {
        if(ans & 1) {
            if(s[i] == y) ans++;
        }
        else {
			if(s[i] == x) ans++;
        }
    }
    if(ans & 1 == 0) ans--;
    return ans;
}
int solve(int n) {
	int ans = 0;
    for(int i=0; i<10; i++) {
		ans = max(ans, cnt1(n, i));
    }
    for(int i=0; i<10; i++) {
		for(int j=0; j<10; j++) {
            if(i != j) ans = max(ans, cnt2(n, i, j));
        }
    }
    return n-ans;
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) {
		string a;
        cin >> a;
        for(int i=0; i<a.size(); i++) s[i] = a[i]-'0';
        cout << solve(a.size()) << endl;
    }
    return 0;
}
```



## [A. Strange Birthday Party](https://codeforces.com/problemset/problem/1470/A)(1300)

题意：给出n个人，每个人一个值$k_i$，m个礼物，每个礼物的价值是$c_i$（单调递增），现给出每个人都一个礼物，求最小花费

给礼物的规则：可以给第i个人挑第j个礼物要满足$j <= k_i$，也可以直接给第i个人$c_{k_i}$的钱。



题解：贪心，把最小价值的礼物先给$k_i$比较大的人。

从每个$c_i$用多少次的角度看（很难，我想的）

**从每个人要花费的钱的多少的角度：**

每个人都要一定的花费，$k_i$较大的人如果给钱的话就要花费$c_{k_i}$，比给礼物要多。而如果$k_i$比较小的话，还给礼物的话，就会浪费钱。所以要优先给$k_i$大的人礼物

举个例子：$k_1 = 1, k_2=5$.c={1,2,3,4,5}

如果先给$k1$礼物的话，最小的花费是3。

如果先给$k2$礼物的话，最小的花费是2。



``` 
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 3e5+10;
ll k[N], c[N];
int main() {
    int t;
    scanf("%d", &t);
    while(t--) {
        int n, m;
        scanf("%d%d", &n, &m);
        for(int i=1; i<=n; i++) scanf("%lld", &k[i]);
        for(int i=1; i<=m; i++) scanf("%lld", &c[i]);
        sort(k+1, k+1+n, greater<int>());        
        ll ans = 0;
        for(int i=1, j=1; i<=n; i++) {
            if(k[i] > j) ans += c[j], j++;
            else ans += c[k[i]];
        }
        printf("%lld\n", ans);
    }
	return 0;
}
```







[错误代码](https://paste.ubuntu.com/p/cpVNrvKvxH/)

错误代码思路：

先将第一个数不变，然后后面的每一个数就是前面得倍数或因子。

代码出了问题，思路没捋清楚，有点难搞。

正确思路：

将每个数都弄成2的整数倍，$2b_i>a_i$

CODE

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 55;
ll a[N], ans[N];
ll c(ll x) {
    ll p = 1;
    while(2*p < x) p *= 2;
    return p;
} 
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        ll sum = 0, u = 0;
        for(int i=1; i<=n; i++) {
            cin >> a[i];
        }
        for(int i=1; i<=n; i++) {
            ans[i] = c(a[i]);
            cout << ans[i] << ' ';
        }
        cout << endl;
    }
    return 0;
}
```





## C. Array Destruction

题意：给出一个数列，有$2n$个数，问是否存在一个$x$，使$a_i+a_j=x(i!=j)$。我们再将$x=max(a_i,a_j)$，之后重复前一个操作，每个数只能用一次，问是否存在一个数$x$，使全部的数都被选到，如果存在输出选择顺序。



思路：最大值肯定是第一个被选的值之一，（简单证明：如果第一次选的是第二大的值，那最大的值就一定选不上了。)然后，我们再枚举另一个数$a_j$找到$x$，在第二次选择时，已知$x=$最大值，第二大的值，这样我们直接去剩下的数里面去找另一个值，如果存在，继续直到能够选完全部的数，如果不存在，换一个数$a_j$重新开始判断，直到全部的数都被枚举完之后。

选择的过程中用到了$muliset$，方便选择。

```cpp
#include<bits/stdc++.h>
using namespace std;
vector<int> a;
//对于每种 第一次选择判断是否是可以成功的。
vector<int> check(vector<int> x, int u, int n) {
    multiset<int> s;
    vector<int> ans;
    for(auto it:x) s.insert(it);
    for(int i=0; i<n; i++) {//n次选择。
        
        auto it1 = s.end();//当前最大值。
        it1--;
        int y = u-*it1;
        s.erase(it1);
        auto it2 = s.find(y);
        if(it2 == s.end()) return {};//选择失败，返回空数组。
        //把选择顺序加入数组中。
        ans.push_back(y);
        ans.push_back(u-y);
        u = max(y, u-y);
        s.erase(it2);
        
    }
    return ans;
}

int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int t;
    cin >> t;
    while(t--) {
        int n, x, flag = 0;
        cin >> n;
        for(int i=0; i<2*n; i++) {
            cin >> x;
            a.push_back(x);
        }
        sort(a.begin(), a.end());
        for(int i=0; i<2*n-1; i++) {//枚举每个a_j
            
            int k = a[i] + a[a.size()-1];
            vector<int> v = check(a, k, n);
  			//选择成功
            if(v.size()) {
                printf("Yes\n%d\n", k);
                for(int j=0; j<n; j++) {
                    printf("%d %d\n", v[j*2], v[j*2+1]);
                }
                flag = 1;
                break;
            }
            
        }
        if(!flag) printf("No\n");
        a.clear();
    }
    return 0;
}
```



## [B. Different Divisors](https://codeforces.com/problemset/problem/1474/B)



题意：给出一个数d，求一个数a。满足至少四个因子，每个因子按大小进行排序，每个相邻的因子相差至少为d。

思路：每个数至少都有1和它本身两个因子，我们找到离1最近的素数x，再找到理x最近的素数y，答案就是x*y。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e7+10;
const ll mod = 1e9+7;
int p[N],tot; 
bool st[N];
void init() {
    for(int i=2; i<N; i++) {
        if(!st[i]) p[tot++] = i;
        for(int j=0; j<tot&&i*p[j]<N; j++) {
            st[i*p[j]] = 1;
            if(i % p[j] == 0) break;
        }
    }
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    init();
    int t;
    cin >> t;
    while(t--) {
        ll d;
        cin >> d;
        int i, j;
        for(i=0; i<tot; i++) {
            if(p[i]-1 >= d) break; 
        }
        for(j=i+1; j<tot; j++) {
            if(p[j]-p[i] >= d) break;
        }
        cout << 1ll*p[i]*p[j] << endl;
    }
    return 0;
}

```





## [D. Fox And Jumping](https://codeforces.com/contest/510/problem/D)

题意：在一个无限长的纸条上，给出n个卡片，每个卡片都有有两个属性l，c。我们可以花费c得到这个卡片，就能够用这张卡片来向左或者向右移动l的距离，问最小的代价使可以到达枝条上的全部点。

思路：我们可以把纸条想象成坐标轴，起始位置为0，这样我们花费$c_i$得到卡片$i$，就相当于能够写作$0+\sum_{i=1}^kx_i\times l_i(k未知，x_i为一个常数)$。这样一来我们要遍历到每个点的话$\sum_{i=1}^kx_i\times l_i=1$，根据裴蜀定理，我们就能得到$gcd(l_1,l_2,...l_k)=1$。

我们就要找到最小花费的组合使$gcd = 1$（动态规划）。

我们设$dp[i]$表示：$gcd = i$的最小花费。$i$不是连续的而且可能很大，就用$map$来搞。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef unsigned long long ll;
const int N = 310;
int l[N], c[N];
map<int, int> dp;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n;
    cin >> n;
    for(int i=1; i<=n; i++) cin >> l[i];
    for(int i=1; i<=n; i++) cin >> c[i];
    for(int i=1; i<=n; i++) {
        dp[l[i]] = dp[l[i]] ? min(dp[l[i]], c[i]) : c[i];
        for(auto it : dp) {
            int g = __gcd(l[i], it.first);
            int C = dp[l[i]] + it.second;
            
            dp[g] = dp[g] ? min(dp[g], C) : C;
        }
    }
    cout << (dp[1] ? dp[1] : -1);
    return 0;
}
```







## [C. Maximum width](https://codeforces.ml/problemset/problem/1492/C)



```cpp
#include<bits/stdc++.h>
using namespace std;

typedef unsigned long long ll;
const int N = 2e5+10;
const int maxn = 2005;
const int INF = 0x3f3f3f3f;
const ll mod = 1e9+7;

char s[N], t[N];
int u[N], v[N];
int main() { 
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, m, ans = 0;
    cin >> n >> m;
    cin >> s+1;
    cin >> t+1;
    for(int i=1, j=1; i<=m; i++, j++) {
		while(t[i] != s[j]) j++;
		u[i] = j;
	}
	for(int i=m, j=n; i>0; i--, j--) {
		while(t[i] != s[j]) j--;
		v[i] = j;
	}
	for(int i=1; i<m; i++) ans = max(ans, v[i+1]-u[i]);
    cout << ans << endl;
    return 0;
}
```





## [B. Berland Crossword](https://codeforces.ml/problemset/problem/1494/B)

二进制枚举四个角上是否涂色

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 1e2+10;
int n;
bool check(int x, int y, int z) {
    if(!y&&!z) return x <= n-2;
    if(y&&!z) return 1<=x && x<=n-1;
    if(!y&&z) return 1<=x && x<=n-1;
    if(y&&z) return 2<=x;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int t;
    cin >> t; 
    while(t--) {
        int u, l, d, r, f = 1;
        cin >> n >> u >> l >> d >> r;
        for(int i=0; i<(1<<4); i++) {
            int a=(i&1), b=(i&(1<<1)), c=(i&(1<<2)), j=(i&(1<<3));
            if(check(u,a,b)&&check(l,b,c)&&check(d,c,j)&&check(r,j,a)) {
                f = 0;
                // cout << i << endl;
                break;
            }
        }
        if(f) cout << "NO\n";
        else cout << "YES\n";
    }
    return 0;
}
```





## *[C. Pekora and Trampoline](https://codeforces.ml/problemset/problem/1491/C)

[题解](https://blog.csdn.net/Brian_Pan_/article/details/114259573)



```cpp
#include <bits/stdc++.h>
using namespace std;

#define int long long

void solve()
{
    int n;
    cin >> n;

    vector<int> s(n + 2), c(n + 4);
    int t = 0, ans = 0;

    for (int i = 1; i <= n; i++)
        cin >> s[i];

    for (int i = 1; i <= n; i++)
    {
        t += c[i];
        c[i + 2]++;
        c[min(n + 1, i + s[i] + 1)]--;
        c[i + 1] += max(0ll, t - s[i] + 1);
        c[i + 2] -= max(0ll, t - s[i] + 1);
        ans += max(s[i] - t - 1, 0ll);
    }

    cout << ans << endl;
}

signed main()
{
    ios::sync_with_stdio(false);

    int t;
    cin >> t;

    while (t--)
        solve();
}
```







## *[D. Zookeeper and The Infinite Zoo](https://codeforces.com/problemset/problem/1491/D)思维+位运算



给出u，v，问有没有从u到v的边。

根据题意：u的二进制位的1每次都只能向前移动，且个数都会变成小于等于。

$6(110)_2$能够变成$8(1000)_2$，$10(1010)_2$，$12(1100)_2$。

我们只要比较$v$是否大于u，以及1的个数是否小于等于u，和是不是全部1的位置都比u最左边的1位置大。

```cpp
#include<bits/stdc++.h>
using namespace std;
bool check(int x, int y) {
    int a = 0, b = 0;
    for(int i=0; i<31; i++) {
        if((x>>i)&1) a++;
        if((y>>i)&1) b++;
        if(b) {
            if(a == 0) return false;
            a--, b--;
        }
    }
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int t;
    cin >> t;
    while(t--) {
        int n, m;
        cin >> n >> m;
        if(n <= m && check(n, m)) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}
```



## [C1. k-LCM (easy version)](https://codeforces.ml/problemset/problem/1497/C1)

问题：将$n$分解成k个数相加（k为3），使得这k个数的$lcm\le \frac n2$

思路：如果n是奇数提出一个1，剩下一个偶数平分，$lcm=\frac {n-1}2$。

​			如果n是一个偶数：如果$\frac n2$也是一个偶数，我们就分两次，为$\frac n2, \frac n4, \frac n4,lcm=\frac n2$.

​											  否则我们就先提出一个2，再平分。



```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif  
    int t;
    cin >> t;
    while(t--) {
        int n, k;
        cin >> n >> k;
        if(n % 2) cout << 1 << ' ' << (n-1)/2 << ' ' << (n-1)/2 << endl;
        else {
            int k = n/2;
            if(k % 2 == 0) cout << k << ' ' << k/2 << ' ' << k/2 << endl;
            else cout << 2 << ' ' << (n-2)/2 << ' ' << (n-2)/2 << endl;
            
        }
    }
    return 0;
}
```



## [C2 k-LCM (hard version)](https://codeforces.ml/problemset/problem/1497/C2)

问题：将$n$分解成k个数相加，使得这k个数的$lcm\le \frac n2$

思路：将k-3个位置都填1，剩下三个位置按easy version放剩下的数。

```cpp
#include<bits/stdc++.h>
using namespace std;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif  
    int t;
    cin >> t;
    while(t--) {
        int n, k;
        cin >> n >> k;
        int tmp = n-k+3;
        if(tmp % 2) {
            cout << 1 << ' ' << (tmp-1)/2 << ' ' << (tmp-1)/2;
        }
        else {
            int aa = tmp/2;
            if(aa % 2) cout << 2 << ' ' <<  (tmp-2)/2 << ' ' <<  (tmp-2)/2;
            else cout << aa << ' ' << aa/2 <<  ' ' << aa/2;
        }
        for(int i=0; i<k-3; i++) {
            cout << ' ' << 1;
        }
        cout << endl;
    }
    return 0;
}
```







## [E. Colorings and Dominoes](https://codeforces.ml/problemset/problem/1511/E)

如何确定对行进行贡献的统计和对列进行贡献的统计不会产生影响。

计算贡献的最小单位是一行或一列。

我们求出某一行(列)的贡献时会乘以$2^{sum-cnt}$代表剩下的$sum-cnt$个o有多少种可能，在每种可能下这一行产生的贡献始终不会改变。



$dp$状态转移方程

$dp[i]$表示连续的i个o能产生多少贡献。

举例：$oooo$，假设我们已知前3个$o$的贡献$dp[3$]，我们假设第4个$o$涂上蓝色，$dp[4]+=dp[3]$，假设第4个涂上红色，我们把这4个$o$分成两部分，左边两个$oo$，右边两个$oo$，

1. 右边两个$oo$只有在都是红色的时候才能产生贡献，对$dp[4$]产生的贡献就是$2^2$.
2. 左边两个$oo$产生的贡献：因为第3个$o$可以涂两种颜色，对$dp[4$]产生的贡献就是$2*dp[3]$。

综上：$dp[n] = dp[n-1]+2*dp[n-2]+2^{n-2}$;



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 3e5+10;
const int mod = 998244353;
ll dp[N], f[N];
//dp[i]表示连续的i个o能产生多少贡献。
string s[N];
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
    int n, m;
    cin >> n >> m;
    f[0] = 1, f[1] = 2;
    for(int i=0; i<n; i++) cin >> s[i];
    for(int i=2; i<N; i++) {
        dp[i] = ((dp[i-1] + 2ll*dp[i-2] % mod) % mod + f[i-2]) % mod;
        f[i] = f[i-1] * 2 % mod;
    }
    ll cnt = 0, ans1 = 0, ans2 = 0, sum = 0;
    for(int i=0; i<n; i++) {
        for(int j=0; j<m; j++) {
            if(s[i][j] == 'o') sum++;
        }
    }
    for(int i=0; i<n; i++) {
        for(int j=0; j<m; j++) {
            if(s[i][j] == 'o') cnt++;
            if(s[i][j] != 'o' || j == m-1) {
                if(cnt != 1) {
                    ans1 = (ans1 + dp[cnt]*f[sum-cnt]%mod) % mod;
                }
                cnt = 0;
            }
        }
    }
    cnt = 0;
    for(int j=0; j<m; j++) {
        for(int i=0; i<n; i++) {
            if(s[i][j] == 'o') cnt++;
            if(s[i][j] != 'o' || i == n-1) {
                if(cnt != 1) {
                    ans2 = (ans2 + dp[cnt]*f[sum-cnt]%mod) % mod;
                }
                cnt = 0;
            }
        }
    }
    cout << (ans1 + ans2) % mod << endl;
    return 0;
}
```









## [Codeforces Round #716 (Div. 2)](https://codeforces.com/contest/1514)



### [A. Perfectly Imperfect Array](https://codeforces.com/contest/1514/problem/A)

题意：给出一个序列，问是否存在一个子序列连乘不是完全平方数。

思路：如果在序列中存在一个非完全平方数，那么一定存在子序列连乘不是完全平方数。

我们就可以对序列的每个数质因子分解，看每个质因子的次数是不是偶数，如果存在不是就说明这个数不是完全平方数。

参考代码

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int s[110];
bool check(int n) {
    for(int i=1; i<=n; i++) {
        for(int j=2; j*j<=s[i]; j++) {//对某个数进行质因子分解.
            int cnt = 0;
            if(s[i] % j == 0) {
                while(s[i] % j == 0) cnt++, s[i]/=j;
                if(cnt % 2) return true;
            }
        }
        if(s[i] > 1) return true;
    }
    return false;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        for(int i=1; i<=n; i++) cin >> s[i];
        if(check(n)) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}
```



### [B. AND 0, Sum Big](https://codeforces.com/contest/1514/problem/B)

题意：给出n$和$$k$，问构造长度为n的序列满足以下条件的有多少个（对$10^9+7$取模）

1. 每个元素的取值范围为$[0, 2^k-1]$
2. 全部元素按位与后为0
3. 全部元素的和尽可能的大。

思路：元素取值范围是$[0,2^k-1]$，将一个数转换成2进制的形式下就有k个位置。

对n个数来说，只要在k个位置上都至少存在一个0，那么按位与之后的结果就会变成0。要使全部元素的和尽可能的大，那么每个位置只能出现一个0。

每个位置上的那一个0都有n种选择，一共有k个位置，答案就是$n^k$。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mod = 1e9+7;
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
    int t;
    cin >> t;
    while(t--) {
        int n, m;
        cin >> n >> m;
        cout << qpow(n, m) << endl;
    }
    return 0;
}
```



### [C. Product 1 Modulo N](https://codeforces.com/contest/1514/problem/C)

题意：给定一个n，问在区间$[1,n-1]$中选择一个最长的子序列使他们的连乘模上n为1。

思路：区间内与n全部互质的数都存放下来，如果这些数的连乘不满足条件，就把连乘等于的那个数去掉再输出，一定满足条件。



子序列的每个数都必须和n互质。

证明：$a\times b=1(mod~n)$，表示a在模n下存在逆元为b，它的充要条件就是$gcd(n,a)=1$。

假设在子序列中存在一个数不与n互质，则连乘到这个数时将不再满足条件$gcd(a, n)=1$。在区间中也就不会再有下一个数使连乘模n为1。





把连乘等于的那个数去掉

证明：这个数一定在已经选择的数中，因为，每个数都和n互质，所以连乘的数也和n互质且小于n，所以一定在子序列中，只要除去这个数，连乘就是1了。



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int mod = 1e9+7;
ll f[100010];
ll qpow(ll x, ll y) {
    ll ans = 1;
    while(y) {
        if(y & 1) ans = ans * x % mod;
        x = x * x % mod;
        y >>= 1;
    }
    return ans;
}
vector<int> v;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int n, ans = 1, f = 0;
    cin >> n;
    for(int i=1; i<n; i++) {
        if(__gcd(i, n) == 1) {
            v.push_back(i);
            ans = 1ll * ans * i % n;
        }
    }
    int tmp = 0;
    if(ans != 1) {
        cout << v.size()-1 << endl;
        tmp = ans;
    }
    else cout << v.size() << endl;
    for(auto it : v) {
        if(it != tmp) cout << it << ' ';
    }
    return 0;
}
```





###  [D. Cut and Stick](https://codeforces.ml/problemset/problem/1514/D)

题意：给出一个长度为n的序列，m个询问。对于每个询问，我们要将询问序列按要求划分成多个序列，使每个序列都满足条

1. 划分序列的方法：从序列中选取子序列来组成新的区间
2. 条件：设序列长度为$len$， 序列中每种元素的个数不能超过$\lceil\frac {len}d\rceil$。

思路：我们设在序列中的出现次数最多的是$x$次，如果$\lceil\frac {len}d\rceil\le x$。最优的解法就是$2*x-len$。

然后我们用莫队来维护序列中的$x$，就能得到每个询问的答案。



最优的解法就是$2*x-len$

推导：$x$个相同的元素，$len-x$个其他元素，这$len-x$个元素最多再添加$len-x+1$，剩下的相同的元素有$x-(len-x+1)$个，这些每个都要单独成一个序列才能满足。

综上划分出来的序列的个数为：$2\times x-len$。



```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 3e5+10;
int block, ans = 0;
int s[N], cnt[N], t[N], res[N];
struct Y {
	ll l, r, i;
	Y(int x=0, int y=0, int z=0):l(x), r(y), i(z) {}
} a[N];

//奇偶化分块
bool cmp1(Y a, Y b) {
    if(a.l/block != b.l/block) return a.l < b.l;
    if((a.l/block) & 1) return a.r < b.r;
    return a.r > b.r;
}
void add(int x) {
    t[cnt[x]]--;
    cnt[x]++;
    t[cnt[x]]++;
    ans = max(cnt[x], ans);
}
void del(int x) {
    t[cnt[x]]--;
    if(t[cnt[x]] == 0 && ans == cnt[x]) ans--;
    cnt[x]--;
    t[cnt[x]]++;
}

int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int n, m, x, y;
    scanf("%d%d", &n, &m);
    block = ceil(sqrt(n));
    for(int i=1; i<=n; i++) cin >> s[i];
    for(int i=1; i<=m; i++) {
        scanf("%d%d", &x, &y);
        a[i] = Y(x, y, i);
    }
    int l = 1, r = 0;
    sort(a+1, a+1+m, cmp1);
    for(int i=1; i<=m; i++) {
        while(l > a[i].l) l--, add(s[l]);
        while(r < a[i].r) r++, add(s[r]);
        while(l < a[i].l) del(s[l]), l++;
        while(r > a[i].r) del(s[r]), r--;

        res[a[i].i] = ans*2 - (r-l+1);
    }
    for(int i=1; i<=m; i++) printf("%d\n", max(res[i], 1));
    return 0;
}
```





## [Codeforces Round #717 (Div. 2)](https://codeforces.com/contest/1516)



### [A. Tit for Tat](https://codeforces.com/contest/1516/problem/A)

题意：给出一个长为$n$的序列，可以进行小于等于$k$次操作，每次操作能选择两个不同位置上的元素，一个加1，一个减1。求操作之后字典序最小的序列是什么。

思路：每次都把最后一个元素加一，前面元素减一，该元素为0后，跳到下一个元素，直到k为0或到达最后一个元素，结束。

参考代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int s[110];
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int t;
    cin >> t;
    while(t--) {
        int n, k;
        cin >> n >> k;
        for(int i=1; i<=n; i++) {
            cin >> s[i];
        }
        int i = 1;
        for(int i=1; i<n; i++) {
            if(s[i] <= k) {
                s[n] +=s[i];
                k -= s[i];
                s[i] = 0;
            }
            else {
                s[n] += k;
                s[i] -= k;
                break;
            }
        }
        for(i=1; i<=n; i++) cout << s[i] << ' ';
        cout << endl;
    }
    return 0;
}
```





### [B. AGAGA XOOORRR](https://codeforces.com/contest/1516/problem/B)

题意：给出长度为n的序列，问是否存在某种划分子串的方式使，每个子串的异或和相等。

思路：假设答案有k个子串，如果k使偶数，可以分成两边，可知左边异或和等于右边异或和；如果k是奇数，那么一定可以化成3个异或和相等的子串（举例，答案{3，3，3，3，3}， 可以分成{[3]，[3，3，3]，[3]}，也满足条件）。

参考代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int s[2010];
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        for(int i=1; i<=n; i++) {
            cin >> s[i];
            s[i] = s[i] ^ s[i-1];
        }
        int f = 1;
        for(int i=1; i<n; i++) {
            if((s[n] ^ s[i]) == s[i]) {
                f = 0;
                break;
            }
            for(int j=i+1; j<n; j++) {
                if(s[i] == (s[j]^s[i]) && s[i] == (s[n]^s[j])) {
                    f = 0;
                    break;
                }
            }
        }
        if(f) cout << "NO\n";
        else cout << "YES\n";
    }
    return 0;
}
```



### [C. Baby Ehab Partitions Again](https://codeforces.com/contest/1516/problem/C)

题意：给出一个长为n的序列，问删除最少个元素，使序列不满足：将序列分成两部分子序列（不存在相交元素），使得这两个子序列的和相等。

思路：设序列和是$sum$。

1. 如果$sum$是一个奇数，这种情况满足条件，
2. 如果sum是偶数，但不存在子序列和为$\frac {sum}2$，这种情况也满足条件。
3. 如果sum是偶数，且存在子序列和为$\frac {sum}2$，我们就可以通过删除某个元素来变成第一种情况。

说明：删除的元素是序列中某一个确定的值。

证明：

在情况3下，已知$sum$是偶数，和为$\frac {sum}2$的子序列存在，且我们必须删除一个或多个元素来使条件成立。

将子序列化成$\{2^{c_1}a_1,2^{c_2}a_2,...,2^{c_n}a_n\}$的形式，每个$a_i$都是奇数，设最小的$c_i$为$c_k$，$a_k$一定是奇数。当每个元素都除以$2^{c_k}$，仍然可得到和为$\frac {sum}2$的子序列。我们只要将$a_k$除去，剩下的sum一定是一个奇数（第一种情况），满足条件。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
int s[110], dp[2000010];
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    int n, ans = 0, mi = 100, f, sum = 0;
    cin >> n;
    for(int i=1; i<=n; i++) {
        cin >> s[i];
        int tmp = s[i];
        ans = 0, sum += s[i];
        while(!(tmp & 1)) {
            ans++, tmp >>= 1;
        }
        if(ans < mi) {
            mi = ans;
            f = i;
        }
    }
    dp[0] = 1;
    for(int i=1; i<=n; i++) {
        for(int j=sum; j>=0; j--) {
            if(dp[j] && j+s[i] <= 200000) dp[j+s[i]] = 1; 
        }
    }
    if((sum & 1) || !dp[sum/2]) cout << "0\n";
    else cout << "1\n" << f << '\n';
    return 0;
}
```



### [D. Cut](https://codeforces.com/contest/1516/problem/D)

题意：给出一个长度为n的序列，和q个询问，对于每个询问，求出最少分成多少个连续的子区间，使每个子区间的$lcm$等于子区间各个数的乘积。

思路：质因子分解+倍增思想。

1. 预处理：利用质因子分解来找当前元素能够向后跳到的最远的位置（满足从开始位置到结束位置中的每个元素都互质）

2. 利用倍增的思想，来加速跳的过程。
3. 对于每个询问，只要从没有跳到边界，就一直往下跳，直到位置大于右边界，跳的次数加一就时答案。



预处理：$dp[i][0]$表示区间$[i，dp[i][0]]$中所有的元素都是互质的。

倍增：$dp[i][j]$表示从$i$开始跳$2^j$次，跳到的位置(注意每次跳的长度不一定是相同的)，也就是分成了$2^j$个区间。

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N = 1e5+10;
int s[N], ne[N], dp[N][20];
unordered_map<int, int> p;
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    memset(ne, 0x7f, sizeof ne);
    int n, m, x, y;
    cin >> n >> m;
    for(int i=1; i<=n; i++) {
        cin >> s[i];
    }
    //预处理
    dp[n+1][0] = n+1;
    for(int i=n; i>=1; i--) {
        int tmp = s[i];
        dp[i][0] = dp[i+1][0]; //继承前一个元素能到到最大位置。
        for(int j=2; 1ll*j*j<=tmp; j++) {//质因子分解
            if(tmp % j == 0) {
                if(p[j]) ne[i] = min(p[j], ne[i]);//判断之前是否出现过这个素数。
                p[j] = i;//标记
                while(tmp % j == 0) tmp /= j;
            }
        }
        if(tmp > 1) {
            if(p[tmp]) ne[i] = min(p[tmp], ne[i]);
            p[tmp] = i;
        }
        if(ne[i] == 2139062143) ne[i] = n+1;
        dp[i][0] = min(ne[i], dp[i][0]);//取最小值，为了使从开始位置到结束位置中的每个元素都互质
    }
    for(int i=n+1; i>=1; i--) {
        for(int j=1; j<=17; j++) {//倍增思想。
            dp[i][j] = dp[dp[i][j-1]][j-1];
        }
    }
    //每个询问
    for(int i=1; i<=m; i++) {
        cin >> x >> y;
        int ans = 0;
        for(int i=17; i>=0; i--) {//可以证明，下一个位置能够跳的次数一定小于之前跳的次数。
            if(dp[x][i] <= y) {
                ans += (1<<i);
                x = dp[x][i];
            }
        }
        cout << ans+1 << endl;
    }
    return 0;
}
```









## [C. Chef Monocarp](https://codeforces.com/contest/1437/problem/C)

问题：给出$n$道菜，每道菜有一个预期出锅时间$t_i$，但每分钟只能出一道菜，设第$i$道菜的出锅时间为$T_i$，则会得到$|t_i-T_i|$的不愉悦值，问怎么安排出菜时间，才能使不愉悦值最小，输出最小值。

给出一个引理：
$$
t_i<t_j且T_i>T_j\\
\Rightarrow\mid t_i-T_i\mid+\mid t_j-T_j\mid>\mid t_i-T_j\mid+\mid t_j-T_i\mid\\
$$
说明:$T_i$最多到$400$.

因为:$t_i$最多到$200$,$n$最大也是$200$.感性理解一下,最大就是400.

先对每道菜的期望时间排个序。

对第在第$i$个时间点选择第$j$道菜,则第$j+1$道菜只能在时间$i$后出菜,这样就保证了,无后效性.

然后就确定$dp$状态:

$dp[i][j]$表示第$i$个时间选择前$j$个菜的最小时间.

转移方程:

$dp[i][j]=min(dp[i][j],dp[i-1][j-1]+abs(t_i-i))$



```cpp
#include<bits/stdc++.h>
using namespace std;
#define int long long
typedef long long ll;
const int N = 4e2+10;
const double eps = 1e-13;
const int inf = 0x7fffffffffffff;
int s[N], dp[N][N];
signed main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif  
    int t;
    cin >> t;
    while(t--) {
        int n;
        cin >> n;
        for(int i=1; i<=n; i++) {
            cin >> s[i];
        }
        sort(s+1, s+1+n);
        memset(dp, 0x7f, sizeof dp);
        for(int i=0; i<=n; i++) {
            dp[i][0] = 0;
        }
        for(int i=1; i<=400; i++) {
            for(int j=1; j<=n; j++) {
                dp[i][j] = min(dp[i-1][j], dp[i-1][j-1] + abs(s[j]-i));
            }
        }
        cout << dp[400][n] << endl;
    }
    return 0;
}
```















# [D. Dice](https://codeforces.com/gym/102700/problem/D)

题意：

给定$n$个色子，每个色子有$k$个面，每个面都有一个数字，数字模$m$为$0$的面出现的概率为$0$，剩下的面出现的的概率相等，每个面在模$m$下，只有$m$中情况，我们可以算出一个色子的$m$个概率。

思路：

设$a_i$表示$1$个色子丢出的面模$m$是$i$的概率。

设$b_i$表示$1$个色子丢出的面模$m$是$i$的概率。

则：
$$
\begin{align*}
&b_0=a_0*a_0+a_1*a_{m-1}+a_2*a_{m-2}...a_{m-1}*a_1\\
&b_1=a_0*a_1+a_1*a_0+a_2*a_{m-1}...a_{m-1}*a_2\\
&b_2=a_0*a_2+a_1*a_1+a_2*a_0...a_{m-1}*a_3\\
&...\\
&b_{m-1}=a_0*a_{m-1}+a_1*a_{m-2}+a_2*a_{m-3}...a_{m-1}*a_0
\end{align*}
$$
可以写出矩阵的形式
$$
[b_0,b_1...b_{m-1}]=[a_0,a_1...a_{m-1}]\times
\left[ \begin{array}{l}
a_0&a_{m-1}&\cdots&a_1\\
a_1&a_0&\cdots&a_2\\
\vdots&\vdots&\ddots&\vdots\\
a_{m-1}&a_{m-2}&\cdots&a_0
\end{array}\right]^T
$$
可以看出我们要用矩阵快速幂来进行优化加速。



Code：

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll mod = 1000000007;
const ll N = 210;

ll n, k, m;

ll qpow(ll x, ll y) {
    ll ans = 1;
    while(y) {
        if(y & 1) ans = ans * x % mod;
        x = x * x % mod;
        y >>= 1;
    }
    return ans;
}

struct matrix {
    ll s[N][N];
    matrix operator * (const matrix & a) const {
        matrix tmp;
        for(int i=0; i<m; i++) {
            for(int j=0; j<m; j++) {
                tmp.s[i][j] = 0;
                for(int k=0; k<m; k++){
                    tmp.s[i][j] = (tmp.s[i][j] + s[i][k] * a.s[k][j] % mod) % mod;
                }
            }
        }
        return tmp;
    }
} A, B, ans, I;



matrix mqpow(matrix a, ll y) {
    ans = I;
    while(y) {
        if(y & 1) ans = ans * a;
        a = a * a;
        y >>= 1;
    }
    return ans;
}


int val[N];

int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    cin >> n >> k >> m;
    
    ll aa = qpow(1ll*k-k/m, mod-2);
    for(int i=1; i<=k; i++) {
        if(i % m == 0) continue;
        else val[i % m] = (val[i % m] + aa) % mod;
    }
    for(int i=0; i<m; i++) I.s[i][i] = 1, B.s[0][i] = val[i];
    int u = 0;
    for(int i=0; i<m; i++) {
        for(int j=0; j<m; j++) {
            A.s[i][j] = val[(j+u) % m];
        }
        u = (u - 1 + m) % m;
    }  
    
    A = mqpow(A, (n-1));
    ans = B * A;
    cout << ans.s[0][0] << endl;
    
    return 0;
}

```











# [A. Approach](https://codeforces.com/gym/102700/problem/A)

题意：

给出$4$个点，$A，B，C，D$，两个人$a,b$，a从$A$到$B$，b从$C$到$D$，他们两个速度相同，问他们相距最近的距离大小（注：当某一个人到达终点时，他会停留在终点，而另一个人仍然会继续前进）



思路：

猜测：距离的他们两点的距离是一个关于时间的凹函数，然后就是三分去查找。

注意细节：当一个人已经到达终点时，可以肯定当前距离函数仍然是一个凹函数或者一个单调递增的函数，所以我们要再用一次三分，来找到当前段的最小值，再和上一段进行比较。

！！！：特判a的起点和终点在同一个点，b的起点和终点在同一个点。

（精度很难确定）

Code



```cpp
#include<bits/stdc++.h>
using namespace std;
double len1, len2;
const double eps = 5e-7;
double ax, ay, bx, by, cx, cy, dx, dy;
double dis(double a, double b, double c, double d) {
    return sqrt((a-c)*(a-c)+(b-d)*(b-d));
}
double f(double t) {
    double u = ax + t*(bx-ax)/len1, v = ay + t*(by-ay)/len1;
    double g = cx + t*(dx-cx)/len2, h = cy + t*(dy-cy)/len2;
    return dis(u, v, g, h);
}

double ff(double t) {
    double u = bx, v = by;
    double g = cx + t*(dx-cx)/len2, h = cy + t*(dy-cy)/len2;
    return dis(u, v, g, h);
}
int main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif
    scanf("%lf%lf%lf%lf%lf%lf%lf%lf", &ax, &ay, &bx, &by, &cx, &cy, &dx, &dy);
    len1 = dis(ax, ay, bx, by);
    len2 = dis(cx, cy, dx, dy);
    // cout << len1 << ' ' << len2 << endl;
    if(len1 == 0 && len2 == 0) {
        printf("%.12lf", dis(ax, ay, cx, cy));
        return 0;
    }
    double l = 0, r = min(len1, len2), lmid, rmid, ans = 1e18;
    while(len1 && len2 && r-l > eps) {
        lmid = l+(r-l)/3;
        rmid = r-(r-l)/3;
        if(f(lmid) <= f(rmid)) r = rmid;
        else l = lmid;
    }
    if(len1 && len2) ans = f(l);
    if(len1 > len2) {
        swap(len1, len2);
        swap(ax, cx);
        swap(bx, dx);
        swap(ay, cy);
        swap(by, dy);
    }
    l = len1, r = len2;
    while(r - l > eps) {
        lmid = l + (r-l)/3;
        rmid = r - (r-l)/3;
        if(ff(lmid) <= ff(rmid)) r = rmid;
        else l = lmid;
    }
    ans = min(ans, ff(l));
    printf("%.12lf", ans);
    return 0;
}
```







[D. Kavi on Pairing Duty](https://codeforces.com/contest/1529/problem/D)
题意：在$x$轴上有$2n$个点，$x_i=i$，现在两两点连线形成线段（点不能重复使用），要满足任意两两线段至少满足下面的一个条件，问不同的线段组合有多少符合题意。

条件是：

1. A线段和B线段长度相等
2. A线段包含在B线段中。

下图中A对应2，B对应1，C不满足题意。

![img](https://espresso.codeforces.com/b1a1f1ddeec9def8c456b35697a922fbfc2e593a.png)

思路：我们枚举最外面有多少个大线段，能够包含剩下的线段，我们就把范围缩小得到的结果


```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int N = 1e6+10;
const int inf = 2e9+7;
const int mod = 998244353;
typedef long long ll;
int p[N], f[N], tot, ans[N], cnt[N];
bool st[N];
void init() {
    cnt[1] = f[1] = 1;
    for(int i=2; i<N; i++) {
        if(!st[i]) cnt[i] = 1, p[tot++] = i, f[i] = 2;
        for(int j=0; j<tot&&i*p[j]<N; j++) {
            st[i*p[j]] = 1;
            if(i % p[j] == 0) {
                cnt[i*p[j]] = cnt[i] + 1;
                f[i*p[j]] = f[i] / (cnt[i] + 1) * (cnt[i*p[j]] + 1);
                break;
            }
            f[i*p[j]] = f[i] * f[p[j]];
            cnt[i*p[j]] = 1;
        }
    }
}
signed main() {
#ifndef ONLINE_JUDGE
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
    int StartTime = clock();
#endif
    ios::sync_with_stdio(false);
    init();
    int n;
    cin >> n;
    ans[1] = 1;
    int sum = 1;
    for(int i=2; i<=n; i++) {
        ans[i] = (sum + f[i]) % mod;
        sum = (sum + ans[i]) % mod;
    }
    cout << ans[n]%mod << endl;
#ifndef ONLINE_JUDGE
    printf("Run_Time = %d ms\n", clock() - StartTime);
#endif    
    return 0;
}
```













# C. Need for Pink Slips

问题：一个游戏，给出$C,M,P$三个事件，每个事件的概率是$c，m，p$，当出现事件p的时候，游戏结束。在每一轮游戏过程中$c，m，p$都会根据规则发生变化。问游戏进行多少轮的期望。

规则：给定一个小数$v$。

- $(c,m,p)=(0.2,0.1,0.7)$， $v=0.1$， 在发生事件C后，因为$c>v$，所以$c=c-v，m=v/2+m, p = v/2+p$，$(c,m,p)=(0.1,0.15,0.75)$.

- $(c,m,p)=(0.2,0.1,0.7)$， $v=0.2$， 在发生事件C后，因为$c\le v$，所以$c=0，m=c/2+m, p = c/2+p$，$(c,m,p)=(0.1,0.15,0.75)$.
- $(c,m,p)=(0.2,0,0.8)$， $v=0.1$， 在发生事件C后，因为$c>v$，但是$m=0$，所以$c=c-v，m=, p = v+p$，$(c,m,p)=(0.1,0,0.9)$
- $(c,m,p)=(0.1,0,0.9)$，$v=0.2$， 在发生事件C后，因为$c\le v$，但是$m=0$，所以$c=0，m=0, p = c+p$，$(c,m,p)=(0,0,1)$

解答：期望=轮数*概率，根据规则，$dfs$每一种情况，都加起来就是答案，注意设置esp。

```cpp
#include <bits/stdc++.h>

using namespace std;
// #define double long double
typedef long long ll;
const int N = 1e4+10;
const int mod = 1e9+7;
const double esp = 1e-12;
double s[10], v, ans = 0;
double i = 1;
//s数组存放c,m,p。ans是答案。
void dfs(double val) {
    //val表示出现当前情况的概率。
    if(val == 0 || val*s[3] < esp) return;
    else {
        //val*s[3]表示下一轮就是事件p发生，能对期望产生val*s[3]*i的贡献。
        ans += val*s[3]*i;
    }
    i += 1;
    double a = s[1], b = s[2], c = s[3];
    double tmp = val*a;
    if(a > 0) {
        //根据规则来计算下一轮c,m,p的值。
        if(a <= v) {
            s[1] = 0;
            if(s[2] < esp) s[3] = a + s[3];
            else s[2] = a/2 + s[2], s[3] = a/2 + s[3];
            
            dfs(val*a);
            s[1] = a, s[2] = b, s[3] = c;//回溯。
        }
        else {
            s[1] = a-v;
            if(s[2] < esp) s[3] += v;
            else s[2] = v/2 + s[2], s[3] = v/2 + s[3];
            dfs(val*a);
            s[1] = a, s[2] = b, s[3] = c;
        }
    }
    if(b > 0) {
        tmp = val * b;
        if(b <= v) {
            s[2] = 0;
            if(s[1] < esp) s[3] += b;
            else s[1] = b/2+s[1] , s[3] = b/2 + s[3];
            dfs(val*b);
            s[1] = a, s[2] = b, s[3] = c;
        }
        else {
            s[2] = b-v;
            if(s[1] < esp) s[3] += v;
            else s[1] = v/2+s[1], s[3] = v/2 + s[3];
            dfs(val*b);
            s[1] = a, s[2] = b, s[3] = c;
        }
    }
    i -= 1;
}
int main() {
#ifndef ONLINE_JUDGE 
    freopen("in.txt", "r", stdin);
    freopen("out.txt", "w", stdout);
#endif 
    int t;
    cin >> t;
    while(t--) {
        ans = 0, i = 1;
        for(int i=1; i<=3; i++) cin >> s[i];
        cin >> v;
        dfs(1);
        printf("%.12lf\n", ans);
   }
}   

```

