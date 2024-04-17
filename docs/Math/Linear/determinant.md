## 普通高消

```cpp
int n,p,f[610][610];
inline void del(int x,int y,int z){for(int i=1;i<=n;++i)f[x][i]=(f[x][i]-1ll*z*f[y][i]%p+p)%p;}
inline void swp(int x,int y){swap(f[x],f[y]);}
inline int solve()
{
    int ans=1;
    for(int i=1;i<=n;++i)
    {
        int pos=0;
        for(int j=i;j<=n;++j)if(f[j][i]){pos=j;break;}
        if(!pos)return 0;
        if(pos!=i)ans=Cdel(0,ans);
        for(int j=1;j<=n;++j)swap(f[pos][j],f[i][j]);
        for(int j=i;j<=n;++j)
        {
            int v=Cmul(f[j][i],power(f[i][i],MOD-2));
            for(int k=i;k<=n;++k)Mdel(f[j][k],Cmul(f[i][k],v));
        }
        Mmul(ans,f[i][i]);
    }
    return ans;
}
```

## 辗转相除法

```cpp
int n,p,f[610][610];
inline void del(int x,int y,int z){for(int i=1;i<=n;++i)f[x][i]=(f[x][i]-1ll*z*f[y][i]%p+p)%p;}
inline void swp(int x,int y){swap(f[x],f[y]);}
inline int solve()
{
    int ans=1;
    for(int i=n;i>=1;--i)
    {
        for(int j=1;j<i;++j)
        {
            while(f[j][i]&&f[i][i])
            {
                if(f[j][i]>f[i][i])del(j,i,f[j][i]/f[i][i]);
                else del(i,j,f[i][i]/f[j][i]);
            }
            if(f[j][i])ans=p-ans,swp(i,j);
        }
        ans=1ll*ans*f[i][i]%p;
    }
    return ans;
}
```