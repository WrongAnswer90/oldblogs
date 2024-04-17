筛的是 $f(p^k)=p^k(p^k-1)$。

```cpp
ll n,w[300010],g1[300010],g2[300010],sum1[300010],sum2[300010],pr[300010];
int N,m,cnt,id1[300010],id2[300010],iv6;
bool v[300010];
inline void preSieve()
{
    for(int i=2;i<=N;++i)
    {
        if(!v[i])pr[++cnt]=i;
        for(int j=1;j<=cnt&&pr[j]*i<=N;++j)
        {v[pr[j]*i]=1;if(i%pr[j]==0)break;}
    }
    for(int i=1;i<=cnt;++i)
    {
        sum1[i]=Cadd(sum1[i-1],pr[i]);
        sum2[i]=(sum2[i-1]+1ll*pr[i]*pr[i])%MOD;
    }
}
inline int f1(ll x){x%=MOD;return x*(x+1)/2%MOD;}
inline int f2(ll x){x%=MOD;return x*(x+1)%MOD*(2*x+1)%MOD*iv6%MOD;}
inline int getid(ll x){return x<=N?id1[x]:id2[n/x];}
ll S(ll x,int j)
{
    if(pr[j]>x)return 0;
    ll ans=Cdel(Cdel(g2[getid(x)],g1[getid(x)]),Cdel(sum2[j],sum1[j]));
    for(int i=j+1;i<=cnt&&pr[i]*pr[j]<=x;++i)
    {
        for(ll e=1,sp=pr[i];sp<=x;sp*=pr[i],++e)
        ans=(ans+sp%MOD*(sp%MOD-1)%MOD*(S(x/sp,i)+(e>1)))%MOD;
    }
    return ans;
}
inline ll sieve()
{
    N=sqrt(n),preSieve(),iv6=power(6,MOD-2);
    for(ll l=1,r;l<=n;l=r+1)
    {
        r=n/(n/l),w[++m]=n/l;
        g1[m]=f1(w[m])-1,g2[m]=f2(w[m])-1;
        if(w[m]<=N)id1[w[m]]=m;else id2[n/w[m]]=m;
    }
    for(int i=1;i<=cnt;++i)
    {
        for(int j=1;j<=m&&pr[i]*pr[i]<=w[j];++j)
        {
            g1[j]=((g1[j]-1ll*pr[i]*(g1[getid(w[j]/pr[i])]-sum1[i-1]))%MOD+MOD)%MOD;
            g2[j]=((g2[j]-1ll*pr[i]*pr[i]%MOD*(g2[getid(w[j]/pr[i])]-sum2[i-1]))%MOD+MOD)%MOD;
        }
    }
    return S(n,0)+1;
}
```