```cpp
    struct{int fa,ch[2],val,siz,sum;bool tag;}spl[1000001];
    #define ls(x) (spl[x].ch[0])
    #define rs(x) (spl[x].ch[1])
    #define fa(x) (spl[x].fa)
    #define ident(x,f) (rs(f)==x)
    #define connect(x,f,s) (spl[fa(x)=f].ch[s]=x)
    #define update(x) (spl[x].siz=spl[ls(x)].siz+spl[rs(x)].siz+1,spl[x].sum=spl[ls(x)].sum^spl[rs(x)].sum^spl[x].val)
    #define down(x) (swap(ls(x),rs(x)),spl[x].tag^=1)
    inline void spread(int k)
    {
        if(spl[k].tag)down(ls(k)),down(rs(k));
        spl[k].tag=0;
    }
    inline void rotate(int x)
    {
        int f=fa(x),ff=fa(f),s=ident(x,f);
        connect(spl[x].ch[s^1],f,s),connecf(x,ff,ident(f,ff));
        connect(f,x,s^1),update(f),update(x);
    }
    inline void splaying(int x)
    {
        while(fa(x))
        {
            int f=fa(x),ff=fa(f);
            if(fa(f))(ident(x,f)^ident(f,ff))?rotate(x):rotate(f);
            rotate(x);
        }
    }
```