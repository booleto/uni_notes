

### Các phương pháp RK cấp 2
Ý tưởng: Chúng ta lấy tích phân từ $t \rightarrow th$ ($h$ là bước/step $<< 1$)

$$
\int^{t+h}_{t}y^.(x)ds = \int^{t+h}_{t} f(s, y(x))ds 
$$

$$
\rightarrow y(t+h) = y(t) + \int^{t+h}_{t} f(s, y(x))ds \quad (2)
$$

Ta xấp xỉ $\int^{t+h}_t$ bên vế phải bởi $c_0 f(t, y)h + c_1 f(t+ph, y(t+ph))h \quad (3)$

ở đây ta có 2 hệ số $c_0, c_1$ và 2 điểm $(t, y(t))$ và $(t + ph, y(t + ph))$

Nhắc lại: $\int^{t+h}_{t}f(x, y(t))ds \approx \frac{1}{2} f(t,y)h + \frac{1}{2} f(t+h, y(t+h)) \cdot h$ , ở đây $c_0 = c_1 = \frac{1}{2}$ và $p=1$

(Đây là công thức hình thang)

Vì $y(t+ph)$ chưa biết nên ta xấp xỉ thêm
$y(t+ph) \approx y(t) + q f(t,y(t)) \cdot h \quad (4)$

Thay (4) vào (3) ta có RK cấp 2

$$
y(t+h) = y(t) + h [c_0 f(t, y)h + c_1 f(t+ph, y(t)+qf(t, y(t)) )h]
$$

ở đây $c_0, c_1, p, q$ là các tham số. 
Các tham số khác nhau $\rightarrow$ các phương pháp khác nhau

RK viết lại:

$$
y(t+h) = y(t) + c_0 k_0 + c_1 k_1
$$

$$
k_0 = hf(t, y(t))
$$

$$
k_1 = hf(t+ph, y(t)+qk_0)
$$

**Các phương pháp phổ biến:**

$c_0 = 0, c_1 = 1, p = q = \frac{1}{2} \rightarrow$ Euler cải tiến

$c_0 = c_1 = \frac{1}{2}, p = q = 1 \rightarrow$ Phương pháp Heun/hình thang

$c_0 = \frac{1}{2}, c_1 = \frac{2}{3}, p = q = \frac{3}{4} \rightarrow$ phương pháp Ralson

Cụ thể:

Hình thang/Heun:

$$
y(t+h) = y(t) + \frac{1}{2} \cdot (k_0 + k_1)
$$

$$
k_0 = hf(t, y(t))
$$

$$
k_1 = hf(t+h, y(t)+k_0)
$$

Ralston

$$
y(t+h) = y(t) + \frac{1}{3} k_0 + \frac{2}{3} k_1)
$$

$$
k_0 = hf(t, y(t))
$$

$$
k_1 = hf(t+ \frac{3}{4} h, y(t)+ \frac{3}{4} k_0)
$$

Euler cải tiến

$$
y(t+h) = y(t) + k_1)
$$

$$
k_0 = hf(t, y(t))
$$

$$
k_1 = hf(t+ \frac{1}{2} h, y(t)+ \frac{1}{2} k_0)
$$

### Bài tập:

$$
\dot y(t) = 2t \cdot y(t) + e^{t^2} \quad \forall t \in [0, 10]
$$

$$
y(0) = 1
$$

Cho nghiệm chính xác $x(t) = e^{t^2}(t+1)$

Đề bài yêu cầu tính $y(t)$ đến $t=0.04$ với bước $h=0.01$

Với công thức hình thang:

$$
y(t+h) = y(t) + \frac{1}{2} \cdot (k_0 + k_1)
$$

$$
k_0 = h (2t \cdot y(t) + e^{t^2})
$$

$$
k_1 = h( 2(t+h) \cdot (y(t)+k_0) + e^{t^2})
$$

|t | 0 | 0.1 | 0.2 | 0.3 |
|----------------- | --- | --- | --- | --- |
| $h (2t \cdot y(t) + e^{t^2})$ | 0.1 | 0.12 | 0.15 | 0.19
| $y(t) + k_0$ | 1.1 | 1.23 | 1.41 | 1.64 |
| $k_1 = h( 2(t+h) \cdot (y(t)+k_0) + e^{t^2}$ | 0.12 | 0.17 | 0.22 | 0.28 |
| $y(t) + \frac{1}{2} \cdot (k_0 + k_1)$ | 1.11 | 1.26 | 1.45 | **1.69** |

=> $y(0.4) = 1.69$

Với công thức Euler cải tiến:

$$
y(t+h) = y(t) + k_1
$$

$$
k_0 = hf(t, y(t)) = h (2t \cdot y(t) + e^{t^2})
$$

$$
k_1 = h f(t+ \frac{1}{2} h, y(t)+ \frac{1}{2} k_0) = h[ 2(t + \frac{1}{2} h)  \cdot (y(t) + \frac{1}{2} k_0) + e^{(t+h)^2}]
$$

|t | 0 | 0.1 | 0.2 | 0.3 |
|--- | --- | --- | --- | --- |
| $k_0 = h (2t \cdot y(t) + e^{t^2})$ | 0.1 | 0.12 |  |  |
| $y(t) + \frac{1}{2} k_0$ | 1.05 | 1.17 |  |  |
| $k_1 = h[ 2(t + \frac{1}{2} h)  \cdot (y(t) + \frac{1}{2} k_0) + e^{(t+h)^2}]$ | 0.11 |  |  |  |
| $y(t + h) = y(t) + k_1$ | 1.11 |  |  | **** |
