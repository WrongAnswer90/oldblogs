感觉常数还算比较优秀 QwQ。以下数据范围除了 FFT 均是 $10^5$。

FFT($10^6$): $270\text{ms}$

求逆: $33\text{ms}$

开根: $53\text{ms}$

对数函数: $50\text{ms}$

指数函数: $175\text{ms}$（太拉了）

除法（取模）: $50\text{ms}$

幂函数: $320\text{ms}$（太拉了）

注意更改 MAXN。使用前要调用 `init()`。

```cpp
namespace Poly
{
	const int MAXN=200000;
	int Shape,Invn[MAXN+10],R[MAXN*4+10],Prt[MAXN*4];
	inline void init()
	{
		Invn[0]=1;
		for(int i=1;i<=MAXN;++i)Invn[i]=Cmul(Invn[i-1],i);
		int tmp=power(Invn[MAXN],MOD-2);
		for(int i=MAXN;i>=1;--i)Invn[i]=Cmul(tmp,Invn[i-1]),Mmul(tmp,i);
	}
	inline int inv(int x){return x<=MAXN?Invn[x]:power(x,MOD-2);}
	inline void NTT(vi&A,int n,int opt)
	{
		static ull B[MAXN*4+10];ull iv=power(n,MOD-2);A.resize(n);
		for(int i=0;i<n;++i)B[i]=A[R[i]];
		for(int mid=1;mid<n;mid<<=1)
		{
			for(int j=0;j<n;j+=mid<<1)
			{
				for(int k=j;k<j+mid;++k)
				{
					ull x=B[k],y=Prt[mid+k-j]*B[k+mid]%MOD;
					B[k]=x+y,B[k+mid]=x+MOD-y;
				}
			}
		}
		if(opt)for(int i=0;i<n;++i)A[i]=B[i]%MOD;
		else{reverse(B+1,B+n);for(int i=0;i<n;++i)A[i]=Cmul(B[i]%MOD,iv);}
	}
	inline void init(int lim)
	{
		if(lim==Shape)return;
		int n=lim/2;Shape=lim;
		for(int i=0;i<lim;++i)R[i]=(R[i>>1]>>1)|((i&1)?n:0);
		for(int i=1;i<lim;i<<=1)
		{
			int wm=power(Root,(MOD-1)/(i<<1));Prt[i]=1;
			for(int j=1;j<i;++j)Prt[i+j]=Cmul(Prt[i+j-1],wm);
		}
	}
	inline vi del(vi A,vi B,int N=-1)
	{
		if(~N)A.resize(N+1),B.resize(N+1);
		int n=A.size()-1;
		for(int i=0;i<=n;++i)Mdel(A[i],B[i]);
		return A;
	}
	inline vi add(vi A,vi B,int N=-1)
	{
		if(~N)A.resize(N+1),B.resize(N+1);
		int n=A.size()-1;
		for(int i=0;i<=n;++i)Madd(A[i],B[i]);
		return A;
	}
	inline vi mul(vi A,int k,int N=-1)
	{
		if(~N)A.resize(N+1);
		int n=A.size()-1;
		for(int i=0;i<=n;++i)Mmul(A[i],k);
		return A;
	}
	inline vi inte(vi A,int N=-1)
	{
		if(~N)A.resize(N+1);
		int n=A.size();A.resize(n+1);
		for(int i=n;i>0;--i)A[i]=Cmul(A[i-1],inv(i));
		return A[0]=0,A;
	}
	inline vi der(vi A,int N=-1)
	{
		if(~N)A.resize(N+1);
		int n=A.size()-2;
		for(int i=0;i<=n;++i)A[i]=Cmul(A[i+1],i+1);
		return A.resize(n+1),A;
	}	
	inline vi FFT(vi A,vi B,int N1=-1,int N2=-1)
	{
		if(N1!=-1)A.resize(N1+1);if(N2!=-1)B.resize(N2+1);
		int n=A.size()-1,m=B.size()-1,N=1,len=0;
		while(N<=n+m)N<<=1,++len;
		init(N),NTT(A,N,1),NTT(B,N,1);
		for(int i=0;i<N;++i)A[i]=Cmul(A[i],B[i]);
		return NTT(A,N,0),A.resize(n+m+1),A;
	}
	inline vi inv(vi A,int N=-1)
	{
		if(N!=-1)A.resize(N+1);
		vi B={(int)power(A[0],MOD-2)};
		int n=A.size()-1,nn=n;
		for(n=1;n<=nn+1;n<<=1)
		{
			vi TB=B,C(n*2);init(n<<1);
			copy(A.begin(),A.begin()+min(nn+1,2*n),C.begin());
			NTT(C,n<<1,1),NTT(B,n<<1,1);
			for(int i=0;i<(n<<1);++i)C[i]=1ll*C[i]*B[i]%MOD;
			NTT(C,n<<1,0);C.resize(n<<1);fill(C.begin(),C.begin()+n,0),NTT(C,n<<1,1);
			for(int i=0;i<(n<<1);++i)B[i]=1ll*B[i]*(MOD-C[i])%MOD;
			NTT(B,n<<1,0);B.resize(n<<1),move(TB.begin(),TB.end(),B.begin());
		}
		return B.resize(nn+1),B;
	}
	inline vi ln(vi A,int N=-1){return inte(FFT(der(A,N),inv(A,N)));}
	inline vi exp(vi A,int N=-1)
	{
		if(~N)A.resize(N+1);
		int m=A.size()-1;static vi B,C;
		B.clear(),C.clear(),B.eb(1);
		for(int n=2;n<=m;n<<=1)B.resize(n+1),C=del(A,ln(B),n),Madd(C[0],1),B=FFT(B,C,n,n);
		C=del(A,ln(B),m),Madd(C[0],1);
		B.resize(m+1),C.resize(m+1);
		B=FFT(B,C,m,m),B.resize(m+1);
		return B;
	}
	inline vi div(vi A,vi B,int N1=-1,int N2=-1)
	{
		if(~N1)A.resize(N1+1);if(~N2)B.resize(N2+1);
		int n=A.size()-1,m=B.size()-1;
		reverse(A.begin(),A.end()),reverse(B.begin(),B.end());
		A=FFT(A,inv(B,n-m+1)),A.resize(n-m+1),reverse(A.begin(),A.end());
		return A;
	}
}
```