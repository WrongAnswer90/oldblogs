```cpp
	int t[501][501];
	int n,m,match[501],pre[501];
	ll ex[501],ey[501],vy[501],slack[501];
	void bfs(int k)
	{
		ll x,y=0,yy=0,delta;
		memset(pre,0,sizeof(pre)),memset(slack,127,sizeof(slack)),match[y]=k;
		while(1)
		{
			x=match[y];delta=INF,vy[y]=1;
			for(int i=1;i<=n;++i)
			{
				if(vy[i])continue;
				if(slack[i]>ex[x]+ey[i]-t[x][i])slack[i]=ex[x]+ey[i]-t[x][i],pre[i]=y;
				if(slack[i]<delta)delta=slack[i],yy=i;
			}
			for(int i=0;i<=n;++i)
			{
				if(vy[i])ex[match[i]]-=delta,ey[i]+=delta;
				else slack[i]-=delta;
			}
			y=yy;
			if(match[y]==-1)break;
		}
		while(y){match[y]=match[pre[y]],y=pre[y];}
	}
	ll KM()
	{
		memset(match,-1,sizeof(match)),memset(ex,0,sizeof(ex)),memset(ey,0,sizeof(ey));
		for(int i=1;i<=n;++i)memset(vy,0,sizeof(vy)),bfs(i);
		ll res=0;
		for(int i=1;i<=n;++i)if(match[i]!=-1)res+=t[match[i]][i];
		return res;
	}
```