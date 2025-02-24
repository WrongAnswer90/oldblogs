```cpp
    namespace Poly
    {
        inline vi fix(vi A,int n){return A.resize(n),A;}
        const int MAXN=2000000;
        int Shape,R[MAXN<<1],Prt[MAXN<<1];
        inline int powerr(int x){return power(Root,x);}
        ull B[MAXN<<1];
        inline void NTT(vi&A,int n,int opt)
        {
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
            if(opt==1)for(int i=0;i<n;++i)A[i]=B[i]%MOD;
            else
            {
                reverse(B+1,B+n);int iv=power(n,MOD-2);
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
            int N=A.size();vi B(N-1);
            for(int i=0;i<N-1;++i)B[i]=Cmul(i+1,A[i+1]);
            return B;
        }
        inline vi inte(vi A)
        {
            int N=A.size();vi B(N+1);
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
            int N=A.size();
            B={(int)power(A[0],MOD-2)};
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
            reverse(all(A)),reverse(all(B));
            D=FFT(A,inv(fix(B,n-m+1))),D.resize(n-m+1);
            reverse(all(D)),reverse(all(A)),reverse(all(B));
            C=FFT(B,D);
            for(int i=0;i<m;++i)C[i]=Cdel(A[i],C[i]);
            return mp(D,fix(C,m-1));
        }
        inline int bostan_mori(vi P,vi Q,int n)
        {
            vi rQ;
            int N=1;
            while(N<=(int)P.size()*2)N<<=1;
            init(N);
            auto reduce=[&](vi&A,int opt)->void
            {
                for(int i=opt;i<(int)A.size();i+=2)A[i>>1]=A[i];
                A.resize((A.size()+1-opt)>>1);
            };
            while(n)
            {
                rQ=Q;
                for(int i=1;i<(int)Q.size();i+=2)rQ[i]=Cdel(0,rQ[i]);
                NTT(rQ,N,1),NTT(P,N,1),NTT(Q,N,1);
                for(int i=0;i<N;++i)Mmul(P[i],rQ[i]),Mmul(Q[i],rQ[i]);
                NTT(P,N,0),NTT(Q,N,0);
                reduce(Q,0),reduce(P,n&1);
                n>>=1;
            }
            if(P.empty())return 0;
            return Cmul(P[0],power(Q[0],MOD-2));
        }
        inline int CCHIR(vi P,vi Q,int n)
        {
            int m=Q.size();
            Q.eb(0);
            for(int j=m;j;--j)Q[j]=Q[j-1];
            Q[0]=MOD-1;
            return bostan_mori(fix(FFT(P,Q),m),Q,n);
        }
        inline vi BM(vi A)
        {
            vi lst,ans;
            int p=-1,delta=0;
            for(int i=0;i<(int)A.size();++i)
            {
                int tmp=A[i];
                for(int j=0;j<(int)ans.size();++j)
                Mdel(tmp,Cmul(A[i-1-j],ans[j]));
                if(!tmp)continue;
                if(p==-1)
                {
                    p=i,delta=tmp;
                    ans.resize(i+1);
                    continue;
                }
                vi now=ans;
                int mul=Cmul(tmp,power(delta,MOD-2));
                if((int)ans.size()<(int)lst.size()+i-p)
                ans.resize((int)lst.size()+i-p);
                Madd(ans[i-p-1],mul);
                for(int j=0;j<(int)lst.size();++j)
                Mdel(ans[i-p+j],Cmul(mul,lst[j]));
                if((int)now.size()-i<(int)lst.size()-p)
                lst=now,p=i,delta=tmp;
            }
            return ans;
        }
        vi IFFT(vi F,vi G)
        {
            int n=F.size();
            reverse(F.begin(),F.end());
            F=fix(FFT(F,G),n);
            reverse(F.begin(),F.end());
            return F;
        }
        vi _Q[400010],_iQ[400010];
        vi MultiPoints(vi F,vi X)
        {
            int n=max(F.size(),X.size()),m=X.size();
            F.resize(n),X.resize(n);
            vi ans(n);
            auto Prework=[&](auto Prework,int id,int l,int r)->void
            {
                if(l==r)
                {
                    _Q[id]={1,Cdel(0,X[l])};
                    return;
                }
                int mid=(l+r)>>1;
                Prework(Prework,id<<1,l,mid);
                Prework(Prework,id<<1|1,mid+1,r);
                int N=1;
                while(N<=r-l+1)N<<=1;
                init(N);
                _iQ[id<<1]=_Q[id<<1],NTT(_iQ[id<<1],N,1);
                _iQ[id<<1|1]=_Q[id<<1|1],NTT(_iQ[id<<1|1],N,1);
                _Q[id].resize(N);
                for(int i=0;i<N;++i)_Q[id][i]=Cmul(_iQ[id<<1][i],_iQ[id<<1|1][i]);
                NTT(_Q[id],N,0);
            };
            auto solve=[&](auto solve,int id,int l,int r,vi tmp)->void
            {
                tmp.resize(r-l+1);
                if(l==r)return ans[l]=tmp[0],void();
                int mid=(l+r)>>1,N=1;
                while(N<=r-l+1)N<<=1;
                init(N);
                tmp.resize(N);
                reverse(all(tmp));
                NTT(tmp,N,1);
                vi A(N),B(N);
                for(int i=0;i<N;++i)A[i]=Cmul(tmp[i],_iQ[id<<1|1][i]);
                for(int i=0;i<N;++i)B[i]=Cmul(tmp[i],_iQ[id<<1][i]);
                NTT(A,N,0),NTT(B,N,0);
                A.resize(N),B.resize(N);
                reverse(all(A)),reverse(all(B));
                solve(solve,id<<1,l,mid,A);
                solve(solve,id<<1|1,mid+1,r,B);
            };
            Prework(Prework,1,0,n-1);
            solve(solve,1,0,n-1,IFFT(F,inv(_Q[1])));
            ans.resize(m);
            return ans;
        }
        vi _P[400010],_iP[400010];
        vi InterPolate(vi X,vi Y)
        {
            int n=X.size();
            auto Prework=[&](auto Prework,int id,int l,int r)->void
            {
                if(l==r)return _iP[id]=_P[id]={Cdel(0,X[l]),1},void();
                int mid=(l+r)>>1,N=1;
                Prework(Prework,id<<1,l,mid);
                Prework(Prework,id<<1|1,mid+1,r);
                while(N<=r-l+1)N<<=1;
                init(N);
                NTT(_iP[id<<1],N,1);
                NTT(_iP[id<<1|1],N,1);
                _P[id].resize(N);
                for(int i=0;i<N;++i)
                _P[id][i]=Cmul(_iP[id<<1][i],_iP[id<<1|1][i]);
                NTT(_P[id],N,0);
                _P[id].resize(r-l+2);
                _iP[id]=_P[id];
            };
            Prework(Prework,1,0,n-1);
            vi D=MultiPoints(der(_P[1]),X);
            auto solve=[&](auto solve,int id,int l,int r)->vi
            {
                if(l==r)return {Cmul(Y[l],power(D[l],MOD-2))};
                int mid=(l+r)>>1,N=1;
                vi A=solve(solve,id<<1,l,mid);
                vi B=solve(solve,id<<1|1,mid+1,r);
                while(N<=r-l+1)N<<=1;
                init(N);
                NTT(A,N,1),NTT(B,N,1);
                for(int i=0;i<N;++i)
                {
                    Mmul(A[i],_iP[id<<1|1][i]);
                    Mmul(B[i],_iP[id<<1][i]);
                }
                NTT(A,N,0),NTT(B,N,0);
                A.resize(r-l+1),B.resize(r-l+1);
                for(int i=0;i<r-l+1;++i)Madd(A[i],B[i]);
                return A;
            };
            vi T=solve(solve,1,0,n-1);
            return solve(solve,1,0,n-1);
        }
    }
```