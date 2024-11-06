## Without Tag

```cpp
template<class Info> struct Segment
{
	#define ls(p) (t[p].l+t[p].r)
	#define rs(p) (ls(p)^1)
	private:
		vector<Info> tmp;
		struct{int l,r;Info v;}t[N<<1];
		void build(int p,int l,int r)
		{
			t[p].l=l,t[p].r=r;
			if(l==r)return t[p].v=tmp[l-1],void();
			int mid=(l+r)>>1;
			build(ls(p),l,mid),build(rs(p),mid+1,r);
			t[p].v=t[ls(p)].v+t[rs(p)].v;
		}
	public:
		void change(int p,int x,Info v)
		{
			if(t[p].l==t[p].r)return t[p].v=v,void();
			change(ls(p)^(x>t[ls(p)].r),x,v);
			t[p].v=t[ls(p)].v+t[rs(p)].v;
		}
		Info ask(int p,int l,int r)
		{
			if(l<=t[p].l&&r>=t[p].r)return t[p].v;
			if(r<=t[ls(p)].r)return ask(ls(p),l,r);
			if(l>t[ls(p)].r)return ask(rs(p),l,r);
			return ask(ls(p),l,r)+ask(rs(p),l,r);
		}
		template<class Compare> pair<int,Info>
		findnex(int p,int x,Info y,Compare cmp)
		{
			if(x>t[p].r||!cmp(y,t[p].v))return mp(0,(Info){});
			if(t[p].l==t[p].r)return mp(t[p].l,t[p].v);
			pair<int,Info> v=findnex(ls(p),x,y,cmp);
			if(v.fi)return v;
			return findnex(rs(p),x,y,cmp);
		}
		template<class Compare> pair<int,Info>
		findpre(int p,int x,Info y,Compare cmp)
		{
			if(x<t[p].l||!cmp(y,t[p].v))return mp(0,(Info){});
			if(t[p].l==t[p].r)return mp(t[p].l,t[p].v);
			pair<int,Info> v=findpre(rs(p),x,y,cmp);
			if(v.fi)return v;
			return findpre(ls(p),x,y,cmp);
		}
		void print(int p=1)
		{
			if(t[p].l==t[p].r)return t[p].v.print();
			print(ls(p)),print(rs(p));
		}
		template<class Compare> pair<int,Info>
		findnex(int x,Info y,Compare cmp)
		{return findnex(1,x,y,cmp);}
		template<class Compare> pair<int,Info>
		findpre(int x,Info y,Compare cmp)
		{return findpre(1,x,y,cmp);}
		void change(int x,Info v){change(1,x,v);}
		Info ask(int l,int r){return ask(1,l,r);}
		void init(vector<Info> ve){tmp=ve,build(1,1,ve.size());}
	#undef ls
	#undef rs
};
struct Info
{
	//...
	Info(){}
	Info operator +(const Info nd)const
	{
		//...
	}
	void print()
	{
		//...
	}
};
```

## With Tag

```cpp
template<class Info,class Tag> struct Segment
{
	#define ls(p) (t[p].l+t[p].r)
	#define rs(p) (ls(p)^1)
	private:
		vector<Info> tmp;
		struct{int l,r;Info v;Tag tg;}t[N<<1];
		inline void down(int p,Tag tg){t[p].v=t[p].v*tg,t[p].tg=t[p].tg*tg;}
		inline void spread(int p){down(ls(p),t[p].tg),down(rs(p),t[p].tg),t[p].tg=Tag();}
		void build(int p,int l,int r)
		{
			t[p].l=l,t[p].r=r;
			if(l==r)return t[p].v=tmp[l-1],void();
			int mid=l+((r-l)>>1);
			build(ls(p),l,mid),build(rs(p),mid+1,r);
			t[p].v=t[ls(p)].v+t[rs(p)].v;
		}
		void change(int p,int x,Info v)
		{
			if(t[p].l==t[p].r)return t[p].v=v,void();
			spread(p);
			change(ls(p)^(x>t[ls(p)].r),x,v);
			t[p].v=t[ls(p)].v+t[rs(p)].v;
		}
		Info ask(int p,int l,int r)
		{
			if(l<=t[p].l&&r>=t[p].r)return t[p].v;
			spread(p);
			if(r<=t[ls(p)].r)return ask(ls(p),l,r);
			if(l>t[ls(p)].r)return ask(rs(p),l,r);
			return ask(ls(p),l,r)+ask(rs(p),l,r);
		}
		void modify(int p,int l,int r,Tag tg)
		{
			if(l<=t[p].l&&r>=t[p].r)return down(p,tg);
			spread(p);
			if(l<=t[ls(p)].r)modify(ls(p),l,r,tg);
			if(r>t[ls(p)].r)modify(rs(p),l,r,tg);
			t[p].v=t[ls(p)].v+t[rs(p)].v;
		}
		template<class Compare> pair<int,Info>
		findnex(int p,int x,Info y,Compare cmp)
		{
			if(x>t[p].r||!cmp(y,t[p].v))return mp(0,(Info){});
			if(t[p].l==t[p].r)return mp(t[p].l,t[p].v);
			spread(p);
			pair<int,Info> v=findnex(ls(p),x,y,cmp);
			if(v.fi)return v;
			return findnex(rs(p),x,y,cmp);
		}
		template<class Compare> pair<int,Info>
		findpre(int p,int x,Info y,Compare cmp)
		{
			if(x<t[p].l||!cmp(y,t[p].v))return mp(0,(Info){});
			if(t[p].l==t[p].r)return mp(t[p].l,t[p].v);
			spread(p);
			pair<int,Info> v=findpre(rs(p),x,y,cmp);
			if(v.fi)return v;
			return findpre(ls(p),x,y,cmp);
		}
	public:
		void print(int p=1)
		{
			if(t[p].l==t[p].r)return t[p].v.print();
			spread(p),print(ls(p)),print(rs(p));
		}
		void printall(int p=1)
		{
			write('[',t[p].l,',',t[p].r,']',':');
			t[p].v.print();
			spread(p),print(ls(p)),print(rs(p));
		}
		void modify(int l,int r,Tag tg){modify(1,l,r,tg);}
		template<class Compare> pair<int,Info>
		findnex(int x,Info y,Compare cmp)
		{return findnex(1,x,y,cmp);}
		template<class Compare> pair<int,Info>
		findpre(int x,Info y,Compare cmp)
		{return findpre(1,x,y,cmp);}
		void change(int x,Info v){change(1,x,v);}
		Info ask(int l,int r){return ask(1,l,r);}
		void init(vector<Info> ve){tmp=ve,build(1,1,ve.size());}
	#undef ls
	#undef rs
};
struct Tag
{
	//...
	Tag(){}
	Tag operator *(const Tag nd)const
	{
		Tag v;
		//...
		return v;
	}
};
struct Info
{
	//...
	Info(){}
	Info operator +(const Info nd)const
	{
		Info v;
		//...
		return v;
	}
	Info operator *(const Tag nd)const
	{
		Info v;
		//...
		return v;
	}
	void print()
	{
		//...
	}
};
```
