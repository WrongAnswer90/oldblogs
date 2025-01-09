```cpp
    int P,iP;
    inline void init(int _P){P=_P,iP=(__int128(1)<<64)/P+1;}
    inline int Cmod(ll x){return x-(__int128(x)*iP>>64)*P;}
```