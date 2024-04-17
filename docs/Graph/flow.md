## 最大流

```cpp
int S,T,cnt=1,n,m,head[201],to[10001],nex[10001],v[10001],d[201],now[201];
inline void add(int x,int y,int z){to[++cnt]=y,nex[cnt]=head[x],head[x]=cnt,v[cnt]=z;}
queue<int> q;
int bfs()
{
    while(!q.empty())q.pop();
    q.e(S),memset(d,0,sizeof(d)),d[S]=1,now[S]=head[S];
    while(!q.empty())
    {
        int k=q.front();q.pop();
        for(int i=head[k];i;i=nex[i])
        {
            if(v[i]&&!d[to[i]])
            {
                d[to[i]]=d[k]+1,now[to[i]]=head[to[i]],q.e(to[i]);
                if(to[i]==T)return 1;
            }
        }
    }
    return 0;
}
int dinic(int k,int flow)
{
    if(k==T)return flow;
    int rest=flow,t;
    for(int i=now[k];i&&rest;i=nex[i])
    {
        now[k]=i;
        if(d[to[i]]!=d[k]+1||!v[i])continue;
        t=dinic(to[i],min(v[i],rest));
        if(!t)d[to[i]]=0;
        rest-=t,v[i]-=t,v[i^1]+=t;
    }
    return flow-rest;
}
```

## 费用流

```cpp
int cnt=1,n,m,S,T,sum,ans,incf[5010],pre[5010],d[5010],h[5010],head[5010],to[100010],nex[100010],v[100010],c[100010];
inline void Add(int x,int y,int z,int t){to[++cnt]=y,v[cnt]=z,nex[cnt]=head[x],c[cnt]=t,head[x]=cnt;}
inline void add(int x,int y,int z,int t){Add(x,y,z,t),Add(y,x,0,-t);}
priority_queue<pii> q;
bool vis[5010];
bool dij()
{
    for(int i=1;i<=n;++i)if(vis[i])h[i]+=d[i];
    memset(d,127,sizeof(d)),memset(incf,0,sizeof(incf)),memset(vis,0,sizeof(vis));
    incf[S]=inf,d[S]=0,q.e(mp(0,S));
    while(!q.empty())
    {
        int now=q.top().se;q.pop();
        if(vis[now])continue;
        vis[now]=1;
        for(int i=head[now];i;i=nex[i])
        {
            if(v[i]&&Mmin(d[to[i]],d[now]+c[i]+h[now]-h[to[i]]))
            incf[to[i]]=min(v[i],incf[now]),pre[to[i]]=i,q.e(mp(-d[to[i]],to[i]));
        }
    }
    return incf[T];
}
inline void update()
{
    int now=T;sum+=incf[T],ans+=incf[T]*(d[T]-h[S]+h[T]);
    while(now!=S)v[pre[now]]-=incf[T],v[pre[now]^1]+=incf[T],now=to[pre[now]^1];
}
```