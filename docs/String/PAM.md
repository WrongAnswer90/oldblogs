```cpp
	char s[500010];
	struct PAM
	{
		int cnt,lst,L;
		struct{int len,val,fa,ch[26];}t[1000010];
		PAM(){cnt=1,t[0].fa=1,t[1].len=-1;}
		inline int newnode(int x){t[++cnt].len=x;return cnt;}
		inline int get(int x){while(s[L]!=s[L-t[x].len-1])x=t[x].fa;return x;}
		inline void ins(int x)
		{
			++L;
			int p=get(lst),nw;
			if(!t[p].ch[x])
			{
				nw=newnode(t[p].len+2);
				t[nw].fa=t[get(t[p].fa)].ch[x];
				t[p].ch[x]=nw;
				t[nw].val=t[t[nw].fa].val+1;
			}
			else nw=t[p].ch[x];
			lst=nw;
		}
	}t;
```