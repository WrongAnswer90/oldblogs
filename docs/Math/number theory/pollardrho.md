```cpp
mt19937_64 rnd(time(0)^(ull)(new char));
#define rd(l,r) ((l)+rnd()%((int)((r)-(l)+1)))
inline int mul(int x,int y,int z)
{
    int a=x*((ld)y/z),b=x*y-a*z;
    if(b<0)b+=z;
    if(b>=z)b-=z;
    return b;
}
inline int add(int x,int y,int z){return x+y>=z?x+y-z:x+y;}
inline int power(int x,int y,int z)
{
    int s=1;
    for(;y;y>>=1,x=mul(x,x,z))if(y&1)s=mul(s,x,z);
    return s;
}
inline bool chk(int n)
{
    if(n<3||n%2==0)return n==2;
    int u=n-1,t=0;
    while(u%2==0)u/=2,++t;
    for(int i=0,s,a,v;i<8;++i)
    {
        a=rd(2,n-1),v=power(a,u,n);
        if(v==1)continue;
        for(s=0;s<t;++s)
        {
            if(v==n-1)break;
            v=mul(v,v,n);
        }
        if(s==t)return 0;
    }
    return 1;
}
#define nx(a) add(mul(a,a,n),c,n)
inline int gcd(int a,int b)
{
    int az=__builtin_ctzll(a),bz=__builtin_ctzll(b);
    int z=min(az,bz),tmp;b>>=bz;
    while(a)a>>=az,tmp=a-b,az=__builtin_ctzll(tmp),b=min(a,b),a=abs(tmp);
    return b<<z;
}
int calc(int n)
{
    if(n==4)return 2;
    int x=rd(3,n-1),y=x,c=rd(3,n-1),d=1;
    x=nx(x),y=nx(nx(y));
    for(int lim=1;x!=y;lim=min(128ll,lim<<1))
    {
        int cnt=1;
        for(int i=0;i<lim;++i)
        {
            int tmp=mul(cnt,abs(x-y),n);
            if(!tmp)break;
            cnt=tmp,x=nx(x),y=nx(nx(y));
        }
        d=gcd(cnt,n);
        if(d>1)return d;
    }
    return n;
}
vi ve;
void dfs(int n)
{
    if(n==1)return;
    int d=calc(n),d2;
    while(d==n)d=calc(n);
    d2=n/d;
    if(chk(d))ve.eb(d);else dfs(d);
    if(chk(d2))ve.eb(d2);else dfs(d2);
}
```