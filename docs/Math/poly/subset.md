```cpp
inline void OR(vi& A,int opt=1)
{
    int n=A.size();
    if(opt==1)
    for(int i=2;i<=n;i<<=1)
    {
        for(int j=0;j<n;j+=i)
        {
            for(int k=j;k<j+(i>>1);++k)
            Madd(A[k+(i>>1)],A[k]);
        }
    }
    else
    for(int i=2;i<=n;i<<=1)
    {
        for(int j=0;j<n;j+=i)
        {
            for(int k=j;k<j+(i>>1);++k)
            Mdel(A[k+(i>>1)],A[k]);
        }
    }
}
inline vi MUL(vi a,vi b)
{
    int N=a.size(),n=__lg(N);static vi c,A[21],B[21],C[21];
    c.resize(N);
    for(int i=0;i<=n;++i)
    {
        A[i].clear(),B[i].clear();
        A[i].resize(N),B[i].resize(N),C[i].resize(N);
    }
    for(int i=0;i<N;++i)A[__builtin_popcount(i)][i]=a[i];
    for(int i=0;i<N;++i)B[__builtin_popcount(i)][i]=b[i];
    for(int i=0;i<=n;++i)OR(A[i]),OR(B[i]);
    for(int i=0;i<=n;++i)
    {
        for(int j=0;j+i<=n;++j)
        {
            for(int k=0;k<N;++k)
            Madd(C[i+j][k],Cmul(A[i][k],B[j][k]));
        }
    }
    for(int i=0;i<=n;++i)OR(C[i],-1);
    for(int i=0;i<N;++i)c[i]=C[__builtin_popcount(i)][i];
    return c;
}
```