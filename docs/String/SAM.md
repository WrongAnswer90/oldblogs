```cpp
	struct SAM
	{
		int lst,cnt;
		struct{int ch[26],fa,len;}t[150010];
		inline void add(int x)
		{
			int p=lst,np,q,nq;
			if((q=t[p].ch[x]))
			{
				if(t[q].len==t[p].len+1)return lst=q,void();
				t[lst=nq=++cnt]=t[q],t[nq].len=t[p].len+1,t[q].fa=t[np].fa=nq;
				while(t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
				return;
			}
			t[lst=np=++cnt].len=t[p].len+1;
			while(!t[p].ch[x])t[p].ch[x]=np,p=t[p].fa;
			if(t[q=t[p].ch[x]].len==t[p].len+1)return t[np].fa=q,void();
			t[nq=++cnt]=t[q],t[nq].len=t[p].len+1,t[q].fa=t[np].fa=nq;
			while(t[p].ch[x]==q)t[p].ch[x]=nq,p=t[p].fa;
		}
		SAM()
		{
			lst=cnt=1,t[0].len=-1;
			for(int i=0;i<26;++i)t[0].ch[i]=1;
		}
	};
```