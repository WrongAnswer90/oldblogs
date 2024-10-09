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
	inline vi fix(vi A,int n){return A.resize(n),A;}
	const int MAXN=2000000;
	int Shape,VBL,Invn[MAXN],R[MAXN<<1],Prt[MAXN<<1],P0[50010],P1[50010];
	inline void init()
	{
		P0[0]=P1[0]=1,VBL=ceil(sqrt(MOD)),Invn[0]=1;
		for(int i=1;i<MAXN;++i)Invn[i]=Cmul(Invn[i-1],i);
		int tmp=power(Invn[MAXN-1],MOD-2);
		for(int i=MAXN-1;i>=1;--i)Invn[i]=Cmul(tmp,Invn[i-1]),Mmul(tmp,i);
		for(int i=1;i<=VBL;++i)P0[i]=Cmul(P0[i-1],Root);
		for(int i=1;i<=VBL;++i)P1[i]=Cmul(P1[i-1],P0[VBL]);
	}
	inline int powerr(int x){return Cmul(P0[x%VBL],P1[x/VBL]);}
	inline void NTT(vi&A,int n,int opt)
	{
		static ull B[MAXN<<1],iv;
		A.resize(n);
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
		else
		{
			reverse(B+1,B+n),iv=power(n,MOD-2);
			for(int i=0;i<n;++i)A[i]=Cmul(B[i]%MOD,iv);
		}
	}
	inline void init(int lim)
	{
		if(lim==Shape)return;
		int n=lim/2;
		for(int i=0;i<lim;++i)R[i]=(R[i>>1]>>1)|((i&1)?n:0);
		for(int i=1;i<lim;i<<=1)
		{
			int wm=powerr((MOD-1)/(i<<1));Prt[i]=1;
			for(int j=1;j<i;++j)Prt[i+j]=Cmul(Prt[i+j-1],wm);
		}
		Shape=lim;
	}
	inline vi der(vi A)
	{
		int N=A.size();
		vi B(N-1);
		for(int i=0;i<N-1;++i)B[i]=Cmul(i+1,A[i+1]);
		return B;
	}
	inline vi inte(vi A)
	{
		int N=A.size();
		vi B(N+1);
		for(int i=1;i<=N;++i)B[i]=Cmul(A[i-1],power(i,MOD-2));
		return B;
	}
	inline vi FFT(vi A,vi B)
	{
		int n=A.size(),m=B.size(),N=1;
		while(N<=n+m)N<<=1;
		init(N),NTT(A,N,1),NTT(B,N,1);
		for(int i=0;i<N;++i)A[i]=Cmul(A[i],B[i]);
		return NTT(A,N,0),A.resize(n+m-1),A;
	}
	inline vi inv(vi A)
	{
		static vi B,TB,C;
		B={(int)power(A[0],MOD-2)};
		int N=A.size();
		for(int n=1;n<N;n<<=1)
		{
			TB=B,C.clear(),C.resize(n<<1);
			copy(A.begin(),A.begin()+min(N,2*n),C.begin());
			init(n<<1);
			NTT(C,n<<1,1),NTT(B,n<<1,1);
			for(int i=0;i<(n<<1);++i)Mmul(C[i],B[i]);
			NTT(C,n<<1,0);
			fill(C.begin(),C.begin()+n,0);
			NTT(C,n<<1,1);
			for(int i=0;i<(n<<1);++i)Mmul(B[i],MOD-C[i]);
			NTT(B,n<<1,0);
			move(TB.begin(),TB.end(),B.begin());
		}
		return B.resize(N),B;
	}
	inline vi sqrt(vi A)
	{
		static vi G,F,TG,TF,TA;
		int n=1,N=A.size();
		G={1};
		for(;n<N;n<<=1)
		{
			TA.resize(n<<1);
			TG=G,TG.resize(n<<1);
			G.resize(n);
			copy(A.begin(),A.begin()+min(N,n),TA.begin());
			init(n<<1);
			NTT(TA,n<<1,1),NTT(TG,n<<1,1);
			for(int i=0;i<(n<<1);++i)TG[i]=Cdel(TG[i],Cmul(TA[i],TG[i],TG[i],TG[i]));
			NTT(TG,n<<1,0);
			if(n>1)for(int i=n>>1;i<n;++i)G[i]=Cmul(TG[i],inv2);
		}
		G.resize(n),F.resize(n),NTT(G,n,1);
		for(int i=0;i<n;++i)F[i]=Cmul(G[i],TA[i]);
		NTT(F,n,0);
		for(int i=n>>1;i<n;++i)F[i]=0;
		TF=F,NTT(TF,n,1);
		A.resize(n),NTT(A,n,1);
		for(int i=0;i<n;++i)TF[i]=Cmul(G[i],Cdel(A[i],Cmul(TF[i],TF[i])));
		NTT(TF,n,0);
		for(int i=n>>1;i<N;++i)F[i]=Cmul(TF[i],inv2);
		return F.resize(N),F;
	}
    inline vi ln(vi A)
	{
		int N=A.size();
		A=FFT(der(A),inv(A));
		return inte(fix(A,N-1));
	}
	inline vi exp(vi A)
	{
		static vi B,C,D,TB;
		int N=A.size();B={1};
		for(int n=1;n<N;n<<=1)
		{
			TB=C=B,C.resize(n<<1),B.resize(n<<1);
			D=ln(B),Mdel(D[0],1);
			for(int i=0;i<min(n<<1,N);++i)D[i]=Cdel(A[i],D[i]);
			for(int i=N;i<(n<<1);++i)D[i]=MOD-D[i];
			init(n<<1),NTT(D,n<<1,1),NTT(C,n<<1,1);
			for(int i=0;i<(n<<1);++i)Mmul(D[i],C[i]);
			NTT(D,n<<1,0),B=D,copy(TB.begin(),TB.end(),B.begin());
		}
		return B.resize(N),B;
	}
	inline pair<vi,vi> div(vi A,vi B)
	{
		int n=A.size(),m=B.size();
		static vi C,D;
		reverse(A.begin(),A.end());
		reverse(B.begin(),B.end());
		D=FFT(A,inv(fix(B,n-m+1))),D.resize(n-m+1);
		reverse(D.begin(),D.end());
		reverse(A.begin(),A.end());
		reverse(B.begin(),B.end());
		C=FFT(B,D);
		for(int i=0;i<m;++i)C[i]=Cdel(A[i],C[i]);
		return mp(D,fix(C,m-1));
	}
}
```