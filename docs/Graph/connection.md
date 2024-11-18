## 边双联通分量

```cpp
    const int N=100000,M=200000;
    int n,m,cnt=1,num,col,low[N+10],dfn[N+10],head[N+10],to[M+10],nex[M+10];
    inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
    stack<int> st;
    vi ve[N+10];
    void tarjan(int x,int fromi)
    {
        dfn[x]=low[x]=++num,st.e(x);
        for(int i=head[x];i;i=nex[i])
        {
            if(i==(fromi^1))continue;
            if(!dfn[to[i]])tarjan(to[i],i),low[x]=min(low[x],low[to[i]]);
            else low[x]=min(low[x],dfn[to[i]]);
        }
        if(low[x]==dfn[x])
        {
            int y;++col;
            do ve[col].eb(y=st.top()),st.pop();while(y!=x);
        }
    }
```

## 强联通分量

```cpp
    const int N=100000;
    int tot,dfn[N+10],low[N+10];
    int num,c[N+10];
    int ins[N+10];
    stack<int> st;
    vi G[N+10];
    void tarjan(int x)
    {
        st.e(x),ins[x]=1,dfn[x]=low[x]=++tot;
        for(auto to:G[x])
        {
            if(!dfn[to])tarjan(to),low[x]=min(low[x],low[to]);
            else if(ins[to])low[x]=min(low[x],dfn[to]);
        }
        if(dfn[x]==low[x])
        {
            int y;++num;
            do c[y=st.top()]=num,ins[y]=0,st.pop();while(y!=x);
        }
    }
```

```cpp
    const int N=100000,M=200010;
    int cnt,head[M+10],to[M+10],nex[M+10];
    int tot,dfn[N+10],low[N+10];
    int num,c[N+10];
    int ins[N+10];
    inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
    stack<int> st;
    void tarjan(int x)
    {
        st.e(x),ins[x]=1,dfn[x]=low[x]=++tot;
        for(int i=head[x];i;i=nex[i])
        {
            if(!dfn[to[i]])tarjan(to[i]),low[x]=min(low[x],low[to[i]]);
            else if(ins[to[i]])low[x]=min(low[x],dfn[to[i]]);
        }
        if(dfn[x]==low[x])
        {
            int y;++num;
            do c[y=st.top()]=num,ins[y]=0,st.pop();while(y!=x);
        }
    }
```

## 广义圆方树

```cpp
    const int N=100010;
    int tot,low[N+10],dfn[N+10];
    int num;
    vi G[N+10],T[N*2+10];
    stack<int> st;
    void tarjan(int x)
    {
        low[x]=dfn[x]=++tot,st.e(x);int y;
        for(auto to:G[x])
        {
            if(!dfn[to])
            {
                tarjan(to),low[x]=min(low[x],low[to]);
                if(low[to]>=dfn[x])
                {
                    T[++num].eb(x),T[x].eb(num);
                    do T[num].eb(y=st.top()),T[st.top()].eb(num),st.pop();while(y!=to);
                }
            }
            else low[x]=min(low[x],dfn[to]);
        }
    }
```