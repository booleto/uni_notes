$$
f(n) = O(cg(n))
$$
sao cho: $\exists c, \forall n > n_0 : f(n) < cg(n)$

### Các quy tắc:
1. $T(n) = O(cg(n)) = O(g(n))$
2. $T_1(n) = O(g_1(n))$
   $T_2(n) = O(g_2(n))$
   $T_1(n) + T_2(n) = O(g_1(n), g_2(n)) = O(max(g_1(n), g_2(n)))$
