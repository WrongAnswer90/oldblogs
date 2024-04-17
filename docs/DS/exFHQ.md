```cpp
struct Node{int l,r,siz,rd;bool tag;long long val,sum;}fhq[20000001];
int cnt,n,root[300001];
long long lastans=0;
inline void update(int x)
{
	fhq[x].siz=fhq[fhq[x].l].siz+fhq[fhq[x].r].siz+1;
	fhq[x].sum=fhq[fhq[x].l].sum+fhq[fhq[x].r].sum+fhq[x].val;
}
inline int newnode(long long val){fhq[++cnt].val=val,fhq[cnt].siz=1,fhq[cnt].sum=val,fhq[cnt].rd=rand()%114514191;return cnt;}
inline int copy(int x){fhq[++cnt]=fhq[x];return cnt;}
inline void spread(int x)
{
	if(fhq[x].tag)
	{
		if(fhq[x].l)fhq[x].l=copy(fhq[x].l);
		if(fhq[x].r)fhq[x].r=copy(fhq[x].r);
		swap(fhq[x].l,fhq[x].r);
		if(fhq[x].l)fhq[fhq[x].l].tag^=1;
		if(fhq[x].r)fhq[fhq[x].r].tag^=1;
		fhq[x].tag=0;
	}
}
void split(int now,int siz,int &x,int &y)
{
	if(!now){x=y=0;return;}
	spread(now);
	if(fhq[fhq[now].l].siz+1<=siz)
	x=copy(now),split(fhq[x].r,siz-fhq[fhq[x].l].siz-1,fhq[x].r,y),update(x);
	else y=copy(now),split(fhq[y].l,siz,x,fhq[y].l),update(y);
}
int merge(int x,int y)
{
	if(!x||!y)return x+y;
	spread(x),spread(y);
	if(fhq[x].rd<fhq[y].rd){fhq[x].r=merge(fhq[x].r,y),update(x);return x;}
	fhq[y].l=merge(x,fhq[y].l),update(y);return y;
}
```