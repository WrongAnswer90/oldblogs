```cpp
    int last=1,cnt=1;
    struct{int ch[26],fa,len;}t[2000010];
    inline void add(int x)
    {
        int np=++cnt,p=last,q,nq;t[last=np].len=t[p].len+1,f[np]=1;
        while(p&&!t[p].ch[x])t[p].ch[x]=np,p=t[p].fa;
        if(!p)return t[np].fa=1,void();
        if(t[q=t[p].ch[x]].len==t[p].len+1)return t[np].fa=q,void();
        t[nq=++cnt]=t[q],t[nq].len=t[p].len+1,t[np].fa=t[q].fa=nq;
        while(p&&t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
    }
```