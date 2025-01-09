```cpp
	namespace FWT
	{
		#define count(i) __builtin_popcount(i)
	    inline void OR(vi&F,int opt=1)
	    {
	        int n=F.size();
	        if(opt==1)for(int i=1;i<n;i<<=1)
	        for(int j=0;j<n;j+=i<<1)for(int k=j;k<j+i;++k)
	        Madd(F[k+i],F[k]);
	        else for(int i=1;i<n;i<<=1)
	        for(int j=0;j<n;j+=i<<1)for(int k=j;k<j+i;++k)
	        Mdel(F[k+i],F[k]);
	    }
	    inline void AND(vi&F,int opt=1)
	    {
	        int n=F.size();
	        if(opt==1)for(int i=1;i<n;i<<=1)
	        for(int j=0;j<n;j+=i<<1)for(int k=j;k<j+i;++k)
	        Madd(F[k],F[k+i]);
	        else for(int i=1;i<n;i<<=1)
	        for(int j=0;j<n;j+=i<<1)for(int k=j;k<j+i;++k)
	        Mdel(F[k],F[k+i]);
	    }
	    inline void XOR(vi&F,int opt=1)
	    {
	        int n=F.size(),x,y;
	        for(int k=1;k<n;k<<=1)for(int i=0;i<n;i+=k<<1)
	        for(int j=i;j<i+k;++j)x=F[j],y=F[j+k],F[j]=Cadd(x,y),F[j+k]=Cdel(x,y);
	        if(opt==-1){for(int i=0,t=power(n,MOD-2);i<n;++i)Mmul(F[i],t);}
	    }
	    inline vi OR(vi&F,vi&G)
	    {
	        assert(F.size()==G.size());
	        OR(F,1),OR(G,1);
	        vi H(F.size());
	        for(int i=0;i<(int)F.size();++i)H[i]=Cmul(F[i],G[i]);
	        return OR(H,-1),H;
	    }
	    inline vi AND(vi&F,vi&G)
	    {
	        assert(F.size()==G.size());
	        AND(F,1),AND(G,1);
	        vi H(F.size());
	        for(int i=0;i<(int)F.size();++i)H[i]=Cmul(F[i],G[i]);
	        return AND(H,-1),H;
	    }
	    inline vi XOR(vi&F,vi&G)
	    {
	        assert(F.size()==G.size());
	        XOR(F,1),XOR(G,1);
	        vi H(F.size());
	        for(int i=0;i<(int)F.size();++i)H[i]=Cmul(F[i],G[i]);
	        return XOR(H,-1),H;
	    }
	    inline vi MUL(vi&F,vi&G)
	    {
	        assert(F.size()==G.size());
	        static vi X[21],Y[21],Z[21],H;
	        int n=F.size(),m=__lg(n);
	        for(int i=0;i<=m;++i)X[i]=vi(n),Y[i]=vi(n),Z[i]=vi(n);
	        for(int i=0;i<n;++i)X[count(i)][i]=F[i];
	        for(int i=0;i<n;++i)Y[count(i)][i]=G[i];
	        for(int i=0;i<=m;++i)OR(X[i],1),OR(Y[i],1);
	        for(int x=0;x<=m;++x)for(int y=0;x+y<=m;++y)
	        for(int i=0;i<n;++i)Madd(Z[x+y][i],Cmul(X[x][i],Y[y][i]));
	        for(int i=0;i<=m;++i)OR(Z[i],-1);
	        H.resize(n);for(int i=0;i<n;++i)H[i]=Z[count(i)][i];
	        return H;
	    }
	    inline vi INV(vi&F)
	    {
	        int n=F.size(),m=__lg(n);
	        static vi X[21],Y[21],G;
	        for(int i=0;i<=m;++i)X[i]=vi(n),Y[i]=vi(n);
	        for(int i=0;i<n;++i)X[count(i)][i]=F[i];
	        for(int i=0;i<=m;++i)OR(X[i],1);
	        for(int i=0;i<n;++i)
	        {
	            Y[0][i]=power(X[0][i],MOD-2);
	            for(int j=1;j<=m;++j)
	            {
	                for(int k=0;k<j;++k)Mdel(Y[j][i],Cmul(Y[k][i],X[j-k][i]));
	                Mmul(Y[j][i],Y[0][i]);
	            }
	        }
	        for(int i=0;i<=m;++i)OR(Y[i],-1);
	        G.resize(n);for(int i=0;i<n;++i)G[i]=Y[count(i)][i];
	        return G;
	    }
	    inline vi LN(vi&F)
	    {
	        int n=F.size(),m=__lg(n),iv[21];
	        static vi X[21],Y[21],G;
	        for(int i=0;i<=m;++i)X[i]=vi(n),Y[i]=vi(n),iv[i]=power(i,MOD-2);
	        for(int i=0;i<n;++i)X[count(i)][i]=F[i];
	        for(int i=0;i<=m;++i)OR(X[i],1);
	        for(int i=0;i<n;++i)
	        {
	            Y[0][i]=power(X[0][i],MOD-2);
	            for(int j=1;j<=m;++j)
	            {
	                for(int k=0;k<j;++k)Mdel(Y[j][i],Cmul(Y[k][i],X[j-k][i]));
	                Mmul(Y[j][i],Y[0][i]);
	            }
	            for(int k=m;k>=0;--k)
	            {
	            	for(int j=1;j<m&&j+k<=m;++j)
	            	Madd(Y[j+k][i],Cmul(X[j+1][i],j+1,Y[k][i]));
	            	Mmul(Y[k][i],X[1][i]);
				}
	            for(int j=m;j>=1;--j)Y[j][i]=Cmul(iv[j],Y[j-1][i]);
	            Y[0][i]=0;
	        }
	        for(int i=0;i<=m;++i)OR(Y[i],-1);
	        G.resize(n);for(int i=0;i<n;++i)G[i]=Y[count(i)][i];
	        return G;
	    }
	    inline vi EXP(vi&F)
	    {
	        int n=F.size(),m=__lg(n),iv[21];
	        static vi X[21],Y[21],G;
	        for(int i=0;i<=m;++i)X[i]=vi(n),Y[i]=vi(n),iv[i]=power(i,MOD-2);
	        for(int i=0;i<n;++i)X[count(i)][i]=F[i];
	        for(int i=0;i<=m;++i)OR(X[i],1);
	        for(int i=0;i<n;++i)
	        {
	            Y[0][i]=1;
	            for(int j=0;j<m;++j)
	            {
	                for(int k=0;k<=j;++k)
	                Madd(Y[j+1][i],Cmul(X[k+1][i],k+1,Y[j-k][i]));
	                Mmul(Y[j+1][i],iv[j+1]);
	            }
	        }
	        for(int i=0;i<=m;++i)OR(Y[i],-1);
	        G.resize(n);for(int i=0;i<n;++i)G[i]=Y[count(i)][i];
	        return G;
	    }
	}
	using namespace FWT;
```