```cpp
const int N=100000;
struct Delset
{
    int t[N+10],del[N+10];
    int top,st[N+10];
    void push(int x){++t[x],st[++top]=x;}
    void erase(int x){--t[x],++del[x];}
    void upd(){while(top>0&&del[st[top]])--del[st[top]],--top;}
    int get(){return st[top];}
};
```