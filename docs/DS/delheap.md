```cpp
template<class Node>
struct Delheap
{
	priority_queue<Node> p,q;
	inline void push(int x){q.e(x);}
	inline void erase(int x){p.e(x);}
	inline void upd()
	{
		while(p.size()&&q.size()&&!(p.top()<q.top())&&!(q.top()<p.top()))
		q.pop(),p.pop();
	}
	inline void pop(){return upd(),q.top();}
	inline int size(){return p.size()-q.size();}
	inline int top(){return upd(),q.top();}
};
```
