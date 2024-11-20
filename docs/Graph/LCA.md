```cpp
	struct _LCA
	{
		int *dfn,*F[19];
	    inline int get(int x,int y){return dfn[x]<dfn[y]?x:y;}
	    inline int lca(int x,int y)
	    {
	        if(x==y)return x;
	        if((x=dfn[x])>(y=dfn[y]))swap(x,y);
	        int k=__lg(y-x++);
	        return get(F[k][x],F[k][y-(1<<k)+1]);
	    }
	    void init(vi*T,int n,int rt=1)
	    {
	    	dfn=new int[n+1];
	    	for(int i=0;i<19;++i)F[i]=new int[n+1];
	    	tot=dfn[0]=0;
	    	auto dfs=[&](auto dfs,int x,int fa)->void
		    {
		        F[0][dfn[x]=++tot]=fa;
		        for(auto to:T[x])
				if(to!=fa)dfs(dfs,to,x);
			};
	        dfs(dfs,rt,0);
	        for(int i=1;i<19;++i)
	        {
	            for(int j=1;j+(1<<i)-1<=n;++j)
	            F[i][j]=get(F[i-1][j],F[i-1][j+(1<<(i-1))]);
	        }
		}
	};
```