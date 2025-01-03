```cpp
    struct LiCT
    {
        #define pdi pair<double,int>
        struct{double k,b;}p[600001];
        struct{int l,r,best;}t[800001];
        int cnt;
        inline int cmp(double x,double y){return x-y>eps?1:y-x>eps?-1:0;}
        inline double calc(int id,int x){return x*p[id].k+p[id].b;}
        void build(int p,int l,int r)
        {
            t[p].l=l,t[p].r=r;
            if(l==r)return;
            int mid=l+((r-l)>>1);
            build(p*2,l,mid),build(p*2+1,mid+1,r);
        }
        inline void add(int x0,int y0,int x1,int y1)
        {
            if(x0==x1)++cnt,p[cnt].k=0,p[cnt].b=max(y0,y1);
            else ++cnt,p[cnt].k=1.0*(y0-y1)/(x0-x1),p[cnt].b=(double)y0-(double)x0*p[cnt].k;
        }
        void update(int p,int u)
        {
            int &v=t[p].best;
            int mid=t[p].l+t[p].r>>1;
            int dl=cmp(calc(u,mid),calc(v,mid));
            if(dl==1||(dl==0&&u<v))swap(u,v);
            int bl=cmp(calc(u,t[p].l),calc(v,t[p].l)),br=cmp(calc(u,t[p].r),calc(v,t[p].r));
            if(bl==1||(bl==0&&u<v))update(p*2,u);
            if(br==1||(br==0&&u<v))update(p*2+1,u);
        }
        void modify(int p,int l,int r,int k)
        {
            if(l<=t[p].l&&r>=t[p].r)return update(p,k);
            if(l<=t[p*2].r)modify(p*2,l,r,k);
            if(r>t[p*2].r)modify(p*2+1,l,r,k);
        }
        pdi maxx(pdi p1,pdi p2)
        {
            if(cmp(p1.fi,p2.fi)==1)return p1;
            if(cmp(p1.fi,p2.fi)==-1)return p2;
            return p1.se<p2.se?p1:p2;
        }
        pdi ask(int p,int k)
        {
            if(t[p].l==t[p].r)return mp(calc(t[p].best,k),t[p].best);
            pdi res=mp(calc(t[p].best,k),t[p].best);
            if(k<=t[p*2].r)return maxx(res,ask(p*2,k));
            return maxx(res,ask(p*2+1,k));
        }
    };
```

```cpp
	namespace Segment
	{
		int cnt,tot,root;
		pii b[200010];
		struct{int ls,rs,i;}t[200010];
		#define ls(x) t[x].ls
		#define rs(x) t[x].rs
		inline int get(int i,int x){return x*b[i].fi+b[i].se;}
		void modify(int&p,int L,int R,int i)
		{
			if(!p)return t[p=++tot]={0,0,i},void();
			int mid=(L+R)>>1;
			if(get(i,mid)>get(t[p].i,mid))swap(i,t[p].i);
			if(get(i,L)>get(t[p].i,L))modify(ls(p),L,mid,i);
			if(get(i,R)>get(t[p].i,R))modify(rs(p),mid+1,R,i);
		}
		int ask(int p,int L,int R,int x)
		{
			if(!p)return -INF;
			int mid=(L+R)>>1,v=get(t[p].i,x);
			if(x<=mid)return max(v,ask(ls(p),L,mid,x));
			return max(v,ask(rs(p),mid+1,R,x));
		}
		void clr(){root=cnt=tot=0;}
	}
	using namespace Segment;
```