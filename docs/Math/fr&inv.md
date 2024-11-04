```cpp
const int N=1000000;
int fr[N+10],inv[N+10];
inline int C(int n,int m){return Cmul(fr[n],inv[m],inv[n-m]);}
inline void init(int n=N)
{
    fr[0]=inv[0]=1;
    for(int i=1;i<=n;++i)fr[i]=Cmul(fr[i-1],i);
    inv[n]=power(fr[n],MOD-2);
    for(int i=n-1;i>0;--i)inv[i]=Cmul(inv[i+1],i+1);
}
```

