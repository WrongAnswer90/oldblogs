```cpp
    int cnt,n,pr[2000010];
    ui a[20000010],ans;
    bitset<20000010> v;
    inline void init()
    {
        for(int i=2;i<=20000000;++i)
        {
            if(!v[i])pr[++cnt]=i;
            for(int j=1;j<=cnt&&i*pr[j]<=20000000;++j)
            {
                v[i*pr[j]]=1;
                if(i%pr[j]==0)break;
            }
        }
    }
    inline void dirichlet()
    {
        for(int i=1;i<=cnt;++i)
        {
            for(int j=1;j*pr[i]<=20000000;++j)
            Madd(a[j*pr[i]],a[j]);
        }
    }
    inline void modius()
    {
        for(int i=1;i<=n;++i)
        {
            for(int j=i+i;j<=n;j+=i)
            Mdel(a[j],a[i]);
        }
    }
```