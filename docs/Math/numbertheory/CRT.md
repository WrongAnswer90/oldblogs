```cpp
int exgcd(int a,int b,int &x,int &y)
{
    if(!b)return x=1,y=0,a;
    int d=exgcd(b,a%b,x,y);
    int z=x;x=y,y=z-x*(a/b);
    return d;
}
int gcd(int a,int b){return b?gcd(b,a%b):a;}
int m[100010],a[100010],n,M=1,x0=0,MM,b,k,x,y;
inline int calc()
{
    for(int i=1;i<=n;++i)
    {
        b=a[i]-x0;
        int d=exgcd(M,m[i],k,y);
        if(b%d!=0){write(114514);return;}
        k=(k*b/d%(m[i]/d)+m[i]/d)%(m[i]/d),MM=M/gcd(M,m[i])*m[i];
        x0=((x0+k*M)%MM+MM)%MM,M=MM;
    }
    return x0;
}
```