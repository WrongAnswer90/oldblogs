```cpp
    int n,d[1000010],f[1000010],p[1000010];
    inline void change()
    {
        for(int i=1;i<n;++i)++d[f[i]];
        for(int i=1,j=1;i<=n-2;++i,++j)
        {
            while(d[j])++j;p[i]=f[j];
            while(i<=n-2&&!--d[p[i]]&&p[i]<j)p[i+1]=f[p[i]],++i;
        }
    }
    inline void recover()
    {
        for(int i=1;i<=n-2;++i)++d[p[i]];p[n-1]=n;
        for(int i=1,j=1;i<n;++i,++j)
        {
            while(d[j])++j;f[j]=p[i];
            while(i<n&&!--d[p[i]]&&p[i]<j)f[p[i]]=p[i+1],++i;
        }
    }
```