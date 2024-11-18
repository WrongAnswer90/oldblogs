```cpp
	int all,minn,root,siz[100010];
	bool vis[100010];
	vi T[100010];
	void findrt(int x,int fa=0)
	{
		siz[x]=1;
		int maxn=0;
		for(auto to:T[x])if(to!=fa)
		{
			findrt(to,x),siz[x]+=siz[to];
			Mmax(maxn,siz[to]);
		}
		if(Mmin(minn,max(maxn,all-siz[x])))
		root=x;
	}
	void starch(int x)
	{
		minn=inf,findrt(x),findrt(x=root),vis[x]=1;
		//...
		for(auto to:T[x])if(!vis[to])
		all=siz[to],starch(to);
	}
```