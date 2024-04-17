## BSGS

```cpp
inline int power(int a,int b,int p)
{
    int ans=1;
    for(;b;a=a*a%p,b>>=1)if(b&1)ans=ans*a%p;
    return ans;
}
inline int BSGS(int a,int b,int p)
{
    int t=sqrt(p)+1;unordered_map<int,int> mp;mp.clear(),b%=p;
    for(int j=0,now=b;j<t;now=now*a%p,++j)mp[now]=j;
    a=power(a,t,p);
    if(!a)return b?-1:1;
    for(int i=0,now=1;i<=t;now=now*a%p,++i)
    {
        int j=mp.find(now)==mp.end()?-1:mp[now];
        if(j>=0&&i*t-j>=0)return i*t-j;
    }
    return -1;
}
```

## exBSGS

```cpp
inline int power(int a,int b,int p)
{
    int ans=1;
    for(;b;a=a*a%p,b>>=1)if(b&1)ans=ans*a%p;
    return ans;
}
inline int BSGS(int a,int b,int p)
{
    int t=sqrt(p)+1;unordered_map<int,int> mp;mp.clear(),b%=p;
    for(int j=0,now=b;j<t;now=now*a%p,++j)mp[now]=j;
    a=power(a,t,p);
    if(!a)return b?-1:1;
    for(int i=0,now=1;i<=t;now=now*a%p,++i)
    {
        int j=mp.find(now)==mp.end()?-1:mp[now];
        if(j>=0&&i*t-j>=0)return i*t-j;
    }
    return -1;
}
void exgcd(int a,int b,int &x,int &y)
{
    if(!b)return x=1,y=0,void();
    exgcd(b,a%b,x,y);int z=x;
    x=y,y=z-x*(a/b);
}
int gcd(int a,int b){return b?gcd(b,a%b):a;}
inline int inv(int b,int p){int x,y;exgcd(b,p,x,y),x=(x%p+p)%p;return x;}
inline int exBSGS(int a,int b,int p)
{
    b%=p;
    if(b==1||p==1)return 0;
    int na=1,k=0,g=gcd(a,p);
    while(g>1)
    {
        if(b%g!=0)return -1;
        ++k,b/=g,p/=g,na=na*(a/g)%p,g=gcd(a,p);
        if(na==b)return k;
    }
    int f=BSGS(a,b*inv(na,p)%p,p);
    if(f==-1)return -1;
    return f+k;
}
```