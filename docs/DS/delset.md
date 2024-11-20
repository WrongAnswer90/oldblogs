```cpp
	const int N=100000;
	struct Delset
	{
		int t[N+10];
		int top,st[N+10];
		void push(int x){st[++top]=x;}
		void erase(int x){++t[x];}
		void upd(){while(top&&t[st[top]])--t[st[top--]];}
		int get(){return upd(),st[top];}
	};
```