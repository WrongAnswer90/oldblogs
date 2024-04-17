```cpp
inline void AND(vector<int>&ve,int opt=1)
{
    int n=ve.size();
    if(opt==1)
    for(int k=2;k<=n;k<<=1)
    {
        for(int i=0;i<n;i+=k)
        {
            for(int j=i;j<i+(k>>1);++j)
            Madd(ve[j],ve[j+(k>>1)]);
        }
    }
    else
    for(int k=2;k<=n;k<<=1)
    {
        for(int i=0;i<n;i+=k)
        {
            for(int j=i;j<i+(k>>1);++j)
            Mdel(ve[j],ve[j+(k>>1)]);
        }
    }
}
inline void OR(vector<int>&ve,int opt=1)
{
    int n=ve.size();
    if(opt==1)
    for(int k=2;k<=n;k<<=1)
    {
        for(int i=0;i<n;i+=k)
        {
            for(int j=i;j<i+(k>>1);++j)
            Madd(ve[j+(k>>1)],ve[j]);
        }
    }
    else
    for(int k=2;k<=n;k<<=1)
    {
        for(int i=0;i<n;i+=k)
        {
            for(int j=i;j<i+(k>>1);++j)
            Mdel(ve[j+(k>>1)],ve[j]);
        }
    }
}
inline void XOR(vector<int>&ve,int opt=1)
{
    int n=ve.size();
    for(int k=2;k<=n;k<<=1)
    {
        for(int i=0;i<n;i+=k)
        {
            for(int j=i;j<i+(k>>1);++j)
            {
                Madd(ve[j],ve[j+(k>>1)]);
                ve[j+(k>>1)]=Cdel(ve[j],ve[j+(k>>1)],ve[j+(k>>1)]);
            }
        }
    }
    int tmp=power(inv2,__lg(n));
    if(opt==-1)for(int i=0;i<n;++i)Mmul(ve[i],tmp);
}
```