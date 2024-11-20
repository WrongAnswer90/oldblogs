```cpp
	struct _HLP
	{
		int tot,*siz,*dep,*son;
	    int *top,*dfn,*id,*fa;
	    void init(vi*T,int n,int rt=1)
		{
			siz=new int[n+1],dep=new int[n+1],son=new int[n+1],top=new int[n+1];
			dfn=new int[n+1],id=new int[n+1],fa=new int[n+1];
			tot=dep[0]=id[0]=dfn[0]=fa[0]=siz[0]=top[0]=0;
		    auto dfs0=[&](auto dfs0,int x,int Fa=0)->void
		    {
		        fa[x]=Fa,siz[x]=1;
		        dep[x]=dep[Fa]+1;
		        for(auto to:T[x])if(to!=Fa)
		        {
		            dfs0(dfs0,to,x),siz[x]+=siz[to];
		            if(siz[to]>siz[son[x]])son[x]=to;
		        }
		    };
		    auto dfs1=[&](auto dfs1,int x,int Top=1)->void
		    {
		        top[x]=Top;
				id[dfn[x]=++tot]=x;
		        if(son[x])dfs1(dfs1,son[x],Top);
		        for(auto to:T[x])if(to!=fa[x]&&to!=son[x])
		    	dfs1(dfs1,to,to);
		    };
	    	dfs0(dfs0,rt),dfs1(dfs1,rt);
		}
	};
```