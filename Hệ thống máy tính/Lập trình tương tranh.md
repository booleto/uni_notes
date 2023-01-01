# Lập trình tương tranh (Concurrent programming)
### Khái niệm cơ bản
##### Tiến trình (Process):
Là bản thực thi của mã chương trình trong hệ điều hành. Các tiến trình có không gian nhớ cho chương trình và . Mỗi tiến trình được hệ điều hành quản lý qua 1 số liệu: pid
VD: Lệnh `fork()` cho phép sao lặp tiến trình cha, tạo ra 1 tiến trình con

```c
pid = fork();
```

Nếu `pid == 0` -> đang ở trong tiến trình con
nếu `pid > 0`  -> đang ở trong tiến trình cha

Chương trình:

```c
int a = 5;
main() {
	int pid = fork();
	if (pid == 0) {
		a += 10;   // con
	} else {
		wait(NULL); // chờ tiến trình con két thúc
		printf("a=%d/n", a);
		// in ra a = 5 vì a += được thực hiện ở tiến trình con
	}
}
```

##### Các trạng thái của tiến trình
VD: Running, Ready, blocked
- Running: khi tiến trình đang được hệ điều hành giao CPU để thực thi
- Ready: hết lượt sử dụng CPU, đang chờ đến lượt tiếp theo
- Blocked: đang chờ dữ liệu vào/ra

##### Luồng (threads)
Là mô hình cho phép thực hiện song song cùng 1 đoạn mã chương trình nhưng theo các trình tự khác nhau
Mỗi luồng có không gian nhớ cho biến cục bộ, tham số riêng nhưng chia sẻ các biến và tài nguyên tập thể
Mã chương trình là chung cho các luồng nhưng mỗi luồng có thể chạy 1 lệnh khác nhau

##### Tình trạng ganh đua (race condition)
Là tình trạng có nhiều hơn 1 tiến trình cùng truy cập 1 tài nguyên chung và dẫn tới kết quả không dự đoán được.

##### Vùng cạnh tranh (critical section/region)
Đoạn mà trong 1 chương trình mà khi thực hiện song song với đoạn mã khác, có thể gây ra tình trạng ganh đua

VD: các thao tác rút tiền tài khoản
1. Nhập mã PIN
2. Kiểm tra mã PIN
3. Nhập số tiền
4. `x := a`  <- critical section
5. `x := x - 3`  <- critical section
6. `a := 3`  <- critical section
7. Xì tiền từ ATM
8. In biên lai

###### 1. Giải pháp luân phiên (Alternato)
Cho 2 tiến trình A, B.
Dữ liệu chung:

```c
int turn; //0: lượt A; 1: lượt B
```

Mã tiến trình A và B:

```c
// A
A() {
	while (1) {
		while (turn != 0);
		critical_section();
		turn = 1; //giao lượt cho B
		non_critical_section();
	}
}
```
```c
// B
B() {
	while (1) {
		while(turn != 1);
		critical_section();
		turn = 0; // giao lượt cho A
		non_critical_section();
	}
}
```

Nhận xét:
- Giới hạn cho 2 tiến trình
- Nếu 1 tiến trình không muốn chạy đoạn mã thì chương trình không thể giao lượt cho tiến trình còn lại
- Đoạn mã xử lý cạnh tranh bị gắn cứng vào cho mỗi tiến trình. Không thể dùng chung 1 đoạn mã xử lý cạnh tranh cho nhiều tiến trình khác nhau
- Lãng phí thời gian CPU: tiến trình yêu cầu CPU chỉ để kiểm tra đã đến lượt chưa. Nhưng sẽ không tới lượt nếu CPU không được giao cho tiến trình còn lại

###### 2. Giải pháp Peterson
Tiến trình muốn sử dụng tài nguyên cạnh tranh thì phải bày tỏ nguyện vọng, trường hợp có 2 tiến trình bày tỏ nguyện vọng thì tiến trình tới trước sẽ được sử dụng tài nguyên.
Khai báo dữ liệu chung:

```c
int turn; //0: lượt A; 1: lượt B
int interest[2]; //true (1) là có quan tâm, false (0) là không quan tâm
```

Giải pháp Peterson cung cấp 2 hàm:

```c
enter_region(int pid); //pid == 0 thì là pid A, pid == 1 thì là pid B
leave_region(int pid);
```

Khi 1 tiến trình muốn sử dụng tài nguyên cạnh tranh thì cần gọi

```c
enter_region();
```

Nếu tài nguyên chưa sẵn sàng thì `enter_region()` không thoát ra
Sau khi chạy xong đoạn mã cạnh tranh, thì tiến trình cần gọi hàm `leave_region()`

Mã:

```c
enter_region(int pid) {
	int other = 1 - pid; //pid của tiến trình còn lại
	interest[pid] = TRUE;
	turn = pid;
	while ((turn == pid) && (interest[other] == TRUE));
}
```

Giải thích điều kiện:
`(turn == pid)`: Nếu thỏa mãn thì chứng tỏ là tiến trình hiện tại là tiến trình gán biến turn sau cùng
TH1: Nếu chỉ tiến trình hiện tại muốn sử dụng tài nguyên `(interest[other] == TRUE)` => sử dụng tài nguyên
TH2: Nếu `interest[other] == TRUE` => chờ (do đến sau)

```c
leave_region(int pid) {
	interest[pid] = FALSE;
}
```

Nhận xét:
- Vẫn giới hạn cho 2 tiến trình
- Nếu 1 tiến trình bày tỏ không quan tâm, thì tiến trình còn lại có thể khai thác tài nguyên cạnh tranh mà không cần chờ tới việc giao lượt
- Mã xử lý cạnh tranh (2 hàm) được viết độc lập với mã chương trình
- Lãng phí thời gian CPU: tiến trình yêu cầu CPU chỉ để kiểm tra đã đến lượt chưa. Nhưng sẽ không tới lượt nếu CPU không được giao cho tiến trình còn lại

###### 3. Giải pháp TSL (Test and Set Lock)
Là 1 lệnh được hỗ trợ bởi phần cứng (CPU) có cú pháp `TSL register, flag`
<=> 2 lệnh nếu được thực hiện 1 cách liên tục: `register := flag`, `flag := 1`
VD: sử dụng biến cờ để xử lý tương tranh tài nguyên
```c
L: if (flag == 0) { //rồi //ToC
	flag := 1; // ToU
	critical_section();
	flag := 0;
} else {
	goto L;
}
```
**ToC**: Time of Check
**ToU**: Time of Use
ToC =/= ToU, các lệnh được đảm bảo thực hiện liên tục

TSL cung cấp 2 hàm `enter_region()` và `leave_region()`

```s
enter_region: TSL register, flag;
			  cmp register, 0; //compare
			  jnz enter_region; // jump if not zero
			  ret

leave_region: mov flag, 0;
			  ret
```

*Nhận xét:*
- (+) Không còn bị giới hạn cho 2 tiến trình
- (-) Lãng phí CPU: vẫn giao CPU cho các tiến trình chưa có tài nguyên dữ liệu để xử lý

###### Nhận xét chung cho 3 giải pháp:
Chưa tạo được trạng thái `blocked` cho những tiến trình chưa có dữ liệu vào/ra

### Khái niệm Semaphore
Semaphore là 1 kiểu dữ liệu nguyên không âm mà ta chỉ có thể thay đổi giá trị của 1 biến qua 2 thao tác
- Thao tác `up`/`V`/`sem_post()`
- Thao tác `down`/`P`/`sem_wait()`

**Thao tác `down(&sem)`**: tích hợp 
- `sem > 0`, thao tác down giảm sem 1 đơn vị
- `sem = 0`, tiến trình gọi thao tác `down` bị chuyển sang trạng thái blocked, thao tác down bị trì hoãn

**Thao tác `up(&sem)`**: tăng sem lên 1 đơn vị
- `sem = 0`, thao tác `up` cũng sẽ đánh thức 1 tiến trình đang ngủ chờ trên semaphore đó

**2 cách sử dụng semaphore**
- *Binary semaphore*: 0/1 => tối đa chỉ có 1 tiến trình, down thành công được 1 binary semaphore
  => Có thể sử dụng semaphore nhị phân để bảo vệ đoạn mã cạnh tranh
  
```c
down(&mutex);
critical_section();
up(&mutex);
```

`mutex` (Mutual Exclusion) là 1 semaphore nhị phân

- *Non-binary semaphore:* >0, biểu diễn số lượng tài nguyên sẵn sàng của hệ thống
VD: `semaphore prititers = z;` dùng để đồng bộ việc cấp phát/giải phóng tài nguyên giữa các tiến trình

##### Bài toán Producer - Consumer

**Producer:**
- Sản xuất 1 mặt hàng
- Tới kho, kiểm tra có ngăn rỗng không? Nếu không => ngủ chờ
- Đưa hàng vào kho
- Lặp lại

**Consumer:**
- Tới kho kiểm tra. Có ngăn đầy không? Nếu không => ngủ chờ
- Lấy 1 mặt hàng khỏi kho
- Đánh thức producer
- Tiêu thụ mặt hàng
- Lặp lại

**Khai báo chung:**

```c
#define N 10; //10 ngăn

int count; //đếm số ngăn có hàng/đầy
```

**Mã chương trình:**

```c
producer() {
	while (1) {
		int i;
		produce_item(&i);
		if (count == N) sleep(); //kho đầy
		enter_item(i); i++; // đưa i vào trong kho
		if (count == 1) wakeup_consumer();
	}
}

consumer() {
	while (1) {
		int i;
		if (count == 0) sleep();
		get_item(&i); count--;
		if (count == N-1) wakeup_producer();
		collect_item(i);
	}
}
```

**Giải pháp sử dụng semaphore**

Khai báo chung:

```c
#define N 10;
semaphore mutex = 1; full = 0; empty = N; //full là số ngăn đầy, empty là số ngăn rỗng
```

code:

```c
producer() {
	int i;
	while (1) {
		produce_item(&i);
		down(&empty); // lấy 1 ngăn đầy, nếu không có thì ngủ
		down(&mutex); // bao bọc critical section bởi down mutex và up mutex
		enter_item(i);
		up(&mutex);
		up(&full);
	}
}

consumer() {
	int i;
	while (1) {
		down(&full);
		down(&mutex); //bao bọc critical section bởi down mutex và up mutex
		get_item(&i);
		up(&mutex);
		up(&empty);
		consumer_item(i);
	}
}
```

###### Tóm tắt cách viết mã xử lý tương tranh:
- Dựa trên yêu cầu bài toán, xác định các semaphore dùng để đồng bọ hoạt động cấp phát/giải phóng tài nguyên (vd: `full`, `empty`)
- Xác định đoạn mã cạnh tranh trong chương trình và bảo vệ nó bởi cặp:

  ``` c
  down(&mutex);
  ...
  up(&mutex);
	```

***Q:*** Nếu đoạn mã cạnh tranh không đúng, chúng ta có thể bảo vệ toàn bộ mã tiến trình bằng `down / up (&mutex)` ?

***Chú ý:*** không được phép down semaphore liên quan đến tài nguyên, trong cặp `down / up (&mutex)`, vì có thể gây ra tình trạng deadlock

##### Bài toán bữa ăn của các nhà hiền triết Trung Hoa (Dining philosophers)
Có 5 nhà hiền triết ngồi quanh bàn tròn. Trước mặt mỗi nhà hiền triết là 1 đĩa mì, 5 đũa, đặt xen kẽ giữa các đĩa mì.
Hoạt động của các nhà hiền triết:
- Suy ngẫm
- Thấy đói, nhặt 2 đũa liền kề
- Ăn
- Đặt đũa xuống

**Một giải pháp:**

Khai báo chung:

```c
#define N 5;
#define LEFT_CH(i) i;
#define RIGHT_CH(i) (i + 1)%N;
```

Code:

```c
philosopher(int i) { //i = 0..4
	while(1) {
		think();
		take_chopstick(LEFT_CH(i));
		take_chopstick(RIGHT_CH(i));
		eat();
		drop_chopstick(LEFT_CH(i));
		drop_chopstick(RIGHT_CH(i));
	}
}
```

sự cố: deadlock xảy ra khi mỗi nhà hiền triết lấy được đũa trái và chờ lấy đũa phải

**Sử dụng semaphore:**

Khai báo chung:

```c
#define N 5;
#define THINKING 0;
#define HUNGRY 1;
#define EATING 2;
#define RIGHT(i) (i+1)%N;
#define LEFT(i) (i-1+N)%N;
semaphore mutex = 1;
semaphore S[N]; //S[i] = 1 nếu 2 đũa sẵn sàng, = 0 nếu 2 đũa không sẵn sàng => ngủ chờ
int state[N]; //state[i] = THINKING/HUNGRY/EATING
```

Code:

```c
philosopher(int i) {
	while(1) {
		think();
		take_chopsticks(i); // lấy cả 2 đũa hoặc chờ
		eat();
		drop_chopsticks(i);
	}
}
```

Hàm `take_chopsticks()` sử dụng 1 hàm phụ `test(i)` để kiểm tra là 2 đũa có sẵn sàng trên bàn hay không
Nếu có thì dùng semaphore S[i] báo hiệu lấy được 2 đũa, và chuyển trạng thái của NHT i sang EATING (đánh dấu 2 đũa đã thuộc về NHT i)

```c
take_chopsticks(int i) {
	state[i] = HUNGRY;
	down(&mutex);
	test(i); //critical
	up(&mutex);
	down(&S[i]); // lấy 2 đũa hoặc ngủ chờ
}

test(int i) {
	if ((state[i] == HUNGRY) && (state[LEFT] != EATING) &&
		(state[RIGHT] != EATING)) {
		up(&S[i]);
		state[i] = EATING; //đánh dấu đũa không còn sẵn sàng
	}
}

drop_chopsticks(int i) {
	state[i] = THINKING; //bỏ 2 đũa xuống
	down(&mutex);
	test(LEFT(i)); //đánh thức nhà hiền triết trái/phải (mã cạnh tranh)
	test(RIGHT(i));
	up(&mutex);
}
```

##### Bài toán đọc và ghi (Reader - writer problem)
Có 1 số tiến trình đọc và 1 số tiến trình ghi trên cùng 1 tệp
Các thao tác thực hiện song song:

		P1			P2			 Permission
		R			W 				OK
		W 			W 				KO
		R 			W 				KO
		W 			R 				KO

- Chỉ có 1 tiến trình được phép ghi vào tệp. Có thể dùng 1 semaphore nhị phân để gác quyền ghi
- Nếu đã có 1 tiến trình đọc thì các tiến trình đọc khác có thể chạy song song, nhưng không cho phép tiến trình ghi nào chạy tiếp
=> Tiến trình đọc đầu tiên là giành quyền ghi, tiến trình đọc cuối cùng phải trả quyền ghi

**Khai báo chung:**

```c
semaphore mutex = 1, db = 1; //db gác quyền ghi, mutex bảo vệ đoạn mã cạnh tranh
int rc; //reader counter
```

**Code:**

```c
writer() {
	prepare_data();
	down(&db); //giành quyền ghi
	write_data();
	up(&db); //trả quyền ghi
}

reader() {
	//chuẩn bị đọc
	down(&mutex);
	rc++;
	if (rc == 1) { //reader đầu tiên cần giành quyền ghi
		down(&db);
	}
	up(&mutex);

	//đọc dữ liệu
	read_data(); // cũng là đoạn mã cạnh tranh, bảo vệ bằng db

	//kết thúc đọc
	down(&mutex);
	rc--;
	if (rc == 0) { //nếu là tiến trình đọc cuối cùng thì cần phải trả lại quyền ghi
		up(&db);
	}
	up(&mutex);
}
```

##### Bài toán cửa hàng cắt tóc (Barbershop/Sleeping Barbers)
Cửa hàng có N ghế chờ

**Client**
- Đến cửa hàng, kiểm tra ghế rồi nếu không có ghế thì bỏ đi
- Ngồi chờ, đánh thức thợ, gọi 1 thợ cắt tóc. Nếu chưa có thì ngồi chờ
- Được cắt tóc

**Barber**
- Tìm 1 khách hàng đang ngồi chờ, nếu không có thì ngủ chờ
- Đánh thức khách hàng, gọi khách hàng
- Cắt tóc

**Nhận xét**
- Ta sẽ không biểu diễn N ghế dưới dạng 1 semaphore N giá trị vì khách hàng không ngủ chờ nếu không có ghế rỗi
	=> Biểu diễn số ghế bận dưới dạng int
- Khách hàng đóng vai trò như là tài nguyên đi với thợ và ngược lại
	=> Số khách hàng và số thợ sẵn sàng bởi 2 semaphore clients, barbers

**Khai báo chung**

```c
#define N 5
semaphore mutex = 1;
semaphore clients, barbers; //barbers là số thợ sẵn sàng chứ ko phải tổng số thợ, clients là số khách hàng đang ngồi chờ

int waitings; //có bao nhiêu ghế bận
```

**Code:**

```c
barber() {
	while(1) {
		down(&clients);
		down(&mutex);
		up(&barbers); //đánh thức khách hàng
		waitings--;
		up(&mutex);
		cut_hair();
	}
}

client() {
	down(&mutex);
	if(waitings < n) { //còn ghế chờ
		waitings++;
		up(&clients); //đánh thức barber
		up(&mutex);
		down(&barbers); //gọi 1 thợ ; không có => ngủ
		get_haircut();
	}
}
```

##### Bài toán tạo phân tử nước
Cho 2 dây chuyền sản xuất các nguyên tố hydro và oxygen. Dây chuyền thứ 3 thu thập 2 nguyên tử hydro + 1 nguyên tử oxygen để tạo ra 1 phân tử nước
Yêu cầu: viết chương trình mô phỏng 3 dây chuyền này

**Biểu diễn dữ liệu chung**

```c
semaphore h_sem, o_sem;
int w_mol;
semaphore mutex = 1;
```

```c
make_hydrogen() {
	while(TRUE) {
		down(&mutex);
		up(&h_sem);
		printf("_H_")
		up(&mutex);
		sleep(1);
	}
}

make_oxygen() {
	while(TRUE) {
		down(&mutex);
		up(&o_sem);
		printf("_O_")
		up(&mutex);
		sleep(1);
	}
}

make_water() {
	while(TRUE) {
		down(&h_sem);
		down(&h_sem);
		down(&o_sem);
		down(&mutex);
		w_mol++;
		printf("w_mol = y.d \n", w_mol);
		up(&mutex);
	}
}
```

### Đồng bộ giữa các tiến trình sử dụng Monitor
Nhận xét: Lập trình với semaphore, nếu không cẩn trọng, có thể dễ tạo ra deadlock vì các thao tác up/down có thể đặt bất kỳ đâu trong chương trình

##### Khái niệm Monitor
Được tạo ra từ lập trình hướng đối tượng và lập trình tương tranh
Cụ thể 1 monitor là 1 đối tượng thuộc lớp có cấu trúc sau

```c
monitor myMonitor {
	shared data; // tài nguyên tương tranh
	condition variables; // kiểm soát sử dụng tài nguyên qua cơ chế ngủ và đánh thức

	procedure p1() {...};

	procedure pn() {...};

	procedure init() {...};
}
```

Trong 1 monitor chỉ có 1 procedure được phép chạy tại 1 thời điểm
=> có thể viết các procedure sao cho mỗi procedure chứa 1 đoạn mã cạnh tranh
=> không bao giờ có 2 đoạn mã cạnh tranh (trên shared data) cùng thực hiện
=> không có race condition

Các biến condition variables kiểm soát việc đoạn mã cạnh tranh có thể truy cập shared data hay không. Nếu dữ liệu chưa sẵn sàng thì tiến trình sẽ ngủ chờ trên condition variable. Một tiến trình khác sẽ đánh thức khi điều kiện thỏa mãn

###### VD: Bài toán gửi/rút tiền

```c
monitor GuiRutTien {
	int balance;
	procedure RutTien() {
		int x;
		x = balance;
		x = x - z;
		balance = x;
	}

	procedure GuiTien() {
		int y;
		y = balance;
		y = y + 3;
		balance = y;
	}
}
```

Cài đặt trong java

```java
class GuiRutTien {
	int balance;
	public synchronized RutTien() {
		int x;
		x = balance;
		x = x - z;
		balance = x;
	}
	
	public synchronized GuiTien() {
		int y;
		y = balance;
		y = y + z;
		balance = y;
	}
}
```

Thực chất đoạn mã chương trình trên đây được compiler tự chèn lệnh khóa ở đầu mỗi thủ tục và mở khóa cuối mỗi thủ tục. Cụ thể đoạn mã mà chương trình được chèn như sau:

```java
public synchronized RutTien() {
	int x;
	this.mutex_lock(); // chèn tự động
	x = balance;
	x = x - z;
	balance = x;
	this.mutex_unlock(); // chèn tự động
}
```
(tương tự cho GuiTien)

###### Condition variables

Một condition variable là 1 biến điều kiện logic được gắn với 1 hàng đợi FIFO

```java
if(cond_var) {
	procedure()
} else {
	queue.add()
}
```

Nếu được logic thỏa mãn, tiến trình được phép chạy đoạn mã cạnh tranh trong thủ tục monitor
Nếu không, tiến trình chuyển sang trạng thái ngủ và được gắn vào cuối hàng đợi FIFO
Một tiến trình khác khi chạy xong đoạn mã chương trình sẽ đánh thức tiến trình đầu hàng đợi FIFO để thực hiện

2 thao tác liên qua đến cond_var:

```java
wait(cond_var); //ngủ chờ trên biến điều kiện
signal(cond_var); // đánh thức tiến trình đầu hàng đợi FIFO gắn với cond_var
```

###### Bài toán nhà sản xuất - người tiêu dùng

```c
monitor NSX_NTD {
	int fullSlots; // đếm số ngăn có hàng
	cond_var_t full; empty;

	procedure producer() {
		int item;
		produce_item(&item);
		
		if(fullSlot == N) { // không cần ngăn rỗng
			wait(&empty); // ngủ trên cond_var là ngăn rỗng
		}
		enter_item(item); fullSlots++;
		signal(&full); // đánh thức NTD ở đầu hàng đợi đang ngủ chờ ngăn có hàng
	}

	procedure consumer() {
		int item;
		if(fullSlots == 0) wait(&full);
		get_item(&item);
		signal(&empty);
		consume_item(item);
	}
}
```

###### Cài đặt monitor trong C/C++
C++ không hỗ trợ trực tiếp monitor mà cần phải cài đặt qua việc:
- Khóa và mở khóa mutex một cách thủ công
- Condition variables 

Các hàm thư viện pthread hỗ trợ khóa mutex và cond_var:

**Khóa mutex**

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_lock(); // khóa
pthread_mutex_unlock(); // mở
```

**Condition variables**

```c
pthread_cond_t condVar = PTHREAD_COND_INITIALIZER;
pthread_cond_wait(); // ngủ chờ
pthread_cond_signal(); // đánh thức
```

**Khai báo chung**

```c
pthread_cond_t full, empty;
pthread_mutex_t mutex;

void* producer() {
	int item;
	while(TRUE) {
		produce_item(&item);
		pthread_mutex_lock(&mutex);
		if(fullSlots == N) {
			pthread_cond_wait(&empty);
		}
		enter_item(item);
		pthread_cond_signal(&full, &mutex);
		pthread_mutex_unlock(&mutex);
	}
}

void* consumer(void*arg) {
	while(TRUE) {
		pthread_mutex_lock(&mutex);
		if (fullSlots == 0) {
			pthread_cond_wait(&full, &mutex);
		}
		get_item(&item);
		pthread_cond_signal(&empty, &mutex);
		pthread_cond_unlock(&mutex);
		consume_item(item);
	}
}
```

Nhận xét:
- Hàm `wait(cond, mutex)` chuyển tiến trình sang trạng thái ngủ chờ điều kiện cond và sau đó `unlock(&mutex)`
- Hàm `signal(&cond, &mutex);` chủ động khóa mutex sau đó trở lại và đánh thức tiến trình ngủ chờ
- Ngữ nghĩa cài đặt monitor khi đánh thức tiến trình:
	- sender priority (Mesa semantics) vs receiver priority (Hoarés semantics)
		- sender priority: Tiến trình đánh thức vẫn tiếp tục thực hiện cho tới khi thoát khỏi monitor. Dễ cài đặt nhưng dễ tạo xung đột
		- receiver priority: tiến trình được đánh thức sẽ được giao quyền điều khiển ngay lập tức. Khó cài đặt thực tế. Giải pháp: thay `if` bằng `while`

