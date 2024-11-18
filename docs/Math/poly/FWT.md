```cpp
    inline void AND(vector<int>&ve,int opt=1)
    {
        int n=ve.size();
        if(opt==1)
        for(int k=2;k<=n;k<<=1)
        {
            for(int i=0;i<n;i+=k)
            {
                for(int j=i;j<i+(k>>1);++j)
                Madd(ve[j],ve[j+(k>>1)]);
            }
        }
        else
        for(int k=2;k<=n;k<<=1)
        {
            for(int i=0;i<n;i+=k)
            {
                for(int j=i;j<i+(k>>1);++j)
                Mdel(ve[j],ve[j+(k>>1)]);
            }
        }
    }
    inline void OR(vector<int>&ve,int opt=1)
    {
        int n=ve.size();
        if(opt==1)
        for(int k=2;k<=n;k<<=1)
        {
            for(int i=0;i<n;i+=k)
            {
                for(int j=i;j<i+(k>>1);++j)
                Madd(ve[j+(k>>1)],ve[j]);
            }
        }
        else
        for(int k=2;k<=n;k<<=1)
        {
            for(int i=0;i<n;i+=k)
            {
                for(int j=i;j<i+(k>>1);++j)
                Mdel(ve[j+(k>>1)],ve[j]);
            }
        }
    }
    inline void XOR(vector<int>&ve,int opt=1)
    {
        int n=ve.size();
        for(int k=2;k<=n;k<<=1)
        {
            for(int i=0;i<n;i+=k)
            {
                for(int j=i;j<i+(k>>1);++j)
                {
                    Madd(ve[j],ve[j+(k>>1)]);
                    ve[j+(k>>1)]=Cdel(ve[j],ve[j+(k>>1)],ve[j+(k>>1)]);
                }
            }
        }
        int tmp=power(inv2,__lg(n));
        if(opt==-1)for(int i=0;i<n;++i)Mmul(ve[i],tmp);
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