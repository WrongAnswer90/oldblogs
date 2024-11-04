```cpp
const int N=1000000;
char s[N+10];
int n,f[N+10],g[N+10];
inline void manacher()
{
    n=strlen(s+1);
	s[0]='*',s[n+1]='#';
	for(int i=1,p=0,r=0;i<=n;++i)
	{
		if(i<=r)f[i]=min(r-i+1,f[2*p-i]);
		while(s[i-f[i]]==s[i+f[i]])++f[i];
		if(Mmax(r,i+f[i]-1))p=i;
	}
	for(int i=1,p=0,r=0;i<n;++i)
	{
		if(i<=r)g[i]=min(r-i,g[2*p-i]);
		while(s[i-g[i]]==s[i+g[i]+1])++g[i];
		if(Mmax(r,i+g[i]))p=i;
	}
}
```