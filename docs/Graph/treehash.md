```cpp
int T,n,siz[101],power[101],g[101],dep[101],head[101],to[101],nex[101],cnt,root1,root2;
pii f[101];
void dfs(int k,int fa)
{
    siz[k]=1;int maxn=0;
    for(int i=head[k];i;i=nex[i])if(to[i]!=fa)dfs(to[i],k),maxn=max(maxn,siz[to[i]]),siz[k]+=siz[to[i]];
    if((n-siz[k])<=n/2&&maxn<=n/2)root1?root2=k:root1=k;
}
void dfs2(int k,int fa)
{
    dep[k]=dep[fa]+1,g[k]=power[1]*dep[k],siz[k]=1;pii a[51];int tot=0;
    for(int i=head[k];i;i=nex[i])if(to[i]!=fa)dfs2(to[i],k),siz[k]+=siz[to[i]],a[++tot]=mp(g[to[i]],to[i]);
    sort(a+1,a+1+tot);int now=1;
    for(int i=1;i<=tot;++i)g[k]+=g[a[i].se]*power[now],now+=siz[a[i].se];
}
inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
pii calc()
{
    power[0]=1;int x;pii p;
    for(int i=1;i<=50;++i)power[i]=power[i-1]*13331;
    root1=root2=0,dfs(tmp,0),dfs2(root1,0),p.fi=g[root1];
    root2?root1=root2:0,dfs2(root1,0),p.se=g[root1];
    if(p.fi>p.se)swap(p.fi,p.se);
    return p;
}
```