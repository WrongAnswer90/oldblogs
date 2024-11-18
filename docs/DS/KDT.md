```cpp
    struct Node{int v,sum,ls,rs,x[2],L[2],R[2];Node(int X=0,int Y=0,int Z=0){x[0]=X,x[1]=Y,v=Z;}}t[200010],l,r;
    int ans,n,cnt,root[18],b[200010];
    #define ls(x) t[x].ls
    #define rs(x) t[x].rs
    inline void update(int p)
    {
        t[p].sum=t[ls(p)].sum+t[rs(p)].sum+t[p].v;
        for(int k=0;k<2;++k)
        {
            t[p].L[k]=t[p].R[k]=t[p].x[k];
            if(ls(p))Mmin(t[p].L[k],t[ls(p)].L[k]),Mmax(t[p].R[k],t[ls(p)].R[k]);
            if(rs(p))Mmin(t[p].L[k],t[rs(p)].L[k]),Mmax(t[p].R[k],t[rs(p)].R[k]);
        }
    }
    inline bool cmpx(int x,int y){return t[x].x[0]<t[y].x[0];}
    inline bool cmpy(int x,int y){return t[x].x[1]<t[y].x[1];}
    int build(int l,int r,int dep=0)
    {
        int mid=l+((r-l)>>1);
        nth_element(b+l,b+mid,b+r+1,dep?cmpx:cmpy);
        int x=b[mid];
        if(l<mid)ls(x)=build(l,mid-1,dep^1);
        if(mid<r)rs(x)=build(mid+1,r,dep^1);
        return update(x),x;
    }
    void append(int&p){if(p)b[++cnt]=p,append(ls(p)),append(rs(p)),p=0;}
    int query(int p)
    {
        if(!p)return 0;
        bool flag=1;
        for(int k=0;k<2;++k)flag&=l.x[k]<=t[p].L[k]&&r.x[k]>=t[p].R[k];
        if(flag)return t[p].sum;
        for(int k=0;k<2;++k)if(t[p].R[k]<l.x[k]||t[p].L[k]>r.x[k])return 0;
        flag=1;
        for(int k=0;k<2;++k)flag&=l.x[k]<=t[p].x[k]&&r.x[k]>=t[p].x[k];
        return flag*t[p].v+query(ls(p))+query(rs(p));
    }
    inline void ins(int x,int y,int z)
    {
        t[++n]=Node(x,y,z),b[cnt=1]=n;
        for(int i=0;;++i)
        {
            if(!root[i]){root[i]=build(1,cnt);break;}
            else append(root[i]);
        }
    }
```