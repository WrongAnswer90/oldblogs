```cpp
int pos[1000010];
char s[1000010];
namespace Trie
{
    int trie[1000010][26],tot=1;
    inline void insert()
    {
        int now=1,n=strlen(s);
        for(int i=0;i<n;++i)
        {
            if(!trie[now][s[i]-'a'])trie[now][s[i]-'a']=++tot;
            now=trie[now][s[i]-'a'];
        }
    }
}
using namespace Trie;
int cnt=1;
struct{int ch[26],fa,len;}t[2000010];
inline int add(int x,int last)
{
    int np=++cnt,p=last,q,nq;t[np].len=t[p].len+1;
    while(!t[p].ch[x])t[p].ch[x]=np,p=t[p].fa;
    if(t[q=t[p].ch[x]].len==t[p].len+1)return t[np].fa=q,np;
    t[nq=++cnt]=t[q],t[nq].len=t[p].len+1,t[np].fa=t[q].fa=nq;
    while(p&&t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
    return np;
}
queue<int> q;
void build()
{
    for(int i=0;i<26;++i)t[0].ch[i]=1;
    t[0].len=-1;q.e(1),pos[1]=1;
    while(!q.empty())
    {
        int now=q.front();q.pop();
        for(int i=0;i<26;++i)
        {
            if(trie[now][i])
            pos[trie[now][i]]=add(i,pos[now]),q.e(trie[now][i]);
        }
    }
}
```