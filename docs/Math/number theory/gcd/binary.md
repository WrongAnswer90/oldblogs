```cpp
inline int gcd(int a,int b)
{
	int az=__builtin_ctz(a),bz=__builtin_ctz(b);
	int z=min(az,bz),tmp;b>>=bz;
	while(a)a>>=az,tmp=a-b,az=__builtin_ctz(tmp),b=min(a,b),a=abs(tmp);
	return b<<z;
}
```