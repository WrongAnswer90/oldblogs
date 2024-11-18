求多项式系数表示法：

```cpp
    int n,m,ans,x[2010],y[2010],d[2010],a[2010],b[2010];
    inline void mian()
    {
        read(n,m),a[0]=1;
        for(int i=1;i<=n;++i)
        {
            read(x[i],y[i]);
            for(int j=i;j>=1;--j)a[j]=Cdel(a[j-1],Cmul(x[i],a[j]));
            Mmul(a[0],MOD-x[i]);
        }
        for(int i=1,v;i<=n;++i)
        {
            int c=power(MOD-x[i],MOD-2);v=1,b[0]=Cmul(a[0],c);
            for(int j=1;j<n;++j)b[j]=Cmul(Cdel(a[j],b[j-1]),c);
            for(int j=1;j<=n;++j)if(i!=j)Mmul(v,Cdel(x[i],x[j]));
            v=Cmul(y[i],power(v,MOD-2));
            for(int j=0;j<n;++j)Madd(d[j],Cmul(v,b[j]));
        }
        for(int j=0;j<n;++j)Madd(ans,Cmul(power(m,j),d[j]));
        write(ans);
    }
```

求点值：

```cpp
    int n,k,ans,x[2001],y[2001];
    inline void mian()
    {
        read(n,k);
        for(int i=1;i<=n;++i)read(x[i],y[i]);
        for(int i=1;i<=n;++i)
        {
            int up=y[i],down=1;
            for(int j=1;j<=n;++j)
            {
                if(i==j)continue;
                Mmul(up,k-x[j]),Mmul(down,x[i]-x[j]);
            }
            Madd(ans,Cmul(up,power(down,MOD-2)));
        }
        write(ans);
    }
```