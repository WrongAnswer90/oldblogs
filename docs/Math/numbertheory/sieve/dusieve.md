```cpp
unordered_map<int,ll> phi;
unordered_map<int,int> mu;
ll PHI[20000001];
int MU[20000001],prime[10000001],cnt,T,n;
bool v[20000001];
inline int read()
{
	int ans=0;char ch=getchar();
	while((ch>'9')||(ch<'0'))ch=getchar();
	while((ch>='0')&&(ch<='9'))ans=ans*10+ch-'0',ch=getchar();
	return ans;
}
ll askphi(int k)
{
	if(k<=20000000)return PHI[k];
	if(phi[k])return phi[k];
	ll sum=k*(k+1)/2;
	for(int l=2,r;l<=k;l=r+1)r=k/(k/l),sum-=(r-l+1)*askphi(k/l);
	return phi[k]=sum;
}
int askmu(int k)
{
	if(k<=20000000)return MU[k];
	if(mu[k])return mu[k];
	int sum=1;
	for(int l=2,r;l<=k;l=r+1)r=k/(k/l),sum-=(r-l+1)*askmu(k/l);
	return mu[k]=sum;
}
inline void sieve()
{
	MU[1]=1,PHI[1]=1;
	for(int i=2;i<=20000000;++i)
	{
		if(!v[i])prime[++cnt]=i,PHI[i]=i-1,MU[i]=-1;
		for(int j=1;j<=cnt;++j)
		{
			if(i*prime[j]>20000000)break;
			v[prime[j]*i]=1;
			if(i%prime[j]==0){PHI[i*prime[j]]=PHI[i]*prime[j],MU[i*prime[j]]=0;break;}
			PHI[prime[j]*i]=PHI[i]*(prime[j]-1),MU[i*prime[j]]=-MU[i];
		}
		PHI[i]+=PHI[i-1],MU[i]+=MU[i-1];
	}
}
```