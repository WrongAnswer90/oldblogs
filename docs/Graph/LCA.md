```cpp
    int n,m,rt,tot,cnt,dfn[500010],head[500010],to[1000010],nex[1000010];
    inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
    int F[19][500010];
    inline int get(int x,int y){return dfn[x]<dfn[y]?x:y;}
    void dfs(int x,int fa)
    {
        F[0][dfn[x]=++tot]=fa;
        for(int i=head[x];i;i=nex[i])if(to[i]!=fa)dfs(to[i],x);
    }
    inline int LCA(int x,int y)
    {
        if(x==y)return x;
        if((x=dfn[x])>(y=dfn[y]))swap(x,y);
        int k=__lg(y-x++);
        return get(F[k][x],F[k][y-(1<<k)+1]);
    }
    inline void build()
    {
        dfs(rt,0);
        for(int i=1;i<19;++i)
        {
            for(int j=1;j+(1<<i)-1<=n;++j)
            F[i][j]=get(F[i-1][j],F[i-1][j+(1<<(i-1))]);
        }
    }
```