```cpp
	int nex[800010],pre[800010],tmp[800010];
	inline void del(int x){nex[pre[x]]=nex[x],pre[nex[x]]=pre[x];}
	inline int get(int y,int x){if(x==y)return INF;return v[x]+calc(x+1,y);}
	void SMAWK(vi x,vi y)
	{
		auto reduce=[&]()->void
		{
			for(int i=0;i+1<(int)y.size();++i)
			nex[y[i]]=y[i+1],pre[y[i+1]]=y[i];
			pre[y[0]]=n+1,nex[n+1]=y[0],nex[y.back()]=-1;
			int i=0,Y=y[0],j=(int)y.size()-(int)x.size();
			while(j>0)
			{
				if(get(x[i],Y)>get(x[i],nex[Y]))
				{
					del(Y),--j;
					if(i)Y=pre[Y],--i;
					else Y=nex[Y];
				}
				else
				{
					if(i+1==(int)x.size())
					del(nex[Y]),--j;
					else Y=nex[Y],++i;
				}
			}
			y.clear(),j=nex[n+1];
			while(j!=-1)y.eb(j),j=nex[j];
		};
		reduce();
		if(x.size()==1)return y;
		vi xx,pos,rl={0},Pos;
		for(int j=1;j<(int)x.size();j+=2)xx.eb(x[j]);
		pos=SMAWK(xx,y),Pos.resize(x.size());
		for(int j=0;j<(int)y.size();++j)tmp[y[j]]=j;
		rl.resize(xx.size()+2);
		for(int k=0;k<(int)xx.size();++k)rl[k+1]=tmp[pos[k]];
		rl[xx.size()+1]=(int)y.size()-1;
		for(int j=0;j<(int)xx.size();++j)
		Pos[(j<<1)|1]=pos[j];
		for(int j=0;j<(int)x.size();j+=2)
		{
			int mx=inf,p=-1;
			for(int k=rl[j>>1];k<=rl[(j>>1)+1];++k)
			if(Mmin(mx,get(x[j],y[k])))p=y[k];
			Pos[j]=p;
		}
		return Pos;
	}
```
