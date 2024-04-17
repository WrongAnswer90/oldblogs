```cpp
struct{int l,r,fa,val,dis;}t[300001];
int find(int x){return x==t[x].fa?x:t[x].fa=find(t[x].fa);}
int merge(int x,int y)
{
    if(!x||!y)return x|y;
    if(t[x].val>t[y].val||(t[x].val==t[y].val&&x>y))swap(x,y);
    t[x].r=merge(t[x].r,y);
    t[t[x].r].fa=x;
    if(t[t[x].l].dis<t[t[x].r].dis)swap(t[x].l,t[x].r);
    t[x].dis=t[t[x].r].dis+1;
    return x;
}
inline void pop(int x){t[x].val=-1,t[t[x].l].fa=t[t[x].r].fa=t[x].fa=t[x].l=merge(t[x].l,t[x].r);}
```