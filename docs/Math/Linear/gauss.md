```cpp
int n,flag1=1,flag2=1,f[501][501];
inline int Gauss()
{
    int now=1;
    for(int i=1,pos=0;i<=n;++i,pos=0)
    {
        for(int j=now;j<=n;++j)if(f[j][i])pos=j;
        if(!pos){flag1=0;continue;}
        for(int j=0;j<=n;++j)swap(f[pos][j],f[now][j]);
        for(int j=1;j<=n;++j)
        {
            if(j==now)continue;
            int rate=Cmul(f[j][i],power(f[now][i],MOD-2));
            for(int k=0;k<=n;++k)Mdel(f[j][k],Cmul(f[now][k],rate));
        }
        ++now;
    }
    if(now<=n)
    {
        while(now<=n)if(f[now++][0]!=0)return -1;
        return 0;
    }
    return 1;
}
```