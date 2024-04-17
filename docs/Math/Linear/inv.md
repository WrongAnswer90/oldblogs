```cpp
int n,f[410][810];
inline int INV()
{
    for(int i=1;i<=n;++i)f[i][i+n]=1;
    for(int i=1;i<=n;++i)
    {
        int pos=0;
        for(int j=i;j<=n;++j)if(f[j][i]){pos=j;break;}
        if(!pos)return 0;
        for(int j=1;j<=n*2;++j)swap(f[i][j],f[pos][j]);
        int tmp=power(f[i][i],MOD-2);
        for(int j=1;j<=n;++j)
        {
            if(j==i)continue;
            int rate=Cmul(f[j][i],tmp);
            for(int k=1;k<=n*2;++k)Mdel(f[j][k],Cmul(f[i][k],rate));
        }
    }
    for(int i=1;i<=n;++i)
    {
        int tmp=power(f[i][i],MOD-2);
        for(int j=n+1;j<=n*2;++j)Mmul(f[i][j],tmp);
    }
    return 1;
}
```