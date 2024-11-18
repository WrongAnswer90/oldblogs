```cpp
	struct Node{int l,r,size,rd,val,tag;}fhq[200001];
	int n,cnt,m,root;
	inline void update(int x){fhq[x].size=fhq[fhq[x].l].size+fhq[fhq[x].r].size+1;}
	inline int newnode(int val){fhq[++cnt].val=val,fhq[cnt].rd=rand()%11451419,fhq[cnt].size=1;return cnt;}
	inline void spread(int x)
	{
		if(fhq[x].tag)
		{
			swap(fhq[x].l,fhq[x].r);
			fhq[fhq[x].l].tag^=1,fhq[fhq[x].r].tag^=1;
			fhq[x].tag=0;
		}
	}
	void split(int now,int rank,int &x,int &y)
	{
		if(!now){x=y=0;return;}
		spread(now);
		if(fhq[fhq[now].l].size>=rank)y=now,split(fhq[now].l,rank,x,fhq[now].l);
		else x=now,split(fhq[now].r,rank-fhq[fhq[now].l].size-1,fhq[now].r,y);
		update(now);
	}
	int merge(int x,int y)
	{
		if(!x||!y)return x+y;
		if(fhq[x].rd>fhq[y].rd){spread(x),fhq[x].r=merge(fhq[x].r,y),update(x);return x;}
		else{spread(y),fhq[y].l=merge(x,fhq[y].l),update(y);return y;}
	}
```