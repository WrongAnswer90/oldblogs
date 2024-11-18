```cpp
    namespace Splay
    {
        struct{int fa,ch[2],val,siz,sum;bool tag;}spl[1000001];
        #define ls(x) (spl[x].ch[0])
        #define rs(x) (spl[x].ch[1])
        #define fa(x) (spl[x].fa)
        #define ident(x,f) (rs(f)==x)
        #define connect(x,f,s) (spl[fa(x)=f].ch[s]=x)
        #define update(x) (spl[x].siz=spl[ls(x)].siz+spl[rs(x)].siz+1,spl[x].sum=spl[ls(x)].sum^spl[rs(x)].sum^spl[x].val)
        #define ntroot(x) ((ls(fa(x))==x)||(rs(fa(x))==x))
        #define down(x) (swap(ls(x),rs(x)),spl[x].tag^=1)
        inline void spread(int k)
        {
            if(spl[k].tag)down(ls(k)),down(rs(k));
            spl[k].tag=0;
        }
        void pushall(int x){if(ntroot(x))pushall(fa(x));spread(x);}
        inline void rotate(int x)
        {
            int f=fa(x),ff=fa(f),s=ident(x,f);
            if(ntroot(f))spl[ff].ch[ident(f,ff)]=x;
            connect(spl[x].ch[s^1],f,s),fa(x)=ff,connect(f,x,s^1),update(f),update(x);
        }
        inline void splaying(int x)
        {
            pushall(x);
            while(ntroot(x))
            {
                int f=fa(x),ff=fa(f);
                if(ntroot(f))(ident(x,f)^ident(f,ff))?rotate(x):rotate(f);
                rotate(x);
            }
        }
    }
    using namespace Splay;
    inline void access(int x){for(int y=0;x;x=fa(y=x))splaying(x),rs(x)=y,update(x);}
    inline void mkroot(int x){access(x),splaying(x),down(x);}
    inline int findroot(int x)
    {
        access(x),splaying(x),spread(x);
        while(ls(x))x=ls(x),spread(x);
        splaying(x);
        return x;
    }
    inline void link(int x,int y)
    {
        mkroot(x);
        if(findroot(y)==x)return;
        fa(x)=y;
    }
    inline void cut(int x,int y)
    {
        mkroot(x);
        if(findroot(y)!=x||fa(y)!=x||ls(y))return;
        fa(y)=rs(x)=0,update(x);
    }
    inline void split(int x,int y){mkroot(x),access(y),splaying(y);}
```