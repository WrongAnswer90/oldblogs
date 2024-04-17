```cpp
char s[11000005];
int n,d1[11000005],d2[11000005];
inline void manacher()
{
    n=strlen(s+1);
	for(int i=1,l=1,r=0;i<=n;++i)
	{
		int k=(i>r)?1:min(d1[l+r-i],r-i+1);
		while((0<=i-k)&&(i+k<=n)&&(s[i+k]==s[i-k]))k++;
		d1[i]=k--;
		if(i+k>r)r=i+k,l=i-k;
	}
	for(int i=1,l=1,r=0;i<=n;++i)
	{
		int k=(i>r)?0:min(d2[l+r-i+1],r-i+1);
		while((0<i-k)&&(i+k<=n)&&(s[i-k-1]==s[i+k]))k++;
		d2[i]=k--;
		if(i+k>r)r=i+k,l=i-k-1;
	}
}
```