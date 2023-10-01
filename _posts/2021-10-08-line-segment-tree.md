---
layout: post
title: line segment tree
subtitle: 
gh-repo: breylaude/blog
gh-badge: [star, fork, follow]
tags: [cp, dp]
comments: true
---


Line segment tree, aka Segment Tree Beats! (Angel Beats? = =), a support $O(nlog2n)$ $O(nlog2n)$ `interval min/max`. The potential energy segment tree.

To take min for example, each node maintains the maximum value and the number of occurrences of the maximum value and the strict second largest value (that is, not equal to the maximum value). If the modified value is less than the second largest value of the current interval, it will recurse downward, otherwise if it is less than the current interval maximum value, it will replace the maximum value with the modified value. This complexity is $O(nlogn)O(nlogn)$。

Amortized analysis proves: set the potential energy function of a node on the line segment tree Phi is the number of essentially different numbers in the interval, and each recursion must make $Phi−1Phi−1$, nodes not completely covered by the modified interval $Phi Phiplus$ one at most, the total complexity is $O(nlogn)O(nlogn)$。

The main idea is, marking and processing are different from ordinary line segment trees, which is that "give the largest", "give the second largest" or "give the largest in history" and "give the second largest in history" should be separated.

The interval sum can be maintained.

It's not difficult to implement.

Template :

Request support:

Modification: add interval, take intervalminQuery: interval sum, intervalmax, interval historymax

You can't maintain the interval sum with the "V" question, so you can only go to Beats.

Here's the code:

```cpp
#include <bits/stdc++.h>
#define rep(i, x, y) for (int i = x; i <= y; i++)
using namespace std;

typedef long long ll;
const int N = 5e5 + 10;
int n, m, a[N];

void rd(int &x) {
    x = 0; int f = 1; char ch = getchar();
    for (; !isdigit(ch); ch = getchar()) if (ch == '-') f = -1;
    for (; isdigit(ch); ch = getchar()) x = (x << 1) + (x << 3) + ch - '0';
    x *= f;
}

namespace SGTBeats {
    #define inf 1e18
    #define mid (l + r >> 1)
    #define ls (x << 1)
    #define rs (x << 1 | 1)

    void cmax(ll &x, ll y) { x = max(x, y); }

    struct atom {
        int l, r;
        ll mxa, se, cnt, mxb, sum;
        ll add_a;  
        ll add_b;  
        ll add_a1;  
        ll add_b1;  
    } tr[N << 2], t;

    void upd(int x) {
        tr[x].sum = tr[ls].sum + tr[rs].sum;
        tr[x].mxa = max(tr[ls].mxa, tr[rs].mxa);
        tr[x].mxb = max(tr[ls].mxb, tr[rs].mxb);
        if (tr[ls].mxa == tr[rs].mxa) {
            tr[x].cnt = tr[ls].cnt + tr[rs].cnt;
            tr[x].se = max(tr[ls].se, tr[rs].se);
        } else {
            tr[x].cnt = (tr[ls].mxa > tr[rs].mxa ? tr[ls].cnt : tr[rs].cnt);
            tr[x].se = (tr[ls].mxa > tr[rs].mxa ? max(tr[rs].mxa, tr[ls].se) : max(tr[ls].mxa, tr[rs].se));
        }
    }
    void modi(int x, ll add_a, ll add_b, ll add_a1, ll add_b1) {
        t = tr[x];
        t.sum += add_a * tr[x].cnt + add_a1 * (tr[x].r - tr[x].l + 1 - tr[x].cnt);
        t.mxa += add_a;
        cmax(t.mxb, tr[x].mxa + add_b);
        t.add_a += add_a;
        t.add_a1 += add_a1;
        cmax(t.add_b, tr[x].add_a + add_b);
        cmax(t.add_b1, tr[x].add_a1 + add_b1);
        if (t.se != -inf) t.se += add_a1;
        tr[x] = t;
    }
    void psd(int x) {
        int mxn = max(tr[ls].mxa, tr[rs].mxa);
        if (tr[ls].mxa == mxn)
            modi(ls, tr[x].add_a, tr[x].add_b, tr[x].add_a1, tr[x].add_b1);
        else
            modi(ls, tr[x].add_a1, tr[x].add_b1, tr[x].add_a1, tr[x].add_b1);
        if (tr[rs].mxa == mxn) 
            modi(rs, tr[x].add_a, tr[x].add_b, tr[x].add_a1, tr[x].add_b1);
        else
            modi(rs, tr[x].add_a1, tr[x].add_b1, tr[x].add_a1, tr[x].add_b1);
        tr[x].add_a = tr[x].add_a1 = tr[x].add_b = tr[x].add_b1 = 0;
    }
    void build(int x, int l, int r) {
        tr[x].l = l, tr[x].r = r;
        if (l == r) {
            tr[x].mxa = tr[x].mxb = tr[x].sum = a[l];
            tr[x].se = -inf;
            tr[x].cnt = 1;
            return;
        }
        build(ls, l, mid), build(rs, mid + 1, r);
        upd(x);
    }
    void modi_add(int x, int l, int r, int lx, int rx, int v) {
        if (lx <= l && r <= rx) {
            modi(x, v, v, v, v);
            return;
        }
        psd(x);
        if (lx <= mid) modi_add(ls, l, mid, lx, rx, v);
        if (rx > mid) modi_add(rs, mid + 1, r, lx, rx, v);
        upd(x);
    }
    void modi_min(int x, int l, int r, int lx, int rx, int v) {
        if (lx > r || rx < l || v >= tr[x].mxa) return;
        if (lx <= l && r <= rx && v > tr[x].se) {
            modi(x, v - tr[x].mxa, v - tr[x].mxa, 0, 0);
            return;
        }
        psd(x);
        modi_min(ls, l, mid, lx, rx, v);
        modi_min(rs, mid + 1, r, lx, rx, v);
        upd(x);
    }
    ll qry(int x, int l, int r, int lx, int rx, int op) {
        if (lx > r || rx < l) return op == 3 ? 0 : -inf;
        if (lx <= l && r <= rx) return op == 3 ? tr[x].sum : op == 4 ? tr[x].mxa : tr[x].mxb;
        psd(x);
        if (op == 3) return qry(ls, l, mid, lx, rx, op) + qry(rs, mid + 1, r, lx, rx, op);
        else return max(qry(ls, l, mid, lx, rx, op), qry(rs, mid + 1, r, lx, rx, op));
    }
};
using namespace SGTBeats;

int main() {
    rd(n), rd(m);
    rep(i, 1, n) rd(a[i]);
    build(1, 1, n);
    int op, l, r, v;
    while (m--) {
        rd(op), rd(l), rd(r);
        if (op == 1) {
            rd(v), modi_add(1, 1, n, l, r, v);
        } else if (op == 2) {
            rd(v), modi_min(1, 1, n, l, r, v);
        } else {
            printf("%lld\n", qry(1, 1, n, l, r, op));
        }
    }
    return 0;
}
```
