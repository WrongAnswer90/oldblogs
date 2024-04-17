## Binary GCD

```cpp
inline int gcd(int a,int b)
{
	int az=__builtin_ctz(a),bz=__builtin_ctz(b);
	int z=min(az,bz),tmp;b>>=bz;
	while(a)a>>=az,tmp=a-b,az=__builtin_ctz(tmp),b=min(a,b),a=abs(tmp);
	return b<<z;
}
```

## 值域预处理 GCD

```cpp
bitset<1000010> v;
int cnt,ans,f[1000010][3],c[1010][1010],pr[300010];
inline int gcd(int x,int y)
{
	int ans=1;
	for(int i=0;i<3;++i)
	{
		int tmp=f[x][i]>1000?y%f[x][i]?1:f[x][i]:c[f[x][i]][y%f[x][i]];
		y/=tmp,ans*=tmp;
	}
	return ans;
}
inline void mian()
{
	f[1][0]=f[1][1]=f[1][2]=1;
	for(int i=2;i<=1000000;++i)
	{
		if(!v[i])pr[++cnt]=i,f[i][0]=f[i][1]=1,f[i][2]=i;
		for(int j=1;j<=cnt&&i*pr[j]<=1000000;++j)
		{
			v[i*pr[j]]=1;
			f[i*pr[j]][0]=f[i][0]*pr[j];
			f[i*pr[j]][1]=f[i][1];
			f[i*pr[j]][2]=f[i][2];
			sort(f[i*pr[j]],f[i*pr[j]]+3);
			if(i%pr[j]==0)break;
		}
	}
	for(int i=1;i<=1000;++i)c[i][0]=c[0][i]=i;
	for(int i=1;i<=1000;++i)for(int j=1;j<=i;++j)c[i][j]=c[j][i]=c[j][i%j];
}
```