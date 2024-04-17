```cpp
int n,maxn,m=128,F[21][1000001],sa[1000001],rk[1000001],b[1000001],x[1000001],y[1000001];
char s[250001];
inline int ask(int x,int y)
{
    if(x==y)return inf;
    if((x=rk[x])>(y=rk[y]))swap(x,y);
    int k=__lg(y-x++);
    return min(F[k][x],F[k][y-(1<<k)+1]);
}
inline void SA()
{
    scanf("%s",s+1),n=strlen(s+1);
    for(int i=1;i<=n;++i)++b[x[i]=s[i]];
    for(int i=2;i<=m;++i)b[i]+=b[i-1];
    for(int i=1;i<=n;++i)sa[b[x[i]]--]=i;
    for(int k=1,num=0;k<=n;k<<=1,num=0)
    {
        for(int i=n-k+1;i<=n;++i)y[++num]=i;
        for(int i=1;i<=n;++i)if(sa[i]>k)y[++num]=sa[i]-k;
        for(int i=1;i<=m;++i)b[i]=0;
        for(int i=1;i<=n;++i)++b[x[i]];
        for(int i=2;i<=m;++i)b[i]+=b[i-1];
        for(int i=n;i>=1;--i)sa[b[x[y[i]]]--]=y[i];
        swap(x,y),x[sa[1]]=1;
        for(int i=2;i<=n;++i)
        {
            if(y[sa[i]]==y[sa[i-1]]&&y[sa[i]+k]==y[sa[i-1]+k])x[sa[i]]=x[sa[i-1]];
            else x[sa[i]]=x[sa[i-1]]+1;
        }
        if((m=x[sa[n]])==n)break;
    }
    for(int i=1;i<=n;++i)rk[sa[i]]=i;
    for(int i=1,k=0;i<=n;++i,k?--k:0)
    {
        while(s[i+k]==s[sa[rk[i]-1]+k])++k;
        F[0][rk[i]]=k;
    }
    for(int i=1;i<=20;++i)
    {
        for(int j=1;j+(1<<i)-1<=n;++j)
        F[i][j]=min(F[i-1][j],F[i-1][j+(1<<(i-1))]);
    }
}
```