```cpp
int tot,siz[N+10],dep[N+10],son[N+10];
int top[N+10],dfn[N+10],id[N+10],fa[N+10];
void dfs0(int x,int Fa=0)
{
	fa[x]=Fa;
	siz[x]=1;
	dep[x]=dep[Fa]+1;
	for(auto to:T[x])if(to!=Fa)
	{
		dfs0(to,x),siz[x]+=siz[to];
		if(siz[to]>siz[son[x]])son[x]=to;
	}
}
void dfs1(int x,int Top=1)
{
	top[x]=Top;
	dfn[x]=++tot;
	id[tot]=x;
	if(son[x])dfs1(son[x],Top);
	for(auto to:T[x])
	{
		if(to!=fa[x]&&to!=son[x])
		dfs1(to,to);
	}
}
```