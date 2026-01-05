## A. LÝ THUYẾT:
### I. Đường cong Elliptic trên tập R:
- Công thức tổng quát theo phương trình Weierstrass: $$y^2 + a_1xy + a_3y = x^3 + a_2x^2 + a_4x + a_6$$
- Sau một số phép thế và biến đổi, phương trình rút gọn còn: $$y^2 = x^3 + Ax + B$$ Với điều kiện $4A^3 + 27B^2 \neq 0$
- Đồ thị của đường cong Elliptic:
    - Trên mặt phẳng 2D:
![image](https://hackmd.io/_uploads/HJygVdx5xl.png)
    - Trên không gian 3D:
![image](https://images.viblo.asia/41f7339d-299e-4304-89a8-7c08c101e282.gif)
> Ví dụ cách vẽ đồ thị đường cong Elliptic $y^2 = x^3 - x + 1$ bằng SageMath:
> ``` py
> # Định nghĩa hàm Elliptic
> a = -1
> b = 1
>
> # Tạo implicit plot (vẽ đồ thị ẩn của phương trình)
> plot_elliptic = implicit_plot(y^2 == x^3 + a*x + b, (x, -3, 3), (y, -5, 5), axes = True)
>
> # Hiển thị
> plot_elliptic.show()
> ```

#### 1. Phép cộng trên đường cong Elliptic:
![image](https://hackmd.io/_uploads/BJrAEugqlg.png)
- Cách hoạt động của phép cộng hai điểm $P$ và $Q$ trên đường cong Elliptic:
    - Kẻ đường thẳng $A$ đi qua $P$ và $Q$.
    - Đường thẳng $A$ cắt đường cong Elliptic tại điểm $-R = -(P + Q)$.
    - Lấy đối xứng giao điểm qua trục $Ox$, ta được điểm $R = (P + Q)$.
Ta có: $$\begin{cases}
x_R=\lambda^2-x_P-x_Q\\
y_R=\lambda(x_P-x_R)-y_P\\
\end{cases}$$ Với: $$\lambda=\frac{y_Q-y_P}{x_Q-x_P}$$ ![image](https://hackmd.io/_uploads/HkluH_gceg.png)
- Trong trường hợp hai điểm $P$ và $Q$ trùng nhau, đường thẳng $A$ là tiếp tuyến với đường cong tại điểm $P$. Thực hiện tương tự ta được điểm $2P$: $$P \oplus Q = R'$$ $$\leftrightarrow P \oplus P = R'$$ $$\leftrightarrow 2P = R'$$
Ta có: $$\begin{cases}
x_R=\lambda^2-2x_P\\
y_R=\lambda(x_P-x_R)-y_P\\
\end{cases}$$ Với: $$\lambda=\frac{3x^2_P-A}{2y_P}$$
- Với trường hợp $P = -Q$, đường thẳng $A$ đi qua $P$ và $Q$ sẽ cắt Elliptic Curve tại điểm "vô cực" $\mathcal{O}$: $$P + Q = (-Q) + Q = \mathcal{O}$$ ![image](https://hackmd.io/_uploads/rJrAHulqlg.png)
- Các tính chất:
    - Tính đóng: Với $P, Q \in E$: $P + Q = R \in E$
    - Tính giao hoán $P + Q = Q + P$
    - Tính kết hợp: $P + Q + R = (P + Q) + R = Q + (Q + R)$
    - Tính nghịch đảo: $P+(-P)=\mathcal{O}$
    - Phần tử đơn vị: $P + \mathcal{O} = \mathcal{O} + P = P$

#### 2. Phép nhân trên đường cong Elliptic:
- Phép nhân trên đường cong Elliptic thực chất là thực hiện phép cộng nhiều lần.
> Ví dụ: Thực hiện nhân $3P$, ta tính $2P = P + P$, sau đó tính $3P = 2P + P$.
> ![image](https://hackmd.io/_uploads/H1gt2_e9gl.png)
- Việc tính toán $nP$ có thể thực hiện theo phương pháp nhân đôi và cộng.
    - Biểu diễn số $n$ thành dạng: $$n = n_0 + 2n_1 + 2^2n_2 + ... + 2^mn_m$$ Với điều kiện $[n_0...n_m] \in \{0, 1\}$
    - Áp dụng thuật toán:
    ``` py
    Q = 0
    for (i = 0; i <= m; i++):
        if n[i] == 1:
            Q = elliptic_combine(Q, P)     //Tính Q = Q + P
        P = elliptic_double(P)             //Tính P = 2P
    return Q
    ```
- Trên đường cong Elliptic không tồn tại:
    - Phép nhân hai điểm $P$ x $Q$
    - Phép chia vô hướng $Q:n$ (với $Q = nP$). Việc tìm số $n$ là bài toán Logarit rời rạc và khó có thể giải được trong thời gian đa thức.

### II. Đường cong Elliptic trên trường hữu hạn $\mathbb{F}_p$:
- Là đường cong có các hệ số thuộc trường $\mathbb{F}_p$ với công thức: $$y^2 \mod p ≡ (x^3 + Ax + B) \mod p$$ Với $4A^3 + 27B^2 \neq 0$
- Khác với đường cong Elliptic trong trường số thực, ta không thể biểu dễu Elliptic $\mathbb{F}_p$ bằng đồ thị hàm số liên tục.
- Tương tự như khái niệm đối xứng qua trục hoành của đường cong Elliptic số thực, đường cong Elliptic $\mathbb{F}_p$ cũng đối xứng theo nghĩa đối xứng modulo. Giả sử điểm $(x, y)$ thuộc đường cong Elliptic $\mathbb{F}_p$ trên thì điểm $(x, p - y)$ cũng thuộc đường cong trên vì: $$(p - y)^2 = p^2 - 2py + y^2 ≡ y^2 \mod p$$
> Ví dụ: $(1, 7)$ đối xứng với $(1, 16)$ vì $7 + 16 = 0 \mod 23$:
> ![image](https://hackmd.io/_uploads/Bk24t5b9ll.png)
> Các điểm đối xứng nhau qua đường $y = 11.5$. Riêng điểm  $(4, 0)$ xem như là đối xứng của chính nó.
- Cách vẽ trong SageMath:
```py
# (E) : y^2 = x^3 + Ax + B (mod p)
E = EllipticCurve(GF(p), [A, B]) #Thay p, A, B vào
points = E.points()
xs = [ZZ(P[0]) for P in points if not P.is_zero()]
ys = [ZZ(P[1]) for P in points if not P.is_zero()]
list_plot(list(zip(xs, ys)), size=20, axes = True, gridlines = True)
```

#### 1. Phép cộng:
- Tương tự nhóm Abel $(a, b)$ định nghĩa trên đường cong Elliptic số thực, ta cũng định nghĩa một nhóm Abel $E_P(a, b)$ gồm các điểm của đường cong Elliptic $Z_P$ cùng với điểm vô cực $\mathcal{O}$.
    - Điểm $\mathcal{O}$ là phần tử đơn vị của phép cộng.
    - Phần tử nghịch đảo của điểm $P$ trong phép cộng, ký hiệu là $-P$, là điểm đối xứng với $P$, hay $P + (- P) = \mathcal{O}$
    - Với hai điểm $P$ và $Q$ bất kỳ, phép cộng $R$ = $P + Q$ được xác định bằng công thức: $$x_R = \delta^2 - x_P - x_Q \mod p$$ $$y_R = \delta(x_P - x_R) - y_P \mod p$$ Trong đó: $$\Delta =
\begin{cases}
\frac{y_Q - y_P}{x_Q - x_P} \mod p & \text{nếu } P \ne Q \\\\
\left( \frac{3x_P^2 + a}{2y_P} \right) \mod p & \text{nếu } P = Q
\end{cases}$$
- Thực hiện phép toán cộng Elliptic trong Python:
``` py
E = EllipticCurve(GF(19), [-7, 10]) 

#Lấy điểm P(1, 17) và Q(5, 10) thuộc (E)
P = E(1, 17)
Q = E(5, 10)

#Tính R = P + Q
R = P + Q
print(R)
# (3 : 15 : 1)
```
``` py
E = EllipticCurve(GF(19), [-7, 10]) 

#Lấy điểm P(1, 17) thuộc (E)
P = E(1, 17)

#Tính Q = 2P
Q = P + P
print(Q)
#(18 : 4 : 1)
```
- Kiểm tra một điểm có thuộc đường cong hay không bằng hàm `.is_on_curve(x, y)`:
``` py
E = EllipticCurve(GF(19), [-7, 10]) 

#Lấy điểm P(1, 17) và Q(11, 7)
P = (1, 17)
Q = (11, 7)

print(E.is_on_curve(1, 17)) #true
print(E.is_on_curve(11, 7)) #false
```

#### 2. Phép nhân:
- Không tồn tại phép nhân hai điểm bất kỳ thuộc Elliptic Curve và phép chia một điểm có toạ độ $(x, y)$ với một số tự nhiên $n$ nào đó. 
- Về bản chất, phép nhân là phép cộng nhưng được thực hiện nhiều lần. Từ điểm $P$ gốc ban đầu, thực hiện phép nhân $P \in E$ với một số tự nhiên bất kì, ta được toạ độ của một điểm mới trong $E$.
```py
def Scalar_Mul(P, n):
    Q = P
    R = (0, 0)
    while n > 0:
    #If n ≡ 1 mod 2, set R = R + Q.
        if n % 2 == 1:
            R = add_point(R, Q)
        #Set Q = 2 Q and n = ⌊n/2⌋.
        Q = add_point(Q, Q)
        n = n//2
    return R
```
``` py
E = EllipticCurve(GF(19), [-7, 10]) 

#Lấy điểm P(1, 17) 
P = E(1, 17)

for i in range(1, 20):
    Q = i*P
    print(f"{i}*P =", Q)
    
1*P = (1 : 17 : 1)
2*P = (18 : 4 : 1)
3*P = (9 : 7 : 1)
4*P = (7 : 0 : 1)
5*P = (9 : 12 : 1)
6*P = (18 : 15 : 1)
7*P = (1 : 2 : 1)
8*P = (0 : 1 : 0)
9*P = (1 : 17 : 1)
10*P = (18 : 4 : 1)
11*P = (9 : 7 : 1)
12*P = (7 : 0 : 1)
13*P = (9 : 12 : 1)
14*P = (18 : 15 : 1)
15*P = (1 : 2 : 1)
16*P = (0 : 1 : 0)
17*P = (1 : 17 : 1)
18*P = (18 : 4 : 1)
19*P = (9 : 7 : 1)
```

#### 3. Order của đường cong Elliptic:
- **Order của một điểm trên đường cong Elliptic:** Order của một điểm $P$ thuộc Elliptic Curve là một số nguyên $n$ nhỏ nhất sao cho: $$nP = \mathcal{O}$$
- **Order của Elliptic Curve:** Order của một Elliptic Curve trong trường hữu hạn $\mathbb{F}_p$ là **tổng số lượng điểm thuộc đường cong** đó, bao gồm cả điểm vô cực $\mathcal{O}$, được ký hiệu là $\#E(\mathbb{F}_P)$.
- Số lượng các điểm trên đường cong Elliptic trong trường hữu hạn $\mathbb{F}_p$ không phải lúc nào cũng bằng $p$ (số phần tử của trường $\mathbb{F}_p$). Vì vậy, ta có **định lý Hasse**:
    - Cho $E$ là một đường cong Elliptic thuộc trường hữu hạn $F_P$. Gọi $\#E(\mathbb{F}_p)$ là số lượng các điểm thuộc $E$, khi đó: $$p + 1 - 2\sqrt{p} \leq \#E(\mathbb{F}_p) \leq p + 1 + 2\sqrt{p}$$
> Xét ví dụ: Cho $(E):y^2 \equiv x^3 - 7x + 10 \pmod{11}$. Theo định lý Hasse, số điểm thuộc $(E)$ khoảng: $$p + 1 - 2\sqrt{p} \leq \#E(\mathbb{F}_p) \leq p + 1 + 2\sqrt{p}$$ $$\leftrightarrow 11 + 1 - 2\sqrt{11} \leq \#E(\mathbb{F}_p) \leq 11 + 1 + 2\sqrt{11}$$ $$\leftrightarrow 5.3 \leq \#E(\mathbb{F}_p) \leq 18.6$$
> Danh sách các điểm thuộc $(E)$:
> ``` py
> # In ra các điểm thuộc (E)
> E = EllipticCurve(GF(11), [-7, 10])
> for P in E:
>     print(P)
> (0 : 1 : 0)
> (1 : 2 : 1)
> (1 : 9 : 1)
> (2 : 2 : 1)
> (2 : 9 : 1)
> (3 : 4 : 1)
> (3 : 7 : 1)
> (5 : 1 : 1)
> (5 : 10 : 1)
> (8 : 2 : 1)
> (8 : 9 : 1)
> (9 : 4 : 1)
> (9 : 7 : 1)
> (10 : 4 : 1)
> (10 : 7 : 1)
> ```
> Có $15$ điểm thuộc $(E)$, bao gồm cả điểm $\mathcal{O}$. Vậy bậc của đường cong Elliptic là $15$ hay $\#E(\mathbb{F}_p) = 15$.
> Ngoài ra, khi chọn điểm $P(1, 2)$ làm mốc:
> ``` py
> E = EllipticCurve(GF(11), [-7, 10])
> P = E(1,2)
> for i in range(20):
>     Q = i*P
>     print(f"{i}*P =", Q)
>     
> 0*P = (0 : 1 : 0)
> 1*P = (1 : 2 : 1)
> 2*P = (10 : 7 : 1)
> 3*P = (9 : 7 : 1)
> 4*P = (5 : 1 : 1)
> 5*P = (3 : 4 : 1)
> 6*P = (8 : 2 : 1)
> 7*P = (2 : 9 : 1)
> 8*P = (2 : 2 : 1)
> 9*P = (8 : 9 : 1)
> 10*P = (3 : 7 : 1)
> 11*P = (5 : 10 : 1)
> 12*P = (9 : 4 : 1)
> 13*P = (10 : 4 : 1)
> 14*P = (1 : 9 : 1)
> 15*P = (0 : 1 : 0)
> 16*P = (1 : 2 : 1)
> 17*P = (10 : 7 : 1)
> 18*P = (9 : 7 : 1)
> 19*P = (5 : 1 : 1)
> ```
> Có thể thấy $n = 15$ là order của điểm $P(1, 2)$ vì nó là số nguyên nhỏ nhất thoả mãn $nP=\mathcal{O}$.
> Mỗi một điểm thuộc $(E)$ đều sẽ có một order khác nhau. Order của một điểm $P$ bất kỳ trong $(E)$ đều là một ước số của order của đường cong Elliptic (theo định lý Lagrange trong lý thuyết nhóm):
> ``` py
> E = EllipticCurve(GF(17), [-7, 10])
> for P in E:
>     print(f"Order của {P} là một ước số của E? -", E.order() % P.order() == 0)
> ```

#### Hàm `discrete_log` trong Python:
Hàm `discrete_log` (log rời rạc) trong thư viện `sympy` của Python dùng để tìm nghiệm $x$ trong phương trình mũ rời rạc: $$a^x \equiv b \mod n$$
> **Ví dụ:** Tìm $x$ sao cho: $2^x \equiv 5 \mod 13$
> ``` py
> from sympy.ntheory.modular import discrete_log
>
> # Tìm x sao cho 2^x ≡ 5 mod 13
> x = discrete_log(13, 5, 2)
> print(x)  # Output: 4 
> ```
### III. Ứng dụng của Elliptic Curve:
- **Truyền thông an toàn:** Được sử dụng trong các giao thức như TLS/SSL để mã hoá giao tiếp trên web.
- **Chữ ký số:** Sử dụng nhiều trong các chương trình chữ ký số khác nhau.
- **Tiền điện tử:** Nhiều loại tiền điện tử (Bitcoin, Ethereum) sử dụng ECC để tạo khoá và chữ ký số.
- **Thiết bị di dộng:** Hiệu quả của ECC khiến nó trở nên lý tưởng cho các thiết bị di dộng có tài nguyên hạn chế.
- **Internet of Things (IoT):** Được sử dụng để bảo mật thông tin liên lạc giữa các thiết bị IoT có bộ nhớ và khả năng xử lý hạn chế.
- **Chính phủ và quân đội:** Được NSA áp dụng để bảo vệ thông tin mật.

#### 1. Discrete Logarthm Algorthm (DLP):
- DLP (Discrete Logarithm Problem) trên ECC (Elliptic Curve Cryptography) là một vấn đề toán học cốt lõi tạo nên tính an toàn của các hệ mật mã dựa trên đường cong elliptic. Cụ thể ta có bài toán như sau:
  - Cho $E$ là Elliptic Curve được định nghĩa trên trường hữu hạn $\mathbb{F}_p$. Gọi $G$ là một điểm khởi đầu và $Q$ là một điểm bất kì khác $G$ thuộc $E$. Tìm $k$ là một số nguyên thỏa mãn: $$Q = k.G$$ Giá trị $k$ khi này được gọi là Logarit rời rạc của $Q$ theo cơ sở $G$.
  - Vì không hề tồn tại phép chia trên Elliptic Curve nên cách phổ biến nhất và khả thi nhất để tìm lại được $k$ thường là brute-force trong khoảng $[1; n − 1]$, với $n$ là Order của điểm sinh $G$.
- Độ phức tạp thuật toán để giải ECDLP là $\mathcal{O}$.  Nếu như $n$ nhỏ thì việc brute - force sẽ trở nên khả thi. Nhưng trong thực thế, $n$ thường có độ dài lên đến $256$ hoặc $512$ bits, điều đó khiến cho việc brute - force trở nên không còn khả thi.
- Không hề có phương pháp nào đủ mạnh để phá được bài toán ECDLP này nếu các tham số được chọn lọc cẩn thận.

$\rightarrow$ Cho thấy tính khó nhằn của bài toán ECDLP với việc không có thuật toán nào hiện tại có thể giải nó một cách hiệu quả, ngoại trừ phương pháp brute force và một số thuật toán khác có độ phức tạp tính toán rất cao. **Đây chính là nền tảng cho độ an toàn của ECC**

#### 2. Elliptic - Curve Diffie - Hellman (ECDH):
##### Nhắc lại: Tính chất của phép nhân trong đường cong Elliptic:
Phép nhân chính là phép nhân vô hướng của một số với một điểm với tính chất:
- Trên đường cong Elliptic, cho hai số nguyên $m$, $n$ và một điểm $G$, ta có: $$m.(n.G) = (m.n).G = (n.m).G = n.(m.G)$$

##### a) Thuật toán:
- Cho đường cong $E_P(x_1, x_2)$ và một điểm cho trước nằm trên đường cong $G$, gọi G là điểm sinh.
    - Chọn khoá:
        - An chọn một khoá bí mật của mình là $a$.
        - Bình chọn một khoá bí mật của mình là $b$.
    - Trao đổi khoá:
        - An tính $A = a.G$ gửi cho Bình.
        - Bình tính $B = b.G$ gửi cho An.
    -  Tính toán khoá:
        -  An tính: $s_1 = a.B = a.(b.G)$
        -  Bình tính: $s_2 = b.A = b.(a.G)$
- An và Bình cùng chia sẻ khoá chung: $s_1 = s_2 = s$. Do không có phép chia trong đường cong Elliptic nên người thứ ba rất khó tìm ra $a$ khi biết $a.G$ hoặc $b$ khi biết $b.G$.
> Ví dụ: $G = (3, 10)$
> - Chọn khoá:
>     - An chọn khoá bí mật $a = 3$.
>     - Bình chọn khoá bí mật $b = 5$.
> - Trao đổi khoá:
>     - An tính $A = 3.(3, 10) = (19, 5)$ gửi cho Bình.
>     - Bình tính $B = 5.(3, 10) = (9, 16)$ gửi cho An.
> - Tính toán khoá:
>     - An tính: $s_1 = a.B = 3.(9, 16) = (1, 16)$
>     - Bình tính: $s_2 = b.A = 5.(19, 5) = (1, 16)$
> - Cả hai đã trao đổi thành công khoá bí mật chung. Khoá này có thể được dùng làm khoá chung cho mã hoá AES bằng cách lấy toạ độ $x_s = 1$ và dùng hàm băm như SHA - 512 để băm:
> ```py
> from hashlib impport sha512
> x_s = "1".encode()
> key = sha512(x_s).hexdigest[:32]
> print(key
> ```
> - Tóm tắt toàn bộ quá trình:
> ``` py
> # Dùng chung Elliptic Curve E, điểm sinh G
> # Định nghĩa trên trường F_19 với: y^2 = x^3 - 7x + 10
> E = EllipticCurve(GF(23), [1, 1])
> G = E(3, 10)
> 
> #Tạo khoá bí mật của An và Bình:
> a = 3
> b = 5
> 
> #Tính giá trị công khai:
> A = a*G     #(19 : 5 : 1)
> B = b*G     #(9 : 16 : 1)
> 
> #Tính giá trị bí mật:
> sA = a*B
> sB = b*A
> print(sA == sB)
> #s = (1 : 16 : 1)
> ```

##### b) Ưu, nhược điểm:
- Ưu điểm:
    - Bảo mật: Sử dụng cơ sở là đường cong Elliptic nên rất bảo mật.
    - Nhanh: Khoá rất ngắn, chỉ là một điểm nên dữ liệu truyền đi ít hơn.
- Nhược điểm: Có thể bị tấn công Man-in-the-middle.

#### 3. Elliptic Curve Digital Signature Algorithm (ECDSA):
- Được dùng để tạo chữ ký số cho dữ liệu dựa trên đường cong Elliptic, là phiên bản khác của DSA - Digital Signature Algorithm nhưng sử dụng trên Elliptic để nhằm sử dụng độ khó của ECDLP để tăng độ bảo mật cho dữ liệu.
- Giúp chống lại sự giả mạo hay sai lệch dữ liệu, cung cấp phương pháp xác thực mà không ảnh hưởng đến tính bảo mật của dữ liệu gốc.
- Được sử dụng rộng rãi trong nhiều lĩnh vực cần tính bảo mật cao và sự riêng tư dữ liệu (Blockchain,...).
- Chữ kí của ECDSA có dạng là một cặp số nguyên $(r, s)$. Để có thể kí được một plaintext $m$ nào đó:
    - An và Bình phải thoả thuận với nhau về việc sử dụng chung một Elliptic Curve trên trường hữu hạn $\mathbb{F}_p$ và điểm sinh $G$ có order $n$ bất kì thuộc đường cong đó.
    - An chọn khoá bí mật $a \in [1, n - 1]$ và khoá công khai $A = a.G$.
    - An chọn một số nguyên ngẫu nhiên tuyệt đối $k \in [1, n - 1]$ khác với khoá bí mật. Số nguyên $k$ được giữ bí mật tuyệt đối, chỉ có An biết.
    - An tính toạ độ điểm $P$ với công thức: $$P(x_P, y_P) = k.G$$ Khi này giá trị $r$ chính là $x_P$.
    - An tính $z = hash(m)$, với hash là một hàm băm nào đó đủ mạnh (SHA - 256, SHA - 512,...). Công thức tính $s$: $$s \equiv k^{-1}(z + r.a) \pmod n$$ Nếu $s = 0$, chọn lại $k$.
    - Sau khi có được cặp $(r, s)$, An sẽ gửi cặp này cho Bình cùng với $m$ đã được mã hoá của mình để có thể xác minh. Bình sẽ giải mã $m$ bằng khoá chung của cả hai, có thể dùng ECDH để trao đổi.
    - Khi có được plaintext $m$, Bình tính $z = hash(m)$. Để xác minh $m$ có chuẩn hay không, Bình sẽ tính: $$P(x_P, y_P) \equiv s^{-1}.(z.G + r.A) \pmod n$$ Khi đó nếu $x_P$ tính được từ công thức trên bằng $r$ thì chữ ký là hợp lệ và plaintext $m$ chính xác.
    - Chứng minh công thức: $$P(x_P, y_P) \equiv s^{-1}.(z.G + r.A) \pmod n$$ $$\leftrightarrow P = s^{-1}.(z.G + r.a.G)$$ $$\leftrightarrow P = s^{-1}.G.(z + r.a)$$ $$\leftrightarrow P = s^{-1}.G.s.k$$ $$\leftrightarrow P = k.G$$
    - Nếu chữ ký $(r, s)$ hoặc plaintext $m$ bị thay đổi trong quá trình truyền, $P$ tính toán cũng thay đổi $\rightarrow$ Chữ ký không hợp lệ $\rightarrow$ **Cơ chế bảo vệ tính toàn vẹn và xác thực của ECDSA.**
> ``` py
> #!/usr/bin/env sage -python
>
> from sage.all import *
> from hashlib import sha256
> from random import randint
>
> m = "Hello world!"
> # Dùng chung Elliptic Curve E, điểm sinh G
> # Định nghĩa trên trường F_19 với: y^2 = x^3 - 7x + 10
> E = EllipticCurve(GF(23), [1, 1])
> G = E(3, 10)
> 
> #Tạo khoá bí mật của An:
> a = 3
> 
> #Tính giá trị công khai:
> A = a*G     #(19 : 5 : 1)
> print("Public Key A =", A)
>
> #Bậc của điểm sinh G:
> n = G.order()
> print("Order n =", n)
>
> #Hash thông điệp m thành một số nguyên mod n:
> z = int.from_bytes(sha256(m.encode()).digest(), 'big')%n
> print("Hash(m) mod n =", z)
>
>
> #-----BƯỚC KÝ------
> while True:
>     k = randint(1, n - 1)
>     if gcd(k, n) != 1:   #Nếu không nghịch đảo được thì chọn k khác
>         continue
>     P = k*G
>     r = Integer(P[0]) % n  # r = x_P mod n
>     if r == 0:
>         continue
>     k_inv = inverse_mod(k, n)
>     s = (k_inv * (z + a*r)) % n
>     if s != 0:
>         break
> print("Signature (r, s) =", (r, s))
> 
> 
> # -----BƯỚC KIỂM TRA-----
> # Người nhận biết (A, G, n, h, r, s)
> s_inv = inverse_mod(s, n)
> P_ver = s_inv*(z*G + r*A)
> v = Integer(P_ver[0]) % n     #v = x_{P_ver} mod n
> print("Kiểm tra v =", v)
> print("Chữ ký hợp lệ?", v == r)
> ```
- Nếu dùng lại một $k$ cho hai thông điệp khác nhau:
    - Giả sử: (Vì $k$ giống nhau nên $r$ giống nhau) 
        - Thông điệp $m_1$: hash $z_1$, chữ ký $(r, s_1)$
        - Thông điệp $m_2$: hash $z_2$, chữ ký $(r, s_2)$
    - Ta có: $$s_1 = k^{-1}(z_1 + ar) \pmod n$$ $$s_2 = k^{-1}(z_2 + ar) \pmod n$$ Trừ vế theo vế: $$s_1 - s_2 = k_1(z_1 - z_2) \pmod n$$ Khi $s_1 \neq s_2$, suy ra: $$k = \frac{z_1 - z_2}{s_1 - s_2} \pmod n$$ Sau đó: $$a = \frac{k.s_1 - z_1}{r} \pmod n$$ Vậy hacker chỉ cần thấy hai chữ ký với $r$ là tính được.
- Nếu $k$ bị lộ, khoá bí mật $a$ cũng lộ ngay lập tức: $$s = k^{-1}(z + ar) \pmod n \rightarrow a = \frac{sk - z}{r} \pmod n$$

#### 4. Lý giải về độ bảo mật:
- Security bits (bits bảo mật) là cách đo lường mức độ an toàn của một hệ mật mã. Nó biểu thị số lượng bước tính toán mà kẻ tấn công cần thực hiện trong kịch bản tấn công tốt nhất để phá hệ thống.
- Nói cách khác, $n$-bit bảo mật nghĩa là cần khoảng $2^n$ phép tính để phá một hệ mật nào đó.
> Ví dụ:
> ![image](https://hackmd.io/_uploads/H1bWU-13gx.png)
> Từ bảng trên có thể thấy, với cùng một mức Bits bảo mật thì ECC tiêu tốn ít tài nguyên hơn RSA, DH, DSA,…

## B. THỰC HÀNH - CRYPTOHACK:
### I. BACKGROUND:
#### Background Reading:
##### a) Tóm tắt đề:
- Lý thuyết chính về ECC:
    - Khái niệm:
        - ECC là một giao thức mật mã bất đối xứng, giống RSA/Diffie - Hellman nhưng dựa vào "trapdoor function" nên khó để tính ngược.
        - Trong RSA: Khó ở phân tích số nguyên lớn.
        - Trong Diffie - Hellman: Khó ở logarit rời rạc.
        - Trong ECC: Khó ở việc tìm ra $n$ sao cho $Q = [n]P$ khi biết $P$ và $Q$ (phép nhân vô hướng trên đường cong).
    - Đường cong Elliptic:
        - Sử dunnjg dạng Weierstrass: $$E: Y^2 = X^3 + aX + b$$ Với điều kiện $4a^3 + 27b^2 \neq 0$
        - Điểm vô cực $\mathcal{O}$ có vai trò là phần tử đơn vị.
    - Phép cộng điểm:
        - Lấy hai điểm $P$ và $Q$ trên đường cong, nối đường thẳng qua chung, giao điểm với đường cong là $R$. Phép cộng: $$P + Q = R' = (x_R, y_R)$
        - Nếu $P = Q$: Dùng tiếp tuyến tại $P$ để tính.
        - Nếu không có giao điểm, kết quả là điểm vô cực $\mathcal{O}$.
        - Tập hợp các điểm với phép cộng này tạo thành nhóm Abel.
    - Thuộc tính nhóm quan trọng:
        - $P + \mathcal{O} = \mathcal{O} + P = P$
        - $P + (-P) = \mathcal{O}$
        - $(P + Q) + R = P + (Q + R)$ (Kết hợp)
        - $P + Q = Q + P$ (Giao hoán)
    - Trong ECC thực tế: Làm việc trên trường hữu hạn $F_p$ với toạ độ $x$, $y$ là số nguyên $\mod p$.
- Đề bài: *"Property (d) shows that point addition is commutative. The flag is the name we give groups with a commutative operation."* - "Nhóm có phép toán giao hoán gọi là gì?"

##### b) Phân tích cách làm:
Nhóm có phép toán giao hoán là **nhóm Abel** hoặc **Abelian group**.

##### c) Kết quả:
`crypto{Abelian}`

### II. STARTER:
#### 1. Point Negation:
##### a) Tóm tát đề bài:
- Để ứng dụng vào mật mã, cần xét đường cong Elliptic với toạ độ trong trường hữu hạn $F_p$:
    - Dạng tổng quát: $$E: Y^2 = X^3 + aX + b \pmod p$$ với $a, b \in $F_p$, điều kiện $4a^3 + 27b^2 \neq 0$.
    - Tập hợp điểm: $$E(F_p) = \{(x, y):x, y \in F_p, y^2 = x^3 + ax + b\} \cup \{\mathcal{O}\}$$
    - Phép cộng vẫn giữ nguyên quy tắc như challenge trước, chỉ khác là tất cả các phép tính thực hiện modulo $p$
- Đề bài:
    - Cho đường cong: $E: Y^2 = X^3 + 497X + 1768 \pmod {9739}$
    - Cho điểm $P(8045, 6936)$
    - Yêu cầu: Tìm điểm $Q(x, y)$ sao cho $P + Q = \mathcal{O}$

##### b) Phân tích cách làm:
- Vì $P + Q = \mathcal{O} \rightarrow Q = -P$
- Trên đường cong Elliptic, nghịch đảo (điểm đối) của $P(x, y)$ là: $$Q(x, -y \mod p)$$
- Source code:
```py
p = 9739
x = 8045
y = 6936
y_ = -y%p
print(f'crypto{{{x},{y_}}}')
```

##### c) Kết quả:
`crypto{8045,2803}`

#### 2. Point Addition:
##### a) Mô tả đề bài:
- Lý thuyết:
    - Đường cong Elliptic trên trường hữu hạn: $$E: Y^2 \equiv X^3 + aX + b \pmod p$$
    - Cộng hai điểm $P$, $Q$:
        - Nếu $P = \mathcal{O}: P + Q = Q$
        - Nếu $Q = \mathcal{O}: P + Q = P$
        - Nếu $x_1 = x_2$ và $y_1 = -y_2 \mod p: P + Q = \mathcal{O}$ (điểm đối xứng)
        - Nếu $P \neq Q$: $$\lambda = \frac{y_2 - y_1}{x_2 - x_1} \mod p$$
        - Nếu $P = Q$: $$\lambda = \frac{3x_1^2 + a}{2y_1} \pmod p$$
        - Sau đó: $$x_3 = \lambda^2 - x_1 - x_2 \pmod p$$ $$y_3 = \lambda(x_1 - x_3) - y_1 \pmod p$$
        - Kết quả: $$P + Q = (x_3, y_3)$$
    - Lưu ý quan trọng trong trường hữu hạn: Không chia trực tiếp, phải nhân với nghịch đảo modulo: $$\frac{a}{b} \mod p = a.b^{-1} \mod p$$
- Đề bài:
    - Cho:
        - Đường cong: $E: Y^2 \equiv X^3 + 497X + 1768 \pmod {9739}$
        - Các điểm: $P = (493, 5564)$, $Q = (1539, 4742)$, $R = (4403, 5202)$
    - Yêu cầu:
        - Tính điểm $S(x, y) = P + P + Q + R$ bằng cách áp dụng thuật toán cộng điểm trên đường cong Elliptic.
        - Sau khi tính được $S$, kiểm tra lại $S$ có nằm trên đường cong hay không bằng cách thay vào phương trình và kiểm tra vế trái với với vế phải.

##### b) Phân tích cách làm:
- Source code:
``` py
def point_add(P, Q, p):
    if P is None:
        return Q
    if Q is None:
        return P
    
    x1, y1 = P
    x2, y2 = Q

    #Nếu P = -Q:
    if x1 == x2 and (y1 + y2)%p == 0:
        return None
    if P != Q:
        lamda = ((y2 - y1)*pow(x2 - x1, -1, p))%p
    else:
        lamda = ((3*pow(x1, 2) + a)*pow(2*y1, -1, p))%p

    x3 = (pow(lamda, 2) - x1 - x2)%p
    y3 = (lamda*(x1 - x3) - y1)%p
    return x3, y3


def is_on_curve(S, a, b, p):
    if S is None:
        return True
    x, y = S
    return (pow(y, 2) - (pow(x, 3) + a*x + b))%p == 0



p = 9739
a = 497
b = 1768
P = (493, 5564)
Q = (1539, 4742)
R = (4403, 5202)

S = point_add(P, P, p)
S = point_add(S, Q, p)
S = point_add(S, R, p)
if is_on_curve(S, a, b, p):
    x, y = S
    print(f"crypto{{{x},{y}}}")
else:
    print("___")
```
- Source code SageMath:
```py
from sage.all import *

p = 9739
a = 497
b = 1768

E = EllipticCurve(GF(p), [a, b])

P = E(493, 5564)
Q = E(1539, 4742)
R = E(4403, 5202)

S = 2*P + Q + R
x, y = S.xy()
print(f"crypto{{{x},{y}}}")
```

##### c) Kết quả:
`crypto{4215,2162}`

#### 3. Scalar Multiplication:
##### a) Mô tả đề bài:
- Lý thuyết:
    - Tính $[n]P = P + P + ... + P$ $n$ lần trên đường cong Elliptic.
    - Thuật toán Double-and-Add (nhân vô hướng):
        - Input: $P \in E(F_p), n > 0$
        - Output: $Q = [n]P \in E(F_p)$
        ``` py
        Q = P
        R = None     #Điểm vô cực:
        while n > 0:
            if n%2 == 1:     #Nếu bit cuối là 1
                R = point_add(R, Q, p)     #R = R + Q
            Q = point_add(Q, Q, p)     #Q = Q + Q
            n //= 2     #n = n//2 (dịch phải)
        return R
        ```
- Đề bài:
    - Cho:
        - Đường cong: $E: Y^2 \equiv X^3 + 497X + 1768 \pmod {9739}$
        - Điểm: $P = (2339, 2213)$
    - Tính: $Q = [7863]P$

##### b) Phân tích cách làm:
- Source code:
```py
def point_add(P, Q, p):
    if P is None:
        return Q
    if Q is None:
        return P
    
    x1, y1 = P
    x2, y2 = Q

    #Nếu P = -Q:
    if x1 == x2 and (y1 + y2)%p == 0:
        return None
    if P != Q:
        lamda = ((y2 - y1)*pow(x2 - x1, -1, p))%p
    else:
        lamda = ((3*pow(x1, 2) + a)*pow(2*y1, -1, p))%p

    x3 = (pow(lamda, 2) - x1 - x2)%p
    y3 = (lamda*(x1 - x3) - y1)%p
    return x3, y3


def double_and_add(P, n, p):
    Q = P
    R = None     #Điểm vô cực:
    while n > 0:
        if n%2 == 1:     #Nếu bit cuối là 1
            R = point_add(R, Q, p)     #R = R + Q
        Q = point_add(Q, Q, p)     #Q = Q + Q
        n //= 2     #n = n//2 (dịch phải)
    return R


def is_on_curve(S, a, b, p):
    if S is None:
        return True
    x, y = S
    return (pow(y, 2) - (pow(x, 3) + a*x + b))%p == 0



p = 9739
a = 497
b = 1768
P = (2339, 2213)
n = 7863

S = double_and_add(P, n, p)
if is_on_curve(S, a, b, p):
    x, y = S
    print(f"crypto{{{x},{y}}}")
else:
    print("___")
```
- Source code SageMath:
``` py
from sage.all import *

p = 9739
a = 497
b = 1768

E = EllipticCurve(GF(p), [a,b])
P = E(2339,2213)

Q = 7863*P
x,y = Q.xy()
print(f"crypto{{{x},{y}}}")
```
##### c) Kết quả:
`crypto{9467,2742}`

#### 4. Curves and Logs:
##### a) Mô tả đề bài:
- Cho:
    - Đường cong Elliptic: $E: Y^2 = X^3 + 497X + 1768 \pmod {9739}$
    - Generator $G = (1804, 5368)$
    - Alice gửi: $Q_A = (815, 3190)$
    - Seceret interger: $n_B = 1829$
- Yêu cầu:
    - Tính shared secret: $$S = n_B.Q_A = n_B[n_A]G$$ Hoặc $S = n_A.Q_B$ bên phía Alice.
    - Lấy toạ độ $x$ của $S$ tính $SHA1(x)$ và in ra flag dạng hex.
    - Định dạng flag: `crypto{HASH}`

##### b) Phân tích cách làm:
- Sử dụng thuật toán Double_and_Add ở challenge trên để tính phép nhân vô hướng trên Elliptic Curve và Elliptic Curve Diffie - Hellman (ECDH):
    - Alice: $n_A$, $Q_A = n_A.G$
    - Bob: $n_B$, $Q_B = n_B.G$
    - Shared secret: $S = n_B.Q_A = n_A.Q_B$
- Source code:
```py
from hashlib import sha1

def point_add(P, Q, p):
    if P is None:
        return Q
    if Q is None:
        return P
    
    x1, y1 = P
    x2, y2 = Q

    #Nếu P = -Q:
    if x1 == x2 and (y1 + y2)%p == 0:
        return None
    if P != Q:
        lamda = ((y2 - y1)*pow(x2 - x1, -1, p))%p
    else:
        lamda = ((3*pow(x1, 2) + a)*pow(2*y1, -1, p))%p

    x3 = (pow(lamda, 2) - x1 - x2)%p
    y3 = (lamda*(x1 - x3) - y1)%p
    return x3, y3


def double_and_add(P, n, p):
    Q = P
    R = None     #Điểm vô cực:
    while n > 0:
        if n%2 == 1:     #Nếu bit cuối là 1
            R = point_add(R, Q, p)     #R = R + Q
        Q = point_add(Q, Q, p)     #Q = Q + Q
        n //= 2     #n = n//2 (dich phải)
    return R


def is_on_curve(S, a, b, p):
    if S is None:
        return True
    x, y = S
    return (pow(y, 2) - (pow(x, 3) + a*x + b))%p == 0



p = 9739
a = 497
b = 1768
Q_A = (815, 3190)
nB = 1829

S = double_and_add(Q_A, nB, p)
if is_on_curve(S, a, b, p):
    x, y = S
    x = str(x)
    flag = sha1(x.encode()).hexdigest()
    print(f"crypto{{{flag}}}")
else:
    print("___")
```
- Source code SageMath:
``` py
from sage.all import *
from hashlib import sha1

p = 9739
a = 497
b = 1768

E = EllipticCurve(GF(p), [a, b])
Q_A = E(815, 3190)
nB = 1829

S = nB*Q_A
x = int(S[0])
flag = sha1(str(x).encode()).hexdigest()
print(f"crypto{{{flag}}}")
```

##### c) Kết quả:
`crypto{80e5212754a824d3a4aed185ace4f9cac0f908bf}`

#### 5. Efficient Exchange
##### a) Mô tả đề bài:
- Allice và Bob dùng đường cong Elliptic để trao đổi khoá bí mật (ECDH). Để giảm dữ liệu truyền đi, chỉ cần gửi toạ độ $x$ của public key.
- Với đường cong Elliptic và số nguyên tố $p \equiv 3 \pmod 4$, có thể tính được $y$ từ $x$ (có hai nghiệm $y$ đối nhau là $y$ và $-y$).
- Khi nhân vô hướng với private key, kết quả $x$ của khoá chung (shared secret) không đổi dù chọn $y$ hay $-y$.
- Cụ thể, cho: $$E: Y^2 = X^3 + 497X + 1768 \pmod {9739}$$ Và generator $G = (1804, 5368)$
    - Alice gửi $x(Q_A) = 4726$, không gửi $y(Q_A)$
    - Bob có khoá riêng $n_B = 6534$, cần tính khoá chung (shared secret) của Bob. 
::: spoiler File `decrypt.py`
```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib


def is_pkcs7_padded(message):
    padding = message[-message[-1]:]
    return all(padding[i] == len(padding) for i in range(0, len(padding)))


def decrypt_flag(shared_secret: int, iv: str, ciphertext: str):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Decrypt flag
    ciphertext = bytes.fromhex(ciphertext)
    iv = bytes.fromhex(iv)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = cipher.decrypt(ciphertext)

    if is_pkcs7_padded(plaintext):
        return unpad(plaintext, 16).decode('ascii')
    else:
        return plaintext.decode('ascii')


shared_secret = ?
iv = ?
ciphertext = ?

print(decrypt_flag(shared_secret, iv, ciphertext))
```
:::
::: spoiler Thông số được cho
``` py
{'iv': 'cd9da9f1c60925922377ea952afc212c', 'encrypted_flag': 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'}
```
:::

##### b) Phân tích cách làm:
- Ta có đường cong Elliptic: $$Y^2 \equiv X^3 + 497X + 1768 \pmod {9739}$$
- Tìm $Y_Q$ khi biết $X_Q$: $$Y_Q^2 \equiv X_Q^3 + a.X_Q + b \pmod p$$ Vì $p \equiv 3 \pmod 4$ nên: $$Y_Q \equiv \pm (X_Q^3 + a.X_Q + b)^{\frac{p + 1}{4}} \pmod p$$
- Đặt $Q_A = (X_Q, Y_Q)$ vừa tìm được, nhân vô hướng để tìm shared secret: $$S = [n_B]Q_A$$
```py
from sage.all import *
from hashlib import sha1

p = 9739
a = 497
b = 1768
xQ = 4726
nB = 6534

E = EllipticCurve(GF(p), [a, b])
yQ2 = (xQ**3 + a*xQ + b)%p
yQ = pow(yQ2, (p + 1)//4, p)
Q_A = E(xQ, yQ)

S = nB*Q_A
xS = int(S[0])
print(f"Shared secret: xS =",  xS)
```
```ubuntu
Shared secret: xS = 1791
```
- Sau khi đã có $S$, dùng source code `decrypt.py` và các thông số đã cho để lấy flag:
```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import hashlib


def is_pkcs7_padded(message):
    padding = message[-message[-1]:]
    return all(padding[i] == len(padding) for i in range(0, len(padding)))


def decrypt_flag(shared_secret: int, iv: str, ciphertext: str):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Decrypt flag
    ciphertext = bytes.fromhex(ciphertext)
    iv = bytes.fromhex(iv)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    plaintext = cipher.decrypt(ciphertext)

    if is_pkcs7_padded(plaintext):
        return unpad(plaintext, 16).decode('ascii')
    else:
        return plaintext.decode('ascii')


backpack = {'iv': 'cd9da9f1c60925922377ea952afc212c', 'encrypted_flag': 'febcbe3a3414a730b125931dccf912d2239f3e969c4334d95ed0ec86f6449ad8'}
shared_secret = 1791
iv = backpack['iv']
ciphertext = backpack['encrypted_flag']

print(decrypt_flag(shared_secret, iv, ciphertext))
```

##### c) Kết quả:
`crypto{3ff1c1ent_k3y_3xch4ng3}`

### III. PARAMETER CHOICE:
#### 1. Smooth Criminal:
##### a) Tóm tắt đề bài:
- *"Tôi đã dành cả buổi sáng để tìm hiểu về ECC và giờ đã sẵn sàng bắt đầu mã hóa tin nhắn. Hôm nay tôi đã gửi một lá cờ cho Bob, nhưng anh sẽ không bao giờ đọc được đâu."*
- Source code đề cho:
::: spoiler File `source.py`
```py
from Crypto.Cipher import AES
from Crypto.Util.number import inverse
from Crypto.Util.Padding import pad, unpad
from collections import namedtuple
from random import randint
import hashlib
import os

# Create a simple Point class to represent the affine points.
Point = namedtuple("Point", "x y")

# The point at infinity (origin for the group law).
O = 'Origin'

FLAG = b'crypto{??????????????????????????????}'


def check_point(P: tuple):
    if P == O:
        return True
    else:
        return (P.y**2 - (P.x**3 + a*P.x + b)) % p == 0 and 0 <= P.x < p and 0 <= P.y < p


def point_inverse(P: tuple):
    if P == O:
        return P
    return Point(P.x, -P.y % p)


def point_addition(P: tuple, Q: tuple):
    # based of algo. in ICM
    if P == O:
        return Q
    elif Q == O:
        return P
    elif Q == point_inverse(P):
        return O
    else:
        if P == Q:
            lam = (3*P.x**2 + a)*inverse(2*P.y, p)
            lam %= p
        else:
            lam = (Q.y - P.y) * inverse((Q.x - P.x), p)
            lam %= p
    Rx = (lam**2 - P.x - Q.x) % p
    Ry = (lam*(P.x - Rx) - P.y) % p
    R = Point(Rx, Ry)
    assert check_point(R)
    return R


def double_and_add(P: tuple, n: int):
    # based of algo. in ICM
    Q = P
    R = O
    while n > 0:
        if n % 2 == 1:
            R = point_addition(R, Q)
        Q = point_addition(Q, Q)
        n = n // 2
    assert check_point(R)
    return R


def gen_shared_secret(Q: tuple, n: int):
    # Bob's Public key, my secret int
    S = double_and_add(Q, n)
    return S.x


def encrypt_flag(shared_secret: int):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Encrypt flag
    iv = os.urandom(16)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    ciphertext = cipher.encrypt(pad(FLAG, 16))
    # Prepare data to send
    data = {}
    data['iv'] = iv.hex()
    data['encrypted_flag'] = ciphertext.hex()
    return data


# Define the curve
p = 310717010502520989590157367261876774703
a = 2
b = 3

# Generator
g_x = 179210853392303317793440285562762725654
g_y = 105268671499942631758568591033409611165
G = Point(g_x, g_y)

# My secret int, different every time!!
n = randint(1, p)

# Send this to Bob!
public = double_and_add(G, n)
print(public)

# Bob's public key
b_x = 272640099140026426377756188075937988094
b_y = 51062462309521034358726608268084433317
B = Point(b_x, b_y)

# Calculate Shared Secret
shared_secret = gen_shared_secret(B, n)

# Send this to Bob!
ciphertext = encrypt_flag(shared_secret)
print(ciphertext)
```
:::
::: spoiler File output.txt
```txt
Point(x = 280810182131414898730378982766101210916, y = 291506490768054478159835604632710368904)

{'iv': '07e2628b590095a5e332d397b8a59aa7', 'encrypted_flag': '8220b7c47b36777a737f5ef9caa2814cf20c1c1ef496ec21a9b4833da24a008d0870d3ac3a6ad80065c138a2ed6136af'}
```
:::

##### b) Phân tích cách làm:
- **Vấn đề:**
    - Private key $n$ phải được giữ bí mật, ta đã có $Q = nG$, nhưng việc tính $n$ từ $Q$ bằng cách giải ECDLP - Elliptic Curve Discrete Logarithm Problem (Pollard - Rho/Baby-Step Giant-Step) là rất khó nếu đường cong đủ mạnh ($p$ lớn, order lớn).
    - Từ `source.py`, có thể thấy đường cong được dùng là rất nhỏ (~$128$ bits), private key $n$ nằm trong khoảng $[1, p]$ và ta đã biết được $Q$ (hay `public`).
    - **Hướng khai thác:** Ở challenge này `order(G)` không phải prime mà có **factorization smooth**, có thể chia thành nhiều bài toán nhỏ trên các subgroup có order $q_i$ $\rightarrow$ **Pohlig - Hellman**:
        - Tính $N = ord(G)$ và factor thành các danh sách $(p_i, e_i)$.
        - Với mỗi $(p_i, e_i)$:
            - $q_i = p_i^{e_i}$
            - $m_i = \frac{N}{q_i}$
            - $H_i = m_i.G$ (generator của subgroup bậc $q_i$)
            - $Y_i = m_i.public$
            - Tính $n_i \equiv log_{H_i}(Y_i) \pmod {q_i}$ (trong subgroup có order $q_i$):
                - Nếu $q_i$ prime: Dùng BSGS/`discrete_log`
                - Nếu $q_i$ prime power: Dùng phiên bản p - adic của Pohlig - Hellman (lấy các chữ số).
            - Lưu $n_i$, $q_i$, ta có $n \equiv n_i \pmod {q_i} \forall i$
        - Ráp $n = CRT((n_i), (q_i))$
        - Shared secret: $S = nB$. Sau đó dùng AES để tìm flag.
- Source code SageMath:
```py
from sage.all import *
import hashlib
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad


p = 310717010502520989590157367261876774703
a = 2
b = 3

E = EllipticCurve(GF(p), [a, b])

g_x = 179210853392303317793440285562762725654
g_y = 105268671499942631758568591033409611165
G = E(g_x, g_y)

pub_x = 280810182131414898730378982766101210916
pub_y = 291506490768054478159835604632710368904
Public = E(pub_x, pub_y)

#Public key của Bob:
b_x = 272640099140026426377756188075937988094
b_y = 51062462309521034358726608268084433317
B = E(b_x, b_y)

ordG = G.order()
print("Order của G: ord(G) =", ordG)
factors = factor(ordG)
print("Factors của ord(G):", factors)

#Pohlig – Hellman trên từng prime power:
n_list = []
mod_list = []
for prime, exp in factors:
    q = prime**exp
    #Generator của subgroup order q:
    H = (ordG//q)*G
    H_Pub = (ordG//q)*Public
    #Discrete log H_Pub base H modulo q:
    ni = discrete_log(H_Pub, H, operation='+')
    n_list.append(ni)
    mod_list.append(q)

#CRT
n = crt(n_list, mod_list)
print("n =", n)
S = n*B
xS = int(S[0])


backpack = {'iv': '07e2628b590095a5e332d397b8a59aa7', 'encrypted_flag': '8220b7c47b36777a737f5ef9caa2814cf20c1c1ef496ec21a9b4833da24a008d0870d3ac3a6ad80065c138a2ed6136af'}
iv = bytes.fromhex(backpack['iv'])
ct = bytes.fromhex(backpack['encrypted_flag'])
key = hashlib.sha1(str(xS).encode()).digest()[:16]
cipher = AES.new(key, AES.MODE_CBC, iv)
flag = unpad(cipher.decrypt(ct), 16)
print(flag)
```
``` ubuntu
Order của G: ord(G) = 155358505251260494795103074529582338902
Factors của ord(G): 2 * 3^7 * 139 * 165229 * 31850531 * 270778799 * 179317983307
n = 47836431801801373761601790722388100620
b'crypto{n07_4ll_curv3s_4r3_s4f3_curv3s}'
```

##### c) Kết quả:
`crypto{n07_4ll_curv3s_4r3_s4f3_curv3s}`

#### 2. Exceptional Curves:
##### a) Mô tả đề bài:
- *"Rút kinh nghiệm từ những sai lầm... Lần này tôi đã đảm bảo đường cong của mình là prime order. Lá cờ này sẽ an toàn mãi mãi."*
- Source đề cho:
::: spoiler `source.sage`
```py
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
from random import randint
import hashlib

FLAG = b'crypto{??????????????????????}'


def gen_public_key():
    private = randint(1, E.order() - 1)
    public = G * private
    return(public, private)


def shared_secret(public_key, private_key):
    S = public_key * private_key
    return S.xy()[0]


def encrypt_flag(flag):
    # Bob's public key
    b_x = 0x7f0489e4efe6905f039476db54f9b6eac654c780342169155344abc5ac90167adc6b8dabacec643cbe420abffe9760cbc3e8a2b508d24779461c19b20e242a38
    b_y = 0xdd04134e747354e5b9618d8cb3f60e03a74a709d4956641b234daa8a65d43df34e18d00a59c070801178d198e8905ef670118c15b0906d3a00a662d3a2736bf
    B = E(b_x, b_y)
    # Calculate shared secret
    A, nA = gen_public_key()
    print(f'Public Key: {A}')
    secret = shared_secret(B, nA)
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Encrypt flag
    iv = os.urandom(16)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    ciphertext = cipher.encrypt(pad(FLAG, 16))
    # Prepare encryption to send
    data = {}
    data['iv'] = iv.hex()
    data['encrypted_flag'] = ciphertext.hex()
    return data


# Curve params
p = 0xa15c4fb663a578d8b2496d3151a946119ee42695e18e13e90600192b1d0abdbb6f787f90c8d102ff88e284dd4526f5f6b6c980bf88f1d0490714b67e8a2a2b77
a = 0x5e009506fcc7eff573bc960d88638fe25e76a9b6c7caeea072a27dcd1fa46abb15b7b6210cf90caba982893ee2779669bac06e267013486b22ff3e24abae2d42
b = 0x2ce7d1ca4493b0977f088f6d30d9241f8048fdea112cc385b793bce953998caae680864a7d3aa437ea3ffd1441ca3fb352b0b710bb3f053e980e503be9a7fece

# Define curve
E = EllipticCurve(GF(p), [a, b])

# Protect against Pohlig-Hellman Algorithm
assert is_prime(E.order())

# Create generator
G = E.gens()[0]
print(f'Generator: {G}')

encrypted_flag = encrypt_flag(FLAG)
print(encrypted_flag)
```
:::
::: spoiler `output.txt`
```py
Generator: (3034712809375537908102988750113382444008758539448972750581525810900634243392172703684905257490982543775233630011707375189041302436945106395617312498769005 : 4986645098582616415690074082237817624424333339074969364527548107042876175480894132576399611027847402879885574130125050842710052291870268101817275410204850 : 1)
Public Key: (4748198372895404866752111766626421927481971519483471383813044005699388317650395315193922226704604937454742608233124831870493636003725200307683939875286865 : 2421873309002279841021791369884483308051497215798017509805302041102468310636822060707350789776065212606890489706597369526562336256272258544226688832663757 : 1)
{'iv': '719700b2470525781cc844db1febd994', 'encrypted_flag': '335470f413c225b705db2e930b9d460d3947b3836059fb890b044e46cbb343f0'}
```
:::

##### b) Phân tích cách làm:
- Theo đề bài, có thể thấy challenge tiếp theo không sử dụng được Pohlig - Hellman nữa vì người ra đề đã sử dụng **đường cong có bậc nguyên tố lớn** (không có subgroup attack kiểu nhỏ).
- Check các thông số của challenge, ta thấy rằng số lượng các điểm trên đường cong bằng đúng modulus ($order(E) = p$) hay: $$\#E(\mathbb{F}_p) = p$$ Khi đó đường cong Elliptic được gọi là **đường cong bất quy tắc (Anomalous curve)**
**$\rightarrow$ Smart Attack**
- Lý thuyết:
    - Order: $\#E(\mathbb{F}_p) = p + 1 - t$ với $|t| \leq 2\sqrt{p}$ (Định lý Hasse, $t$ được gọi là **Trace of Frobenius**)
    - Nếu $\#E(\mathbb{F}_p) = p \rightarrow t = 1 \rightarrow$ Điều kiện để Smart Attack.
    - Smart Attack: Có một ánh xạ từ $(E(F_p))$ về nhóm cộng $(F_p, +)$ cho phép biến bài toán Log Elliptic thành bài toán log thông thường trên trường $F_p$, giải được nhanh bằng Euclidean algorithm.
        - Với $E/F_p$ anomalous, có phép "Frobenius endomorphism" $\phi: (x, y) \mapsto (x^p, y^p)$ với tính chất $\phi (P) = P + \mathcal{O}$
        - Sử dụng phép nâng lên $Q_p$ (p-adic numbers) để tính log:
            - Nâng $P$, $Q$ từ $E(F_p)$ sang $E(Q_p)$.
            - Dùng công thức: $$log_G(P) \equiv \frac{(x_P/y_P)}{(x_G/y_G)} \pmod p$$
            - Nếu độ mở rộng $n > 1$, dùng công thức lặp để tính log modulo $p^i$
- Tool (lụm trên mạng) dùng để giải Smart Attack: [Source code](https://github.com/jvdsn/crypto-attacks/blob/master/attacks/ecc/smart_attack.py)
- Từ tool mạng và Chat GPT, ta có source code:
```py
import logging
from sage.all import *
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import hashlib


def _gf_to_qq(n, qq, x):
    return ZZ(x) if n == 1 else qq(list(map(int, x.polynomial())))

def _lift(E, p, Px, Py):
    for P in E.lift_x(Px, all = True):
        if (P.xy()[1] % p) == Py:
            return P

def smart_attack(G, P):
    E = G.curve()
    assert E.trace_of_frobenius() == 1, f"Curve should have trace of Frobenius = 1."

    F = E.base_ring()
    p = F.characteristic()
    q = F.order()
    n = F.degree()
    qq = Qq(q, names = "g")

    logging.info(f"Computing l % {p}...")
    E = EllipticCurve(qq, [_gf_to_qq(n, qq, a) + q * ZZ.random_element(1, q) for a in E.a_invariants()])
    Gx, Gy = _gf_to_qq(n, qq, G.xy()[0]), _gf_to_qq(n, qq, G.xy()[1])
    Gx, Gy = (q * _lift(E, p, Gx, Gy)).xy()
    Px, Py = _gf_to_qq(n, qq, P.xy()[0]), _gf_to_qq(n, qq, P.xy()[1])
    Px, Py = (q * _lift(E, p, Px, Py)).xy()
    l = ZZ(((Px / Py) / (Gx / Gy)) % p)

    if n > 1:
        G0 = p ** (n - 1) * G
        G0x, G0y = _gf_to_qq(n, qq, G0.xy()[0]), _gf_to_qq(n, qq, G0.xy()[1])
        G0x, G0y = (q * _lift(E, p, G0x, G0y)).xy()
        for i in range(1, n):
            logging.info(f"Computing l % {p ** (i + 1)}...")
            Pi = p ** (n - i - 1) * (P - l * G)
            if Pi.is_zero():
                continue

            Pix, Piy = _gf_to_qq(n, qq, Pi.xy()[0]), _gf_to_qq(n, qq, Pi.xy()[1])
            Pix, Piy = (q * _lift(E, p, Pix, Piy)).xy()
            l += p ** i * ZZ(((Pix / Piy) / (G0x / G0y)) % p)

    return int(l)


p = 0xa15c4fb663a578d8b2496d3151a946119ee42695e18e13e90600192b1d0abdbb6f787f90c8d102ff88e284dd4526f5f6b6c980bf88f1d0490714b67e8a2a2b77
a = 0x5e009506fcc7eff573bc960d88638fe25e76a9b6c7caeea072a27dcd1fa46abb15b7b6210cf90caba982893ee2779669bac06e267013486b22ff3e24abae2d42
b = 0x2ce7d1ca4493b0977f088f6d30d9241f8048fdea112cc385b793bce953998caae680864a7d3aa437ea3ffd1441ca3fb352b0b710bb3f053e980e503be9a7fece

E = EllipticCurve(GF(p), [a,b])
print("Order of E: ord(E) =", E.order())
print("p =", p)

G = E.gens()[0]
print("Generator: G =", G)

# ---- Bob's public key ----
b_x = 0x7f0489e4efe6905f039476db54f9b6eac654c780342169155344abc5ac90167adc6b8dabacec643cbe420abffe9760cbc3e8a2b508d24779461c19b20e242a38
b_y = 0xdd04134e747354e5b9618d8cb3f60e03a74a709d4956641b234daa8a65d43df34e18d00a59c070801178d198e8905ef670118c15b0906d3a00a662d3a2736bf
B = E(b_x,b_y)

pub_x = 4748198372895404866752111766626421927481971519483471383813044005699388317650395315193922226704604937454742608233124831870493636003725200307683939875286865
pub_y = 2421873309002279841021791369884483308051497215798017509805302041102468310636822060707350789776065212606890489706597369526562336256272258544226688832663757
A = E(pub_x, pub_y)

nA = smart_attack(G, A)
print("Secret scalar nA =", nA)

S = nA*B
xS = int(S[0])

key = hashlib.sha1(str(xS).encode()).digest()[:16]
iv = bytes.fromhex("719700b2470525781cc844db1febd994")
ct = bytes.fromhex("335470f413c225b705db2e930b9d460d3947b3836059fb890b044e46cbb343f0")

cipher = AES.new(key, AES.MODE_CBC, iv)
flag = unpad(cipher.decrypt(ct),16)
print("FLAG:", flag)
```
- Output:
``` ubuntu
Order of E: ord(E) = 8451139905551902831160354990243448994735923344327179631310525520435196861496551764162970081762137226285330762159796842785356834064520159547540428116601719
p = 8451139905551902831160354990243448994735923344327179631310525520435196861496551764162970081762137226285330762159796842785356834064520159547540428116601719
Generator: G = (3034712809375537908102988750113382444008758539448972750581525810900634243392172703684905257490982543775233630011707375189041302436945106395617312498769005 : 4986645098582616415690074082237817624424333339074969364527548107042876175480894132576399611027847402879885574130125050842710052291870268101817275410204850 : 1)
Secret scalar nA = 2200911270816846838022388357422161552282496835763864725672800875786994850585872907705630132325051034876291845289429009837283760741160188885749171857285407
FLAG: b'crypto{H3ns3l_lift3d_my_fl4g!}'
```

##### c) Kết quả:
`crypto{H3ns3l_lift3d_my_fl4g!}`

#### 3. Micro Transmissions:
##### 1. Tóm tắt đề bài:
- *"Đã tinh chỉnh độ dài bit của mình để đảm bảo các gói dữ liệu và thời gian tính toán của tôi cực kỳ hiệu quả."*
- Source code:
::: spoiler source.sage
```py
from Crypto.Util.number import getPrime
from Crypto.Random import random
from Crypto.Cipher import AES
from Crypto.Util.number import inverse
from Crypto.Util.Padding import pad, unpad
import hashlib

FLAG = b"crypto{???????????????????}"


def gen_key_pair(G, nbits):
    n = random.getrandbits(nbits)
    P = n*G
    return P.xy()[0], n


def gen_shared_secret(P, n):
	S = n*P
	return S.xy()[0]


def encrypt_flag(shared_secret: int):
    # Derive AES key from shared secret
    sha1 = hashlib.sha1()
    sha1.update(str(shared_secret).encode('ascii'))
    key = sha1.digest()[:16]
    # Encrypt flag
    iv = os.urandom(16)
    cipher = AES.new(key, AES.MODE_CBC, iv)
    ciphertext = cipher.encrypt(pad(FLAG, 16))
    # Prepare data to send
    data = {}
    data['iv'] = iv.hex()
    data['encrypted_flag'] = ciphertext.hex()
    return data


# Efficient key exchange
nbits = 64
pbits = 256

# Curve parameters
p = getPrime(pbits)
a = 1
b = 4
E = EllipticCurve(GF(p), [a,b])
G = E.gens()[0]

print(f"Sending curve parameters:")
print(f"{E}")
print(f"Generator point: {G}\n")

# Generate key pairs
ax, n_a = gen_key_pair(G, nbits)
bx, n_b = gen_key_pair(G, nbits)

print(f"Alice sends public key: {ax}")
print(f"Bob sends public key: {bx}\n")

# Calculate point from Bob
B = E.lift_x(bx)

# Encrypted flag with shared secret
shared_secret = gen_shared_secret(B,n_a)
encrypted_flag = encrypt_flag(shared_secret)

print(f"Alice sends encrypted_flag: {encrypted_flag}")
```
:::
::: spoiler output.txt
```
Sending curve parameters:
Elliptic Curve defined by y^2 = x^3 + x + 4 over Finite Field of size 99061670249353652702595159229088680425828208953931838069069584252923270946291
Generator point: (43190960452218023575787899214023014938926631792651638044680168600989609069200 : 20971936269255296908588589778128791635639992476076894152303569022736123671173 : 1)

Alice sends public key: 87360200456784002948566700858113190957688355783112995047798140117594305287669
Bob sends public key: 6082896373499126624029343293750138460137531774473450341235217699497602895121

Alice sends encrypted_flag: {'iv': 'ceb34a8c174d77136455971f08641cc5', 'encrypted_flag': 'b503bf04df71cfbd3f464aec2083e9b79c825803a4d4a43697889ad29eb75453'}
```
:::
##### 2. Phân tích cách làm:
- Hướng giải:
    - Từ `source.sage`, trong hàm `gen_key_pair()` có thể thấy rằng private `n = random.getrandbits(nbits)` với `nbits = 64` (thay vì $256$ bits) **$\rightarrow$ Private key rất nhỏ ($\leq 2^{64}$)**.
    - Check order của $G$ thì ta thấy rằng đây là một **smooth number**: `ord(G) = 7 * 11 * 17 * 191 * 317 * 331 * 5221385621 * 5397618469 * 210071842937040101 * 637807437018177170959577732683`
    - Từ hai yếu tố trên và search trên mạng (cụ thể là [link](https://github.com/elikaski/ECC_Attacks?tab=readme-ov-file#The-order-of-the-generator-is-almost-a-smooth-number-and-the-private-key-is-small) và ChatGPT), ta đã có được hướng giải dành cho challenge này **$\rightarrow$ Partical Pohlig - Hellman Attack.**
- Phân tích:
    - Pohlig - Hellman chuẩn:
        - Thuật toán Pohlig - Hellman giải bài toán rời rạc trong nhóm có bậc $n = \displaystyle \prod p_i^{e_i}$ bằng cách:
            - Giải log trong từng nhóm con có bậc $p_i^{e_i}$.
            - Ghép kết quả lại bằng CRT.
        - Nghĩa là ta phải tính được tất cả các $k \mod p_i^{e_i}$ cho mọi thừa số của $n$. Nếu có một thừa số lớn thì việc giải ECDLP là bất khả thi.
    - Khi private key nhỏ hơn bậc nhóm $n$: Trong challenge, bậc nhóm $n ~ 256$ bits, nhưng private key $k < 2^{64}$. Ta không cần biết $k \mod n$, chỉ cần biết $k$ modulo một tích $M$ đủ lớn để phân biệt tất cả các khoá nhỏ: $$M \geq 2^{64}$$ Vì $k < 2^{64} \leq M$ nên chỉ cần CRT sẽ cho ra chính xác $k$.
    - Thuật toán:
        - Chọn tập con $I$ các thừa số nhỏ $p_i^{e_i}$ sao cho $\displaystyle \prod p_i^{e_i} \geq 2^{64}$.
        - Giải discrete log trên các nhóm con này. Với mỗi $p_i^{e_i}$ đã chọn:
            - Đặt $m_i = n/p_i^{e_i}$.
            - Điểm cơ sở cho subgroup là $G_i = m_iG$ bởi vì $ord(G_i) = p_i^{e_i}$.
            - Giải ECDLP nhỏ bằng hàm `discrete_log`: Tìm $k_i$ sao cho $A_i = k_i.G_i$.
            - Kết quả tìm được sẽ là $k_A \mod p_i^{e_i} = k_i$.
        - Dùng CRT ghép lại $\widetilde{k} \in [0, M)$ từ đồng dư $k_A \equiv k_i \pmod {p_i^{e_i}}$.
        - Do $k < 2^{64} \leq M$ nên $\widetilde{k} = k$.
- Source code:
``` py
#!/usr/bin/env sage
from sage.all import *
from hashlib import sha1
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad
import binascii

p = 99061670249353652702595159229088680425828208953931838069069584252923270946291
E = EllipticCurve(GF(p), [1,4])
G = E(43190960452218023575787899214023014938926631792651638044680168600989609069200, 20971936269255296908588589778128791635639992476076894152303569022736123671173)
ax = 87360200456784002948566700858113190957688355783112995047798140117594305287669
bx = 6082896373499126624029343293750138460137531774473450341235217699497602895121
iv_hex = 'ceb34a8c174d77136455971f08641cc5'
ct_hex = 'b503bf04df71cfbd3f464aec2083e9b79c825803a4d4a43697889ad29eb75453'
iv = binascii.unhexlify(iv_hex)
ct = binascii.unhexlify(ct_hex)

def lifts(x):
    F = GF(p)
    rhs = F(x)**3 + F(1)*F(x) + F(4)
    return [] if not rhs.is_square() else [E(x, rhs.sqrt()), E(x, -rhs.sqrt())]

n = G.order(); fac = list(n.factor())
prod = 1
sel = []
for q, e in fac:
    sel.append((q,e))
    prod *= q**e
    if prod.nbits() >= 64: 
        break

for A in lifts(ax):
    subs = []
    mods = []
    for q, e in sel:
        m = n//(q**e)
        k = (m*G).discrete_log(m*A)
        subs.append(k); mods.append(q**e)
    n_a = crt(subs, mods)
    if n_a*G == A:
        for B in lifts(bx):
            S = n_a*B
            key = sha1(str(int(S[0])).encode()).digest()[:16]
            print("FLAG:", unpad(AES.new(key, AES.MODE_CBC, iv).decrypt(ct), 16))
            break
```

##### 3. Kết quả:
`crypto{d0nt_l3t_n_b3_t00_sm4ll}`

### IV. SIGNATURES:
#### 1. Digestive:
##### a) Mô tả đề bài:
- *"Should ECDSA be avoided for better digestive health?"*
- [Link được cho](https://web.cryptohack.org/digestive)
- Source code trong link:
::: spoiler Source code
```py
import hashlib
import json
import string
from ecdsa import SigningKey

SK = SigningKey.generate() # uses NIST192p
VK = SK.verifying_key


class HashFunc:
    def __init__(self, data):
        self.data = data

    def digest(self):
        # return hashlib.sha256(data).digest()
        return self.data



@chal.route('/digestive/sign/<username>/')
def sign(username):
    sanitized_username = "".join(a for a in username if a in string.ascii_lowercase)
    msg = json.dumps({"admin": False, "username": sanitized_username})
    signature = SK.sign(
        msg.encode(),
        hashfunc=HashFunc,
    )

    # remember to remove the backslashes from the double-encoded JSON
    return {"msg": msg, "signature": signature.hex()}


@chal.route('/digestive/verify/<msg>/<signature>/')
def verify(msg, signature):
    try:
        VK.verify(
            bytes.fromhex(signature),
            msg.encode(),
            hashfunc=HashFunc,
        )
    except:
        return {"error": "Signature verification failed"}

    verified_input = json.loads(msg)
    if "admin" in verified_input and verified_input["admin"] == True:
        return {"flag": FLAG}
    else:
        return {"error": f"{verified_input['username']} is not an admin"}
```
:::

##### b) Phân tích cách làm:
- Từ source code trong link, có thể thấy rằng server dùng `ecdsa` với `hashfunc=HashFunc` mà `HashFunc.digest()` trả về $m$ thay vì trả về hash $z$.
- Tham khảo từ [write - up](https://hackmd.io/@at20n0118/HkihW3dpkl#ECDSA-Attacks) của mentor Le Ha thân iu thì ta biết rằng $z$ chính là các bit đầu của $m$, hay nói cách khác một chữ kí có thể được dùng để xác thự nhiều tin nhắn khác nhau. Cụ thể trong challenge trên thì giá trị $z$ là $192$ bit đầu của thông điệp, khi này có thể dùng chữ ký của văn bản này để xác thực cho văn bản khác nếu $192$ bit đầu trong văn bản giống nhau.
- Thử nhờ GPT một đoạn code kết nối đến server, thì output trả về cho thấy $z$ giống nhau thật:
::: spoiler Source check z
``` py
import requests, json
from ecdsa import NIST192p

BASE = "https://web.cryptohack.org"
PATH = "/digestive/sign/{}/"
OUT = "samples.json"
COUNT = 12

n = int(NIST192p.order)

out = []
for _ in range(COUNT):
    r = requests.get(BASE + PATH.format("user/"), timeout=10).json()
    msg = r["msg"]
    sig = bytes.fromhex(r["signature"])
    qlen = len(sig) // 2
    r_int = int.from_bytes(sig[:qlen], "big")
    s_int = int.from_bytes(sig[qlen:], "big")
    z = int.from_bytes(msg.encode(), "big") % n
    out.append({
        "msg": msg,
        "r": int(r_int),
        "s": int(s_int),
        "z": int(z),
        "sighex": r["signature"]
    })

with open(OUT, "w") as f:
    json.dump(out, f, indent=2)

print(f"[+] Saved {OUT} with {len(out)} entries.")
```
:::
::: spoiler Output
``` bash
[
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 1462858629729557953753784632195824236631467626551598653786,
    "s": 4995481254258947877217069992185207599907212955820906853602,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "3ba8f4ebe08abe79770c89a2ba040dd6d7b4e7dac290215acbbb42019d8f30cdb4aa14cf0fa45f8877722551a8ad34e2"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 3083761577532584419129488215385247202764850265346906272110,
    "s": 5597948521080470368768787749560478250355757474123306837911,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "7dc3f91e678925c2dc00a2c8dd2d4e251358685836dc316ee44d4f418cee352a6aa9ae36ee33a063dfe36419942d2797"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 4237880752327608506761779080590019068154164347286890009421,
    "s": 3100310784599648514879132451145352161233149733658463155552,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "acd58a1ed28c1a8706c0c96551c8c40b5353c3d20e88d34d7e70c14161a4ff7b8ef263ef03858ec6d1415beff841b560"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 4592716890219432062792174495859008574134007299385965419583,
    "s": 3507854760540294387252997770682312330325374517076416266466,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "bb4e33a0abe477c536b9b55f4326d9daa82e7b7cc4a41c3f8f0fb6697f73a15448ad71ae11560d46513ebc153a5000e2"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 3177051554719322912294249866084793589690735526775773820855,
    "s": 1224793280361225875926532668382360323257004597063702283469,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "8191f747109eb6181d4df4755753cfae2ed699c95b7177b731f37041c4b14f7846bcafd3c09ffacac8f89077d44004cd"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 102453818033437242807764811656011634859225052836770400492,
    "s": 1914382967766243683112162259235768865439681671840171028584,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "042daaf3024f0913c8214a13e7275cad4192cae20730c8ec4e13173ce38d89a535a7a6785b1661f486d09c354de3f068"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 1337286018012124661234166105965727585954461392511045682332,
    "s": 5371404119031043306554404774599451838161611999651211772183,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "3689eae0516e140eec315901b0e695621333cbcdd82f449cdb10135f7c46553ffa47ef9ec9f0917b5f835beeb5c80117"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 2930946158331996354399618441035671234435902398505398016712,
    "s": 5028715002916281346327082555301911053016632611102885106998,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "778881758c2b280e5b5554747ce84cdf86e20194bc439ac8cd163bff353818fa20945a431061b408192db95d48cf5536"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 5758206939916278615378214538259036018825111446734246860350,
    "s": 4280380186263496991293281212378025868753070650325353112156,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "ead67c4d6b25eefefcc579593704f8bc2f845be6d2ad563eae91411b8d2d358e9250dbe3a9d0ddec0613f1593694225c"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 3169616343821451091171229888670790327503991140477162132887,
    "s": 2369338541698515764651456114574155346679139605681263771360,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "814456b552fae7d7a0a98450eadbcaacf158b426899e959760a10c70c5776a104d9cc1d8522ee6ac0221b6d1a9d67ee0"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 1608942675694999494097666288642052838935287637040241287869,
    "s": 5410270672680743581134776509513907542812230024312189960092,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "419e253443c2143d90eec19ff364f41e81cb68dad9cfa2bddca5dc86a723df5bbe84b19bb24278441dccf89084b93f9c"
  },
  {
    "msg": "{\"admin\": false, \"username\": \"user\"}",
    "r": 871711015156785380567969191505896937977754218197376490798,
    "s": 1086292863040913044439644787584425524686266118029238988239,
    "z": 3863706788818792906039755766317957800530077431251491185132,
    "sighex": "238d16704f4761d35b25ca2fdfa7bec2ae857b3aee06dd2e2c4d6d3aaa47b068a2b6513c9eebd311c458ab8a8f95edcf"
  }

```
:::
- Chiến lược ở đây là sinh nhiều biến thể JSON hợp lệ và thử tất cả `sighex` thu được rồi gửi vào`/verify/<msg>/<sig>/`.
- Với sự trợ giúp của ChatGPT, kết nối tới file output `samples.json`:
``` py
import json, re, requests, urllib.parse, time

BASE = "https://web.cryptohack.org"
VERIFY = "/digestive/verify/{msg}/{sig}/"
DELAY = 0.12
data = json.load(open("samples.json"))
sigs = [e["sighex"] for e in data]
s = data[0]["msg"]
def ev(u):
    vs = {u}
    for i, ch in enumerate(u): vs.add(u[:i] + "\\u%04x"%ord(ch) + u[i + 1:])
    vs |= {v.replace("\\", "\\\\") for v in list(vs)}
    return vs
def gen():
    if s.endswith("}"): b = s[:-1]
    else: b = s
    for ins in (', "admin": true}',', "admin": true, "x":0}',', "x":0, "admin": true}'): yield b+ins
    for i, ch in enumerate(s):
        if ch == ',':
            for ins in (', "admin": true',',\"admin\": true',', "admin": true, "x": 0'): yield s[:i + 1] + ins + s[i + 1:]
    for order in (("admin","username"),("username","admin")):
        for sp1 in ("", " ", "  ", "\t", "\n"):
            for sp2 in ("", " ", "  ", "\t", "\n"):
                for extra in (None, ('"x"', '0'), ('"nonce"', '0')):
                    if order[0] == "admin":
                        parts=[f"\"admin\":{sp1}true"]
                        if extra: parts.append(f"{sp2}{extra[0]}:{sp2}{extra[1]}")
                        parts.append(f"{sp2}\"username\":{sp2}\"user\"")
                    else:
                        parts = [f"\"username\": {sp1}\"user\""]
                        if extra: parts.append(f"{sp2}{extra[0]}:{sp2}{extra[1]}")
                        parts.append(f"{sp2}\"admin\":{sp2}true")
                    s0 = "{" + ",".join(parts) + "}"
                    yield s0; yield json.dumps(s0)
    for uname in ("user", "user1", "admin", "test", "alice"):
        for v in ev(uname):
            s0 = f'{{"admin": true, "username": "{v}"}}'
            yield s0; yield json.dumps(s0)
    for p in ("", "\"pad\":\"A\"", "\"pad\":\"AAAAAAAA\"", "\"pad\":\"" + ("A"*50) + "\""):
        yield b + (', ' + p if p else '') + ', "admin": true}'
seen = set()
sess = requests.Session()
pat = re.compile(r"crypto\{.*?\}")
for cand in gen():
    if cand in seen: continue
    seen.add(cand)
    me = urllib.parse.quote(cand, safe = '')
    for sig in sigs:
        try:
            r = sess.get(BASE + VERIFY.format(msg = me,sig = sig), timeout = 10)
        except:
            time.sleep(DELAY); continue
        m = pat.search(r.text)
        if m:
            print(m.group(0))
            raise SystemExit
        time.sleep(DELAY)
```

##### c) Kết quả:
`crypto{thanx_for_ctf_inspiration_https://mastodon.social/@filippo/109360453402691894}`

#### 2. Curveball:
##### a) Mô tả đề bài:
- *"Here's my secure search engine, which will only search for hosts it has in its trusted certificate cache."*
- Connect at `socket.cryptohack.org 13382`.
- Source code được cho:
::: spoiler `13382.py`
``` py
#!/usr/bin/env python3

import fastecdsa
from fastecdsa.point import Point
from fastecdsa.curve import P256
from utils import listener


FLAG = "crypto{????????????????????????????????????}"
G = P256.G
assert G.x, G.y == [0x6B17D1F2E12C4247F8BCE6E563A440F277037D812DEB33A0F4A13945D898C296,
                    0x4FE342E2FE1A7F9B8EE7EB4A7C0F9E162BCE33576B315ECECBB6406837BF51F5]


class Challenge():
    def __init__(self):
        self.before_input = "Welcome to my secure search engine backed by trusted certificate library!\n"
        self.trusted_certs = {
            'www.cryptohack.org': {
                "public_key": Point(0xE9E4EBA2737E19663E993CF62DFBA4AF71C703ACA0A01CB003845178A51B859D, 0x179DF068FC5C380641DB2661121E568BB24BF13DE8A8968EF3D98CCF84DAF4A9, curve=P256),
                "curve": "secp256r1",
                "generator": [G.x, G.y]
            },
            'www.bing.com': {
                "public_key": Point(0x3B827FF5E8EA151E6E51F8D0ABF08D90F571914A595891F9998A5BD49DFA3531, 0xAB61705C502CA0F7AA127DEC096B2BBDC9BD3B4281808B3740C320810888592A, curve=P256),
                "curve": "secp256r1",
                "generator": [G.x, G.y]
            },
            'www.gchq.gov.uk': {
                "public_key": Point(0xDEDFC883FEEA09DE903ECCB03C756B382B2302FFA296B03E23EEDF94B9F5AF94, 0x15CEBDD07F7584DBC7B3F4DEBBA0C13ECD2D2D8B750CBF97438AF7357CEA953D, curve=P256),
                "curve": "secp256r1",
                "generator": [G.x, G.y]
            }
        }

    def search_trusted(self, Q):
        for host, cert in self.trusted_certs.items():
            if Q == cert['public_key']:
                return True, host
        return False, None

    def sign_point(self, g, d):
        return g * d

    def connection_host(self, packet):
        d = packet['private_key']
        if abs(d) == 1:
            return "Private key is insecure, certificate rejected."
        packet_host = packet['host']
        curve = packet['curve']
        x, y = packet['generator']
        g = Point(x, y, curve=P256)
        Q = self.sign_point(g, d)
        cached, host = self.search_trusted(Q)
        if cached:
            return host
        else:
            self.trusted_certs[packet_host] = {
                "public_key": Q,
                "curve": "secp256r1",
                "generator": G
            }
            return "Site added to trusted connections"

    def bing_it(self, s):
        return f"Hey bing! Tell me about {s}"

    #
    # This challenge function is called on your input, which must be JSON
    # encoded
    #
    def challenge(self, your_input):
        host = self.connection_host(your_input)
        if host == "www.bing.com":
            return self.bing_it(FLAG)
        else:
            return self.bing_it(host)


import builtins; builtins.Challenge = Challenge # hack to enable challenge to be run locally, see https://cryptohack.org/faq/#listener
listener.start_server(port=13382)
```
:::

##### b) Phân tích cách làm:
- Từ source code server, có thể thấy rằng client được phép gửi `g` và `d` rồi tính `Q = g*d` rồi so sánh với các `public_key` đã lưu.
- Vì flag sẽ nằm ở server `www.bing.com` nên ta sẽ chọn `d` khác $1$ và `g` sao cho `g*d = Q_bing`: $$g \equiv d^{-1}.Q_{bing} \pmod n$$
- Server tính `Q` và so sánh với `Q_bing`: $$Q \equiv g.d \pmod n$$ $$\rightarrow Q \equiv d^{-1}.Q_{bing}.d \pmod n$$ $$\leftrightarrow Q \equiv Q_{bing} \pmod n$$ Khi này server sẽ trả về flag.
- Source code:
``` py
#!/usr/bin/env python3
import socket, json
from ecdsa import curves, ellipticcurve

Qx = 0x3B827FF5E8EA151E6E51F8D0ABF08D90F571914A595891F9998A5BD49DFA3531
Qy = 0xAB61705C502CA0F7AA127DEC096B2BBDC9BD3B4281808B3740C320810888592A
curve = curves.NIST256p.curve
n = curves.NIST256p.order

Q = ellipticcurve.Point(curve, Qx, Qy)
d = 2
inv_d = pow(d, -1, n)
g = Q * inv_d

packet = {"private_key": d, "host": "attacker.local", "curve": "secp256r1", "generator": [int(g.x()), int(g.y())]}

s = socket.create_connection(("socket.cryptohack.org", 13382))
s.sendall((json.dumps(packet) + "\n").encode())
print(s.recv(4096).decode(errors="ignore"))
s.close()
```
- Output:
``` ubuntu
Welcome to my secure search engine backed by trusted certificate library!
"Hey bing! Tell me about crypto{Curveballing_Microsoft_CVE-2020-0601}"
```

##### c) Kết quả:
`crypto{Curveballing_Microsoft_CVE-2020-0601}`

#### 3. ProSign 3:
##### a) Mô tả đề bài:
- *"This is my secure timestamp signing server. Only if you can produce a signature for "unlock" can you learn more."*
- Kết nối tại `socket.cryptohack.org 13381`
- Source code đề cho:
::: spoiler `13381.py`
``` py#!/usr/bin/env python3

import hashlib
from Crypto.Util.number import bytes_to_long, long_to_bytes
from ecdsa.ecdsa import Public_key, Private_key, Signature, generator_192
from utils import listener
from datetime import datetime
from random import randrange

FLAG = "crypto{?????????????????????????}"
g = generator_192
n = g.order()


class Challenge():
    def __init__(self):
        self.before_input = "Welcome to ProSign 3. You can sign_time or verify.\n"
        secret = randrange(1, n)
        self.pubkey = Public_key(g, g * secret)
        self.privkey = Private_key(self.pubkey, secret)

    def sha1(self, data):
        sha1_hash = hashlib.sha1()
        sha1_hash.update(data)
        return sha1_hash.digest()

    def sign_time(self):
        now = datetime.now()
        m, n = int(now.strftime("%m")), int(now.strftime("%S"))
        current = f"{m}:{n}"
        msg = f"Current time is {current}"
        hsh = self.sha1(msg.encode())
        sig = self.privkey.sign(bytes_to_long(hsh), randrange(1, n))
        return {"msg": msg, "r": hex(sig.r), "s": hex(sig.s)}

    def verify(self, msg, sig_r, sig_s):
        hsh = bytes_to_long(self.sha1(msg.encode()))
        sig_r = int(sig_r, 16)
        sig_s = int(sig_s, 16)
        sig = Signature(sig_r, sig_s)

        if self.pubkey.verifies(hsh, sig):
            return True
        else:
            return False

    #
    # This challenge function is called on your input, which must be JSON
    # encoded
    #
    def challenge(self, your_input):
        if 'option' not in your_input:
            return {"error": "You must send an option to this server"}

        elif your_input['option'] == 'sign_time':
            signature = self.sign_time()
            return signature

        elif your_input['option'] == 'verify':
            msg = your_input['msg']
            r = your_input['r']
            s = your_input['s']
            verified = self.verify(msg, r, s)
            if verified:
                if msg == "unlock":
                    self.exit = True
                    return {"flag": FLAG}
                return {"result": "Message verified"}
            else:
                return {"result": "Bad signature"}

        else:
            return {"error": "Decoding fail"}


import builtins; builtins.Challenge = Challenge # hack to enable challenge to be run locally, see https://cryptohack.org/faq/#listener
listener.start_server(port=13381)
```
:::

##### b) Phân tích cách làm:
- Từ source code đề bài, ta có thể thấy rằng:
``` py
secret = randrange(1, n)
.
.
.
def sign_time(self):
        now = datetime.now()
        m, n = int(now.strftime("%m")), int(now.strftime("%S"))
```
`secret` được tạo bằng cách random trong `(1, n)`, trong khi đó `n` lại là số giây hiện tại. Khi đó khả năng hai thông điệp dùng chung một secret là rất cao, ta có lý thuyết:
![image](https://hackmd.io/_uploads/rkeHGHSTgl.png)
- Kết nối tới server, chạy vòng lặp gọi `sign_time` nhiều lần để thu về nhiều `(msg, r, s)`. Khi thấy hai chữ kí có cùng `r` thì dựa vào lý thuyết trên để tính `k` và `d`, sau đó `verify` đến server.
- Source code:
``` py
#!/usr/bin/env python3
import json, time
from Crypto.Util.number import bytes_to_long
from hashlib import sha1
from ecdsa.ecdsa import Public_key, Private_key, Signature, generator_192
from pwn import remote

HOST, PORT = "socket.cryptohack.org", 13381
g, n = generator_192, generator_192.order()
def hash_like_server(m): return bytes_to_long(sha1(str(m).encode()).digest())

io = remote(HOST, PORT); 
io.recvuntil(b"verify.\n")
seen = {}
while True:
    io.sendline(json.dumps({"option":"sign_time"}).encode())
    line = io.recvline().decode().strip()
    try:
        sig = json.loads(line)            
    except:
        try:
            sig = eval(line, {"Signature": Signature}) 
        except:
            continue

    if not all(k in sig for k in ("r", "s", "msg")):
        continue

    r = int(sig["r"],16); 
    s = int(sig["s"],16); 
    z = hash_like_server(sig["msg"])
    if r in seen:
        z1, s1 = seen[r]["z"], seen[r]["s"]; 
        z2, s2 = z, s
        k = ((z1 - z2)*pow(s1 - s2, -1, n))%n
        d = ((s1*k - z1)*pow(r, -1, n))%n
        break
    seen[r] = {"s": s, "z": z}

pub = Public_key(g, g * d); 
priv = Private_key(pub, d)
h = hash_like_server("unlock"); 
sig = priv.sign(h, k)
io.sendline(json.dumps({"option": "verify", "msg": "unlock", "r": hex(sig.r),"s": hex(sig.s)}).encode())
print(io.recvline().decode())
```

##### c) Kết quả:
`crypto{ECDSA_700_345y_70_5cr3wup}`
