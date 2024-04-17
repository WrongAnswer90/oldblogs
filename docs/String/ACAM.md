```cpp
int f[200001],trie[200001][26],cnt,fail[200001],head[200001],to[200001],nex[200001],tot;
queue<int> q;
inline void add(int x,int y){to[++tot]=y,nex[tot]=head[x],head[x]=tot;}
inline void init(){memset(trie,0,sizeof(trie)),memset(f,0,sizeof(f)),memset(head,0,sizeof(head)),tot=0,cnt=1;}
inline void insert(int id)
{
    int now=1;
    for(int i=1;i<=len;++i)
    {
        if(!trie[now][s[i]-'a'])trie[now][s[i]-'a']=++cnt;
        now=trie[now][s[i]-'a'];
    }
    end[id]=now;
}
inline void build()
{
    q.emplace(1);
    for(int i=0;i<26;++i)trie[0][i]=1;
    while(!q.empty())
    {
        int now=q.front();q.pop();
        for(int i=0;i<26;++i)
        {
            if(trie[now][i])fail[trie[now][i]]=trie[fail[now]][i],add(trie[fail[now]][i],trie[now][i]),q.emplace(trie[now][i]);
            else trie[now][i]=trie[fail[now]][i];
        }
    }
}
```