# 2022广西省赛F题
题意大致就是颜色序列，三种操作，单点修改颜色，询问区间内颜色出现偶数次的次数。（共1000种颜色）

只需要记录这个颜色是出现了奇数次还是偶数次。

使用线段树，每个节点维护的是区间内各种颜色出现次数的奇偶性。两个区间的合并相当于是异或操作。

所以线段树的节点是一个1000大小的`bitset`。
代码如下
```
#include <iostream>
#include <bits/stdc++.h>

struct Node {
	std::bitset<1001> col;
	Node *ls, *rs;
};

void build(Node *rt, int l, int r, int col[]) {
	if (l == r) {
		rt->col[col[l]] = 1;
		return;
	}
	int mid = l + r >> 1;
	rt->ls = new Node();
	rt->rs = new Node();
	build(rt->ls, l, mid, col);
	build(rt->rs, mid + 1, r, col);
	rt->col = rt->ls->col ^ rt->rs->col;
}

void modf(Node* rt, int l, int r, int p, int x, int pre) {
	if (l == r) {
		rt->col[x] = rt->col[x] ^ 1;
		rt->col[pre] = rt->col[pre] ^ 1;
		return;
	}
	int mid = l + r >> 1;
	if (p <= mid) modf(rt->ls, l, mid, p, x, pre);
	if (p >  mid) modf(rt->rs, mid + 1, r, p, x, pre);
	rt->col = rt->ls->col ^ rt->rs->col;
}
std::bitset<1001> query(Node* rt, int l, int r, int L, int R) {
	if (l >= L and r <= R) {
		return rt->col;
	}
	int mid = l + r >> 1;
	std:: bitset<1001>res;
	if (L <= mid) res ^= query(rt->ls, l, mid, L, R);
	if (R >  mid) res ^= query(rt->rs, mid + 1, r, L, R);
	return res;
}
const int N = 100001;
int a[N];
int main() {
	int n, m;
	std:: cin >> n >> m;
	for (int i = 1; i <= n; i += 1) std:: cin >> a[i];
	auto root = new Node();
	build(root, 1, n, a);
	while (m--) {
		int opt, l, r;
		std:: cin >> opt >> l >> r;
		if (opt == 0) {
			modf(root, 1, n, l, r, a[l]);
		} else if (opt == 1) {
			auto gg = query(root, 1, n, l, r);
			std::cout << ((gg.count() == 1000 or gg.count() == 0) ? 1 : 0) << std:: endl;
		} else if (opt == 2) {
			auto gg = query(root, 1, n, l, r);
			std::cout << gg.count() << std:: endl;
		}
	}
	return 0;
}
```