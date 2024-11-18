## Lucas 定理

```cpp
    inline int power(int x,int y)
    {
        int s=1;
        for(;y;y>>=1,x=1ll*x*x%p))if(y&1)s=1ll*s*x%p;
        return s;
    }
    inline int CC(int n,int m){return n-m<0||n<0?0:1ll*fr[n]*inv[m]%p*inv[n-m]%p;}
    inline int C(int n,int m)
    {
        if(m==0)return 1;
        return 1ll*C(n/p,m/p)*CC(n%p,m%p)%p;
    }
    inline void init()
    {
        for(int i=1;i<p;++i)fr[i]=1ll*fr[i-1]*i%p;
        inv[p-1]=power(fr[p-1],p-2);
        for(int i=p-2;i>=1;--i)inv[i]=1ll*inv[i+1]*(i+1)%p;
    }
```

## exLucas 定理

```cpp
    inline int power(int x,int y,int p)
    {
        int s=1;
        for(;y;y>>=1,x=x*x%p)if(y&1)s=s*x%p;
        return s;
    }
    int exgcd(int a,int b,int &x,int &y)
    {
        if(!b)return x=1,y=0,a;
        int d=exgcd(b,a%b,x,y);
        int z=x;x=y,y=z-x*(a/b);
        return d;
    }
    inline int inv(int x,int p){int X,Y;return exgcd(x,p,X,Y),(X%p+p)%p;}
    int n,m,p,fr[1000001];
    int f(int k,int p,int P){if(!k)return 1;return f(k/p,p,P)*power(fr[P],k/P,P)%P*fr[k%P]%P;}
    int g(int k,int p)
    {
        int s=0,P=p;
        for(;p<=k;p=p*P)s+=k/p;
        return s;
    }
    int calc(int p,int c)
    {
        int P=power(p,c,1000000);fr[0]=1;
        for(int i=1;i<=P;++i)i%p==0?fr[i]=fr[i-1]:fr[i]=fr[i-1]*i%P;
        int up=f(n,p,P),down=f(m,p,P)*f(n-m,p,P)%P,et=g(n,p)-g(m,p)-g(n-m,p);
        return up*inv(down,P)%P*power(p,et,P)%P;
    }
    inline int calc()
    {
        int M=1,x0=0;
        for(int i=2;i*i<=p;++i)
        {
            if(p%i==0)
            {
                int s=0,t,d,p1,X,Y;
                for(;!(p%i);p/=i,++s);
                t=calc(i,s);
                p1=power(i,s,1000000),t=((calc(i,s)-x0)%p1+p1)%p1;
                d=exgcd(M,p1,X,Y),X=(X%p1+p1)%p1;
                X=t/d*X,x0=(x0+M*X)%(M*p1),M=M*p1;
            }
        }
        if(p!=1)
        {
            int s=1,i=p,t,d,p1,X,Y;
            p1=power(i,s,1000000),t=((calc(i,s)-x0)%p1+p1)%p1;
            d=exgcd(M,p1,X,Y),X=(X%p1+p1)%p1;
            X=t/d*X,x0=(x0+M*X)%(M*p1),M=M*p1;
        }
        return x0;
    }
```