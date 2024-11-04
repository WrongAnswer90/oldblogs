```cpp
int last,cnt=1,pos[1000010];
struct{int fa,len,ch[26];}t[2000010];
inline void add(int x)
{
    int p=last,np,q,nq;
    if((q=t[p].ch[x]))
    {
        if(t[q].len==t[p].len+1)return last=q,void();
        t[last=nq=++cnt]=t[q],t[q].fa=nq,t[nq].len=t[p].len+1;
        while(t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
        return;
    }
    t[last=np=++cnt].len=t[p].len+1;
    while(!t[p].ch[x])t[p].ch[x]=np,p=t[p].fa;
    if(t[q=t[p].ch[x]].len==t[p].len+1)return t[np].fa=q,void();
    t[nq=++cnt]=t[q],t[q].fa=t[np].fa=nq,t[nq].len=t[p].len+1;
    while(t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
}
```