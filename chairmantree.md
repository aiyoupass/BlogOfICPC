# 主席树再次体验
挺高兴的┗|｀O′|┛ 嗷~~
写了一发就过了
```
#pragma GCC Optimize("2")
#include<bits/stdc++.h>
using namespace std;

const int N = 1e6 + 5;

struct Node {
	int v;
	Node *ls, *rs;
}pool[N * 20];
int cnt = 0;

Node *new_Node() {
	return pool + (cnt++);
}
void update(Node *r) {
	r->v = r->ls->v + r->rs->v;
}

void build(Node *rt, int l, int r, int a[]) {
	if (l == r) {
		rt->v = a[l];
		return ;
	}
	int mid = l + r >> 1;
	rt->ls = new_Node();
	rt->rs = new_Node();
	build(rt->ls, l, mid, a);
	build(rt->rs, mid + 1, r, a);
	update(rt);
}

Node *insert(Node *rt, Node *las, int l, int r, int x, int a) {
	if (l == r) {
		rt->v = a;
		return rt;
	}
	*rt = *las;
	int mid = l + r >> 1;
	if (x <= mid) {
		rt->ls = new_Node();
		insert(rt->ls, las->ls, l, mid, x, a);
	}
	else if (x > mid) {
		rt->rs = new_Node();
		insert(rt->rs, las->rs, mid + 1, r, x, a);
	}
	update(rt);
	return rt;
}

int query(Node *rt, int l, int r, int x) {
	if (l == r) return rt->v;
	int mid = l + r >> 1;
	if (x <= mid) {
		return query(rt->ls, l, mid, x);
	} else if (x > mid) {
		return query(rt->rs, mid + 1, r, x);
	}
	return 0;
}
int a[N];
Node* root[N];
int main () {
	int n, m;
	ios::sync_with_stdio(false);
	cin.tie(NULL);
	cout.tie(NULL);
	cin >> n >> m;

	for (int i = 1; i <= n; i += 1) {
		cin >> a[i];
	}
	root[0] = new_Node();
	build(root[0], 1, n, a);
	for (int i = 1; i <= m; i += 1) {
		int op, r, c, d;
		cin >> r >> op >> c;
		if (op == 1) {
			cin >> d;
			root[i] = new_Node();
			insert(root[i], root[r], 1, n, c, d);
		} else {
			cout << query(root[r], 1, n, c) << endl;
			root[i] = root[r];
		}
	}
	return 0;
}
```