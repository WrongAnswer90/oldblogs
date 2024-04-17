注意先调用 `sieve()`。

```cpp
int T,n,d,m,cnt,pr[100010];
bitset<2000010> v;
vector<int> ve,ans;
int gcd(int x,int y){return y?gcd(y,x%y):x;}
inline int power(int x,int y,int z)
{
    int s=1;
    for(;y;y>>=1,x=1ll*x*x%z)if(y&1)s=1ll*s*x%z;
    return s;
}
inline int phi(int x)
{
    int y=x;
    for(int i=2;i*i<=x;++i)
    {
        if(x%i==0)
        {
            y=y/i*(i-1);
            while(x%i==0)x/=i;
        }
    }
    if(x!=1)y=y/x*(x-1);
    return y;
}
inline void sieve()
{
    for(int i=2;i<=1000000;++i)
    {
        if(!v[i])pr[++cnt]=i;
        for(int j=1;j<=cnt&&i*pr[j]<=1000000;++j)
        {
            v[i*pr[j]]=1;
            if(i%pr[j]==0)break;
        }
    }
    v.reset(),v[2]=v[4]=1;
    for(int i=2;i<=cnt;++i)
    {
        for(ll j=pr[i];j<=1000000;j*=pr[i])
        v[j]=1,v[2*j]=1;
    }
}
inline vi calc(int n)
{
    read(n,d),m=phi(n),ans.clear(),ve.clear();int mm=m,g=1;
    if(!v[n])return ans;
    for(int i=2;i*i<=m;++i)if(m%i==0){ve.eb(i);while(m%i==0)m/=i;}
    if(m!=1)ve.eb(m);
    for(;g<n;++g)
    {
        if(gcd(g,n)!=1)continue;
        int fl=1;
        for(auto p:ve)if(power(g,mm/p,n)==1){fl=0;break;}
        if(fl)break;
    }
    for(int i=1,nw=g;i<=mm;++i,nw=1ll*nw*g%n)
    if(gcd(i,mm)==1)ans.eb(nw);
    return ans;
}
```