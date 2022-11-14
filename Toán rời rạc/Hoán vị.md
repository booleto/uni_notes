# Hoán vị

VD:
- Có bao nhiêu cách xếp 3 bạn khác nhau vào 3 chỗ ngồi khác nhau
- Có bao nhiêu các số tự nhiên khác nhau có các chữ số khác nhau, gồm các số 1, 2, 3

### Định nghĩa
1 hoán vị của tập $X$ là 1 cách sắp xếp các phần tử của $X$ theo 1 thứ tự nào đó
1 hoán vị của $\{1, 2, 3, \dots, n\}$ được gọi là 1 hoán vị của $n$

### Định lý
Số các hoán vị của 1 tập n phần tử (Số các hoán vị của n) là:
$$
n \cdot (n-1) \cdot (n-2) \cdots 2 \cdot 1
$$

### Chứng minh
Ta muốn xếp $\{1, 2, \dots, n\}$
Vị trí đầu có $n$ cách $a_1$
		thứ 2 có $(n - 1)$ cách $a_2$
		$\vdots$
		thứ k có $(n - k - 1)$ cách $a_k$
		$\vdots$
		thứ n có $1$ cách a_n