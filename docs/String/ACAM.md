```cpp
const int N=200000;
int cnt=1,f[N+10],trie[N+10][26],fail[N+10],end[N+10];
queue<int> q;
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
    q.e(1);
    for(int i=0;i<26;++i)trie[0][i]=1;
    while(!q.empty())
    {
        int now=q.front();q.pop();
        for(int i=0;i<26;++i)
        {
            if(trie[now][i])
            {
                fail[trie[now][i]]=trie[fail[now]][i];
                add(trie[fail[now]][i],trie[now][i]);
                q.e(trie[now][i]);
            }
            else trie[now][i]=trie[fail[now]][i];
        }
    }
}
```