## 边双联通分量

```cpp
int n,m,cnt=1,num,col,low[500001],dfn[500001],head[500001],to[4000001],nex[4000001];
inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
stack<int> st;
vector<int> ve[500001];
void tarjan(int k,int fromi)
{
    dfn[k]=low[k]=++num,st.e(k);
    for(int i=head[k];i;i=nex[i])
    {
        if(i==(fromi^1))continue;
        if(!dfn[to[i]])tarjan(to[i],i),low[k]=min(low[k],low[to[i]]);
        else low[k]=min(low[k],dfn[to[i]]);
    }
    if(low[k]==dfn[k])
    {
        int y;++col;
        do ve[col].eb(y=st.top()),st.pop();while(y!=k);
    }
}
```

## 强联通分量

```cpp
int n,m,cnt,num,col,f[10001],deg[10001],c[10001],dfn[10001],low[10001],ins[10001],x[100001],y[100001],head[10001],to[100001],nex[100001];
inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
stack<int> st;
queue<int> q;
void tarjan(int k)
{
    st.e(k),ins[k]=1,dfn[k]=low[k]=++num;
    for(int i=head[k];i;i=nex[i])
    {
        if(!dfn[to[i]])tarjan(to[i]),low[k]=min(low[k],low[to[i]]);
        else if(ins[to[i]])low[k]=min(low[k],dfn[to[i]]);
    }
    if(dfn[k]==low[k])
    {
        int y;++col;
        do c[y=st.top()]=col,ins[y]=0,st.pop();while(y!=k);
    }
}
```

## 广义圆方树

```cpp
int n,m,q,cnt,tot,num,x[200001],y[200001],low[40001],dfn[40001],head[40001],to[80001],nex[80001];
inline void add(int x,int y){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt;}
vector<int> G[20001],T[40010];
stack<int> st;
void tarjan(int k)
{
    low[k]=dfn[k]=++tot,st.e(k);int y;
    for(auto to:G[k])
    {
        if(!dfn[to])
        {
            tarjan(to),low[k]=min(low[k],low[to]);
            if(low[to]>=dfn[k])
            {
                T[++num].eb(k),T[k].eb(num);
                do T[num].eb(y=st.top()),T[st.top()].eb(num),st.pop();while(y!=to);
            }
        }
        else low[k]=min(low[k],dfn[to]);
    }
}
```