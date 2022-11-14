# Tổ hợp

### Định nghĩa
Một tổ hợp chập k của n là 1 cách chọn (không có thứ tự) k phần tử từ 1 tập n phần tử
-> Số tập con k phần tử của 1 tập hợp n phần tử là: $C_n^k$

### Định lý
Số các tổ hợp chập $k$ của $n$
$$
C^k_n = \frac{n(n-1)\cdots(n-k+1)}{k!}
$$

### Chứng minh
Với mỗi tổ hợp chập $k$ của $n$, nếu sắp xếp có thứ tự $k$ phần tử thì với mỗi tổ hợp chập $k$ của $n$ sẽ có $k!$ chỉnh hợp chập $k$ của $n$

### Một số tính chất
- Định lý:
$$
C^k_n = C^{n-k}_n
$$
Chứng minh: Chọn $k$ phần tử <-> Chọn $n-k$ phần tử để loại đi

- Hằng đẳng thức Pascal:
$$
C^k_n = C^k_{n-1} + C^{k-1}_{n-1}
$$
- Hằng đẳng thức Vandermonde
$$
C^r_{m+n}= \sum\limits_{r=0}^{k} C_m^{k-r} \, C_n^r \quad
(k < m < n)
$$

Chứng minh:
Xét tập $A$ có $m$ phần tử, $B$ có $n$ phần tử
Chọn $0$ phần tử từ $A$, $k$ từ $B$ -> $C^k_n$
Chọn $1$ phần tử từ $A$, $k - 1$ từ $B$ -> $C^{k-1}_n+1$
$\vdots$

### Hệ số nhị thức
$$(x+y)^n = \sum\limits_{k=0}^n C^k_n \, x^k \, y^{n-k}$$

Định lý:
$$2^n = \sum\limits_{k=0}^n C^k_n$$