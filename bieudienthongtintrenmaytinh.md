## 3. [[Biểu diễn số nguyên]]

- Có 2 loại
    - số nguyên không dấu - unsigned integer
    - số nguyên có dấu - signed integer
- Giá trị của số nguyên dương A có dải biểu diễn : 0→2^n - 1

### 3.1 Biểu diễn số nguyên không dấu
### 3.2 Biểu diễn số nguyên có dấu
#### Dải biểu diễn
>Tổng quát: $-2^{n-1}$ -> $2^{n-1} - 1$ 

-  8bit: -128 -> 127
- 16bit: -32768 -> 32767
- 32bit: $-2^{31}$ -> $2^{31} - 1$
- 64bit: $-2^{63}$ -> $2^{63} - 1$

#### Phương pháp dấu và độ lớn
- bit đầu tiên: 
	- `0`: số dương
	- `1`: số âm
- độ lớn: bit còn lại biểu diễn giá trị tuyệt đối của số dạng nhị phân
- VD
	```
	A = -60
	Có 60 = 0011 1100
	số âm => thêm bit dấu 1 = 1011 1100
	=> -60 = 1011 1100
  ```

#### Số bù chín và số bù 10
#### Quy tắc tìm số bù một và số bù hai
- Số bù một = đảo giá trị cac bit
- số bù hai = số bù một + 1
- VD:
	```
	Cho A       =   0010 0101
	Số bù một   =   1101 1010
					+       1
	Số bù hai   =   1101 1011
	```

#### Biểu diễn số nguyên có dấu bằng mã bù hai


- Với A > 0 :
	- bit $a_{n-1} = 0$, bit còn lại biểu diễn độ lớn như số không dấu
	- Dạng tổng quát: $0a_{n-2}...a_2a_1a_0$
	- Dải biểu diễn: 0 -> $2^{n-1} - 1$
	- VD: `A = 58 = 0011 1010`
- Với A < 0 
	- bit $a_{n-1} = 1$, biểu diễn bằng số bù hai của số dương tương ứng
	- Dạng tổng quát: $1a_{n-2}...a_2a_1a_0$
	- Dải biểu diễn: $-1$ -> $-2^{n-1}$
	- VD:
		```
		B = -80
		Có 80      = 0101 0000
		Số bù 1    = 1010 1111
					+        1
		Số bù 2    = 1011 0000
		=> B =  1010 0000
		```


## Cộng trừ nhân chia số nguyên
#### Cộng số nguyên không dấu
- Nguyên tắc : khi cộng hai số nguyên không dấu n-bit -> KQ là n-bit
	- Nếu $C_{out} = 0$ => KQ đúng
	- Nếu $C_{out} = 1$ => KQ sai do tràn nhớ ra ngoài (carry out) khi: **tổng > $2^n - 1$**
		- Lúc này, kết quả vẫn được lưu lại trong phạm vi n-bit
		- Bit vượt quá bị loại bỏ => kết quả bị sai so với thực tế
		- VD:
		```
		Câu 2.25: Đối với số không dấu, 8 bit, 240 + 27 = ?
		=> 240 + 27 = 267 
			240 = 1111 0000
			+
			27  = 0001 1011
			________________
				1 0000 1011  (9bit)
		=> tuy nhiên hệ thống chỉ lưu 8 bit 
		=> Giá trị còn lại sau khi bỏ bit Cout là 0000 1011 = 11
		
		```

#### Cộng số nguyên có dấu
Khi cộng 2 số nguyên có dấu n-bit => KQ là n-bit và không cần quan tâm đến bit $C_{out}$
- Cộng 2 số khác dấu => KQ luôn đúng
- Cộng 2 số cùng dấu
	- KQ cùng dấu với số hạng => KQ đúng
	- KQ ngược dấu => xảy ra overflow và KQ sai, tràn xảy ra khi tổng nằm ngoài dải $-2^{n-1}$ -> $2^{n-1} - 1$ 
		```
		VD1: 75 + 82
		75   =  0100 1011
		+
		82   =  0101 0010 
		____    ___________
		+157    1001 1101    (số 1 đầu <=> bit âm)
				= -128 + 16 + 8 + 4 + 1 = -99 
				
		========================
		VD2: -104 + -43
		-104  =  1001 1000
		+
		-43   =  1101 0101 
		____    ___________
		-147   1 0110 1101  
				= 64 + 32 + 8 + 4 + 1 = +109
		Do tràn bộ nhớ -147 nằm ngoài -128 -> 127
		=> bỏ số 1 đầu tiên 
		=> 0110 1101 = +109 (số 0 bit đầu <=> số dương)
		
		```


#### Nhân số nguyên không dấu

## Số dấu phẩy động
#### Nguyên tắc chung
- Floating Point Number -> biểu diễn cho số thực

>TQ:  $X = (-1)^S . M . R^E$ 
- M : phần định trị - mantissa
- R : cơ số - radix
- E : phần mũ - exponent

>TQ phép nhân:
>$X_1 . X_2 = (-1)^{S_1 \oplus S_2} . (M_1 . M_2). R^{E_1 + E_2}$   

>TQ phép chia
>$X_1 / X_2 = (-1)^{S_1 \oplus S_2} . (M_1 / M_2). R^{E_1 - E_2}$ 

#### Chuẩn IRR754/85
- R = 2
###### Dạng 32bit - single
>$X = (-1)^S.1,M.R^{E-127}$ 

[dang32bit.png](./img/dang32bit.png)
- S là bit dấu
	- S = 0 -> số dương
	- S = 1 -> số âm
- e (8 bit) là mã excess-127 của phần mũ E
	- e = E + 127 => E = e - 127
	- 127 là độ lệch ([bias](#bias))
- m (23 bit) là phần lẻ của phần định trị M = 1.m
- Công thức xác định số thực: $X = (-1)^S.1.m.2^{e-127}$ 

###### Dạng 64bit - double
> $X = (-1)^S.1,M.R^{E-1023}$ 

[64bit.png]()

Số thực trong chuẩn IEEE 754 dạng **double precision** (64-bit) được chia thành ba phần:
- **1 bit** cho dấu (S),
- **11 bits** cho mũ (exponent),
- **52 bits** cho phần **mantissa** (hệ số).

###### Dạng 80bit - double-extended
>$X = (-1)^S.1,M.R^{E-16383}$ 

[80bit.png]()

###### bias
Trong chuẩn IEEE 754, bias - hệ số bù là một giá trị được thêm vào mũ thực (exponent) để đảm bảo rằng mũ trong biểu diễn nhị phân luôn là một số dương.
=> dễ dàng xử lý số âm và số dương trong các phép toán
=> được sử dụng để **điều chỉnh** mũ sao cho mũ thực (unbiased exponent) có thể biểu diễn các giá trị dương và âm một cách hiệu quả.

Cách tính bias : 
> $Bias = 2^{k - 1} - 1$
- k : exponent - số bit dành cho mũ

