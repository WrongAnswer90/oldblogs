```cpp
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
```