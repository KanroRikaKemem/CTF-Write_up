Các thuật toán mật mã thực hiện biến đổi tin tức dưới dạng các số hoặc các phần tử trong không gian hữu hạn. Khi đó các phép mã và giải mã cần phải có đặc tính đóng (*closure property*) trong không gian hữu hạn. Tuy nhiên, các toán tử số học thông thường trên các số không có đặc tính này $\rightarrow$ Cần làm việc trong không gian có các cấu trúc đại số riêng biệt để đảm bảo đặc tính đóng.

### I. NHÓM:
#### 1. Nhóm:
- **Nhóm:** Nhóm $(G, \circ)$ là tập hợp G vùng với một phép toán hai ngôi (ánh xạ từ tập $G$ x $G \rightarrow G$) thoả mãn các tính chất sau với mọi $a, b, c$ thuộc $G$:
    - Tính đóng: $a \circ b \in G$
    - Tính kết hợp: $(a \circ b) \circ c = a \circ (b \circ c)$
    - Phần tử đơn vị (trung hoà): Trong $G$ tồn tại một phần tử được gọi là phần tử đơn vị $e$ sao cho với $\forall a \in G$ thì $a \circ e = e \circ a = a$
    - Phần tử nghịch đảo: Với mỗi $a \in G$ tồn tại $a^{-1}$ (phần tử nghịch đảo của $a$) sao cho $a^{-1} \circ a = a \circ a^{-1} = e$
- Toán tử $\circ$ là ký hiệu chung và có thể đại diện cho toán tử cộng, nhân hoặc các toán tử toán học khác.
> **Ví dụ:** Tập $Z_5 =$ {$0, 1, 2, 3, 4$} là một nhóm theo phép cộng modulo $5$, hoàn toàn thoả mãn bốn định lý theo bảng sau:
> ![image](https://hackmd.io/_uploads/H1hV5HiZee.png)
- **Nhóm hữu hạn và vô hạn:** Nhóm $G$ hữu hạn nếu tập $G$ có số lượng phần tử hữu hạn, ngược lại là vô hạn.
- **Nhóm Abel (Nhóm giao hoán):** Thoả mãn thêm điều kiện phép toán hai ngôi có thêm tính giao hoán với mọi $a, b, c$ thuộc $G$: $a \circ b = b \circ a$
> **Ví dụ:** Cho tập R và phép toán $\circ$: $a \circ b = (a^3 + b^3)^{\frac{1}{3}}$.
> $(R, \circ)$ là một nhóm Abel vì:
> - $a \circ b = (a^3 + b^3)^{\frac{1}{3}} \in R$
> - $\begin{aligned}
\left\{
\begin{aligned}
(a \circ b) \circ c &= \left( a^3 + b^3 \right)^{1/3} \circ c \\
                    &= \left( a^3 + b^3 + c^3 \right)^{1/3} \\
a \circ (b \circ c) &= a \circ \left( b^3 + c^3 \right)^{1/3} \\
                    &= \left( a^3 + b^3 + c^3 \right)^{1/3}
\end{aligned}
\right.
\\
\Rightarrow (a \circ b) \circ c = a \circ (b \circ c)
\end{aligned}$
> - $\begin{aligned}
\left\{
\begin{aligned}
0 \circ a &= \left( 0^3 + a^3 \right)^{1/3} = a \\
a \circ 0 &= \left( a^3 + 0^3 \right)^{1/3} = a
\end{aligned}
\right.
\\
\Rightarrow 0 \circ a = a \circ 0 = a
\end{aligned}$
> - $\left\{
\begin{aligned}
a \circ (-a) &= \left( a^3 + (-a)^3 \right)^{1/3} = 0 \\
(-a) \circ a &= \left( (-a)^3 + a^3 \right)^{1/3} = 0
\end{aligned}
\right.
\\
\Rightarrow a \circ (-a) = (-a) \circ a = 0$
> - $\left\{
\begin{aligned}
a \circ b &= \left( a^3 + b^3 \right)^{1/3} \\
b \circ a &= \left( b^3 + a^3 \right)^{1/3} = \left( a^3 + b^3 \right)^{1/3}
\end{aligned}
\right.
\\
\Rightarrow a \circ b = b \circ a$
- Bậc của nhóm: Số lượng phần tử của tập hữu hạn $G$ được gọi là bậc của $G$, ký hiệu là #$G$.

#### 2. Nhóm con:
- Nếu $H$ là một nhóm con của $(G, \circ)$ thì $(H, \circ)$ cũng là một nhóm và đồng thời thoả mãn các định lý về nhóm. Ký hiệu $H \subseteq G$
    - Đối với phép cộng: $Z \subseteq Q \subseteq R \subseteq C$
    - Tập {$e$} là nhóm con của mọi nhóm.
    - Giao của các nhóm con của nhóm $G$ là một nhóm con.
    - Gọi $a$ là một phần tử của một nhóm $(G, \circ)$, khi đó tập hợp {$a^n: n$ nguyên} là một nhóm con của $G$.
> Tất cả các nhóm con của $Z^+_6$ là {$0$}, {$0, 3$}, {$0, 2, 4$} và chính nó.
- **Nhóm con tách biệt và nhóm con không tầm thường:** Một nhóm con tách biệt của một nhóm $G$ là một nhóm con khác $G$ $(H \ne G)$, ký hiệu $H \subset G$. Một nhóm con không tầm thường của $G$ là bất kỳ nhóm con tách biệt nào của $G$ chứa một phần tử khác $e$.
- **Liên tập (liền kề):** Cho $G$ là nhóm Abel, $H \subseteq G$ và $a \in G$:
    - Tập hợp $\{a \circ h \mid h \in H\}$ gọi là liên tập trái (*left coset*) của nhóm con $H$ trong $G$, ký hiệu là $a \circ H$.
    - Tập hợp $\{h \circ a \mid h \in H\}$ gọi là liên tập phải (*right coset*) của nhóm con $H$ trong $G$, ký hiệu là $H \circ a$.
    - Định lý:
        - Nếu $H$ là một nhóm con của $G$ thì mọi liên tập trái (phải) của $H$ trong $G$ chứa cùng số phần tử.
        - Nếu $H$ là một nhóm con của $G$ thì số liên tập trái phân biệt của $H$ bằng với số liên tập phải phân biệt của $H$.
        - Định lý Lagrange: Nếu $H$ là một nhóm con của một nhóm hữu hạn $G$ thì #$H$|#$G$, hay là số phần tử của $H$ là ước số của $G$.

#### 3. Bậc phần tử của nhóm:
- Cho $G$ là một nhóm và $a \in G$. Bậc của phần tử $a$ là số dương nhỏ nhất $i \in Z$ thoả mãn điều kiện $a^i = e$. Số này kí hiệu là $ord(a)$. Nếu $i$ không tồn tại thì phần tử có bậc vô hạn.
- Biểu thức $a^i$ được sử dụng để chỉ toán tử lặp $(a \circ a \circ ... \circ a)$ $i$ lần ($i$ nguyên, dương). Khi $\circ = +$ thì $a \circ a \circ ... \circ a = a.i$, còn khi $\circ = .$ thì $a \circ a \circ ... \circ a = a^i$
    - Cho $G$ là nhóm hữu hạn và $a \in G$ là phần tử bất kỳ của nhóm, khi đó $ord(a)$|#$G$.
> Bậc của các phần tử nhóm $Z^+_6$ (nhóm với toán tử cộng, $e = 0$): ![image](https://hackmd.io/_uploads/SyKvbj2blg.png)

#### 4. Nhóm thừa số:
- Cho một nhóm $G$ và nhóm con tách biệt $H$ của $G$. Nếu định nghĩa tích của hai liên tập $g.H$ và $g'.H$ là liên tập $g.g'.H$ thì tập hợp các liên tập của $G$ theo $H$ với phép nhân đó làm thành một nhóm, gọi là nhóm thừa số của $G$ theo $H$.
- Ký hiệu: $G/H$
> Cho $n > 0$ là một số nguyên, tập $nZ =$ {$0, $\pm n$, $\pm 2n$, ...} là nhóm con của Z tương ứng với phép cộng. Thì có nhóm thừa số: $$Z / nZ = \{x + nZ \mid x \in Z \} = \{0 + nZ, 1 + nZ, 2 + nZ, \dots, n - 1 + nZ \}$$ Bởi vì: $n + nZ = 0 + nZ$, $n + 1 + nZ = 1 + nZ$,...

#### 5. Nhóm vòng:
Theo cách nói không hình thức, nhóm có biểu diễn vòng là nhóm vòng (*cyclic group*), thường có nhiều đặc tính hấp dẫn và được ứng dụng rộng rãi trong mật mã học.
- **Nhóm cyclic, phần tử sinh của nhóm:** Nhóm $G$ gọi là nhóm cyclic nếu tồn tại phần tử $a \in G$ sao cho $\forall b \in G$ tồn tại số nguyên $i \geqslant 0$ thoả mãn điều kiện $b = a^i$. Phần tử $a$ gọi là phần tử sinh của nhóm $G$. Nếu nhóm $G$ sinh ra bởi $a$ thì ký hiệu $G =$ <$a$> 
    - Nhóm nhân $Z^*_p$ với $p$ là số nguyên tố là một nhóm cyclic.
    - Nhóm $Z^*_{p^n}$ là nhóm cyclic với $\forall n \ge 1$.
> $Z^+_6 = \{0, 1, 2, 3, 4, 5\} = <1> = <5>$, bởi vì mọi phần tử của $Z_6$ đều có thể biểu diễn dưới dạng tổng của $1$ hoặc $5$ hoặc modulo $6$.
- **Hàm Euler:** Đối với $n \in Z$ và $n \ge 1$, hàm Euler $\phi (n)$ bằng số lượng các số nguyên $k$ sao cho $0 \le k < n$ và $GCD(k, n) = 1$.
    - Nhóm con bất kỳ của nhóm vòng cũng là nhóm vòng.
    - Với mỗi ước số dương $k$ của số #<$a$> của nhóm <$a$> bao gồm chỉ một phân nhóm mà nó có bậc $d$.
    - Nếu #<$a$> $= m$ thì #<$a^k$> $= ord(a^k) = m/GCD(k, m)$
    - Với mỗi ước số dương $d$ của số #<$a$> của nhóm <$a$> bao gồm $\phi (n)$ phần tử mà nó có bậc $d$.
    - Giả sử #<$a$> $= m$, khi đó nhóm <$a$> bao gồm $\phi (m)$ phần tử sinh. Chúng là các phần tử $a^r$ sao cho $GCD(r, m) = 1$.

#### Nhóm nhân $Z^*_n$:
- Là nhóm nhân với toán tử nhân của nhóm, bao gồm các phần tử là số nguyên dương nhỏ hơn $n$ và nguyên tố cùng nhau với $n$. Nhóm này bao gồm $\phi (n) = (p - 1)(q - 1)$ phần tử.
- Có nhiều ứng dụng trong lý thuyết số và mật mã học, đặc biệt là việc tìm bậc của nhóm giúp kiểm tra tính nguyên tố của $n$. Nếu như bậc của nhóm là $n - 1$ thì $n$ là số nguyên tố.
> - Cho nhóm $Z^*_5$, các phần tử của nhóm là $1$, $2$, $3$, $4$. Bậc của nhóm là $4$ nên $5$ là số nguyên tố.
> - Cho nhóm $Z^*_{10}$, các phần tử của nhóm là $1$, $3$, $7$, $9$. Bậc của nhóm là $4$ nên $10$ không phải là số nguyên tố.

#### Đồng hình (*homomorphism*), đẳng cấu (*isomorphism*):
- Giả sử $G_1$ và $G_2$ là các nhóm, . và * là các toán tử trong các nhóm này. Ánh xạ $f: G_1 \rightarrow G_2$ được gọi là đồng hình $G_1$ trong $G_2$ nếu $\forall a, b \in G_1$ thoả mãn biểu thức sau: $f(a.b) = f(a)*f(b)$. Thông thường toán tử trong các nhóm này không khác nhau, vì vậy điều kiện đồng hình có thể viết dưới dạng $f(ab) = f(a)f(b)$. Tương tự có thể định nghĩa nhóm đồng hình con.
- Nếu nhóm đồng hình $f: G_1 \rightarrow G_2$ là song ánh, với nó $f^{-1}(G_2) = G_1$ thì nó được gọi là đẳng cấu của nhóm $G_1$ trong $G_2$.
> - Phép đồng hình của nhóm nhân $R^*_+$ (gồm các số thực dương) trong nhóm cộng $R^+$ (gồm các số thực) được thực hiện dưới dạng sau: $$log: R^*_+ \rightarrow R^+$$ $$log(xy) = log(x) + log(y), x, y \in R^+$$
> - Trong nhóm cyclic $Z^*_9 = \{1, 2, 4, 5, 7, 8\}$. Nhận thấy $Z^*_9 = \{2^0, 2^1, 2^2, 2^3, 2^4, 2^5\}$ nên cho phép thiết lập một đẳng cấu $Z^+_6 \mapsto Z^*_9$ theo quy tắc $i \mapsto 2^i$.

### II. VÀNH:
#### 1. Vành:
- **Vành:** Vành $(R, +, *)$ chứa tập hợp $R$ với hai phép toán hai ngôi (được ký hiệu là $+$ (cộng) và * (nhân)) thoả mãn các định lý:
    - $(R, +)$ là một nhóm giao hoá đối với phép cộng. Phần tử đơn vị đối với phép cộng (không) được ký hiệu $0$.
    - Phép * có tính phân phối đối với phép $+$, nghĩa là $\forall a, b, c \in R$: $$a*(b + c) = a*b + a*c$$ $$(b + c)*a = b*a + c*a$$
    - Phép * có tính kết hợp: $$\forall a, b, c \in R: (a*b)*c = a*(b*c)$$
    - Đối với phép * vành $R$ phù hợp theo các định lý đóng kín, kết hợp. Phần tử đơn vị đối với phép nhân (đơn vị) được kí hiệu $1$ với $1 \ne 0$.
> - Tập hợp các số nguyên $Z$ với phép cộng và nhân thông thường là một vành.
> - Tập các ma trận vuông cùng cấp với $n$ với phép cộng và nhân ma trận là một vành.
> - Tập các số với phép cộng và nhân theo modulo $n$ ký hiệu là $Z_n$ là một vành, thường gọi là vành theo thặng dư $n$.
> - Đối với bất kỳ $n > 0$, nhóm $Z^*_n$ là vành đối với các toán tử cộng và nhân theo modulo $n$, và $0 = 0$, $1 = 1$.
- **Vành giao hoán:** Là vành $R$ trong đó phép nhân có tính chất giao hoán.
- **Vành đơn vị:** Là vành trong đó phép nhân có phần tử đơn vị.
- **Vành con:** Tập con $A$ của vành $R$ được gọi là vành con của $R$ nếu chính $A$ là một vành với hai phép toán cộng và nhân trên $R$ (bao gồm cả tính đóng của hai phép toán này trên $A$).
    - Giao của các vành con của $R$ là vành con của $R$.
    - Tập gồm một phần tử {$0$} và chính $R$ là vành con của $R$.
- **Vành thừa số:**
    - Cho $A$ là một ideal của vành $R$ và phần tử $x \in R$. Tập con của $R$ gồm các phần tử dạng $x + a$ với $\forall a \in A$ được gọi là một liên tập của $A$ theo $x$.
    - Ký hiệu $R/A$ là tập hợp tất cả các liên tập của $A$ với $\forall x \in R: R/A = \{x + A \mid x \in R\}$ được gọi là tập thừa số của $R$ theo $A$.
    - Trên tập thừa số $R/A$ có thể xác định hai phép toán cộng và nhân:
        - $(x + A) + (y + A) = (x + y) + A$
        - $(x + A)*(y + A) = (x*y) + A$
    Khi đó có thể chứng mình $R/A$ là một vành và vành này được gọi là vành thừa số của $R$ theo $A$.

#### 2. Một số khái niệm về ideal:
Ideal là một tập con đặc biệt của một vành:
- Vành con $A$ của vành $R$ được gọi là ideal trái (hoặc phải) của $R$ nếu $x*a \in A$ (hoặc $a*x \in A$) với $\forall a \in A$, với $\forall x \in R$.
- Vành con $A$ vừa là ideal trái, vừa là ideal phải của $R$ được gọi là ideal của $R$.
- Giao của họ bất kỳ các ideal của $R$ là ideal của $R$.
- Cho tập con $X \subseteq R$, ideal nhỏ nhất của $R$ chứa $X$ được gọi là $ideal$ sinh bởi $X$.

#### 3. Đồng hình vành:
- **Đồng hình, đẳng cấu:** Cho $R_1$ và $R_2$ là hai vành, ánh xạ $f: R_1 \rightarrow R_2$ được gọi là đồng hình vành nếu $f$ bảo toàn hai phép toán cộng và nhân trong $R$, nghĩa là với $\forall a, b \in R$: $$f(a + b) = f(a) + f(b)$$ $$f(a*b) = f(a)*f(b)$$
- Nếu đồng hình vành $f: R_1 \rightarrow R_2$ là song ánh thì nó được gọi là đẳng cấu của vành $R_1$ trong $R_2$.

### III. TRƯỜNG:
#### 1. Trường:
- **Trường:** Nếu các phần tử khác không của vành tạo thành nhóm tương ứng với phép nhân thì gọi vành đó là trường.
> - Tập $Q$, $R$ và $C$ là trường tương ứng với phép cộng và nhân thông thường, với phần tử $0$ là $0$ và $1$ là $1$.
> - Tập $Z_p$ với $p$ là số nguyên tố là trường tương ứng với phép cộng và nhân cho modulo $p$, với phần tử $0$ là $0$ và $1$ là $1$.
- **Trường con:** Giả sử $F$ là một trường, tập con $E \subseteq F$ được gọi là trường con của $F$ nếu chính $E$ là một trường với cùng một phép toán trong $F$.
> - Tập $A \subseteq R: A = \{a + b \sqrt{2} \mid a, b \in Q\}$ là trường con của $R$.
> - Trường số hữu tỷ $Q$ là trường con của trường số thực $R$, trường số thực là trường con của trường số phức $C$.
- **Cấu trúc đại số:**
    - Thực tế, đôi khi không cần phân chia thành nhóm, vành và trường. Trong tình huống như thế chúng được gọi là các cấu trúc đại số (*algebraic structure*).
    - Cấu trúc đại số hữu hạn nếu nó bao gồm số lượng hữu hạn các phần tử. Số lượng các phần tử là bậc của nó.
    - Tập con không rỗng $S$ mà nó có cấu trúc đại số tương ứng đối với các toán tử đã được định nghĩa trong cấu trúc $A$ được gọi là cấu trúc con của cấu trúc đại số $A$. Nếu $S \neq A$ thì cấu trúc con $S$ được gọi là cấu trúc con riêng biệt của cấu trúc $A$.
    - Giả sử $A$ là cấu trúc đại số, $B \subseteq A$ là cấu trúc con của $A$. Tập hợp tất cả các liên tập $a \circ B$ (phần tử $a$ lấy theo tất cả cấu trúc $A$) đối với toán tử * được xác định theo quy tắc $(a \circ B)*(b \circ B) = (a \circ b) \circ B$, với các phần tử $0 \circ B$ và $1 \circ B$ được gọi là cấu trúc thừa số của $A$ theo modulo $B$, ký hiệu $A/B$.
    - Có thể thấy rằng vành (trường tương ứng) có thể chứa không chỉ vành con (trường con tương ứng) mà cả nhóm con (vành con và nhóm con tương ứng).

#### 2. Trường hữu hạn:
Được ứng dụng rất nhiều trong mật mã.

##### a) Các trường hữu hạn, bao gồm các phần tử nguyên tố:
Các trường hữu hạn mà bậc của chúng (số lượng các phần tử) là số nguyên tố có cấu trúc đơn giản nhất. Các trường như vậy được ứng dụng rộng rãi trong mật mã.
- **Trường nguyên tố:** Trường không bao gồm các trường con riêng biệt. Trường nguyên tố hữu hạn bao gồm số nguyên tố của các phần tử (bậc của nó là số nguyên tố).
> Trường Q là trường nguyên tố, còn tập hợp $R$ thì không vì tập $Q$ là trường con riêng biệt của trường $R$.
- **Trường hữu hạn:** Trường $F$ có $q$ là số phần tử hữu hạn, ký hiệu $F_q$.
    - Nhóm nhân $Z^*_n$ của các phần tử không âm của một trường hữu hạn $F_q$ là nhóm cyclic. Phần tử sinh $\alpha$ của nhóm cyclic gọi là phần tử nguyên thuỷ của trường hữu hạn $F_q$. Tất cả các phần tử của trường hữu hạn có thể viết dưới dạng: $$F_q = \{0, \alpha, (\alpha)^2,..., (\alpha)^{q - 2}, (\alpha)^{q - 1} = (\alpha)^0 = 1\}$$
    - Nếu $p$ là số nguyên tố thì vành $Z_p$ là một trường, nhưng trên thực tế có nhiều trường hữu hạn khác không có dạng trên.
        - Trường Galois: Trường hữu hạn $q$ phần tử với $q = p^n$ ($p$ là số nguyên tố, $n \geq 1$ là số nguyên). 
- **Đồng hình và đẳng cấu:**
    - Giả sử $A$ và $B$ là các cấu trúc đại số. Ánh xạ $f: A \rightarrow B$ được gọi là đồng hình từ cấu trúc $A$ vào cấu trúc $B$ nếu ánh xạ $f$ bao gồm các toán tử đã được định nghĩa trong cấu trúc $A$. Nói cách khác, nếu toán tử $\circ$ được định nghĩa trong cấu trúc $A$ và toán tử * được định nghĩa trong cấu trúc $B$ thì: $$\forall x, y \in A: f(x \circ y) = f(x)*f(y)$$
    - Phép biết đổi đồng hình là song ánh $f$ từ cấu trúc $A$ lên cấu trúc $B$ được gọi là đẳng cấu.
    - Nếu $: A \rightarrow B$ là đồng hình, còn $e$ là phần tử đơn vị của cấu trúc $A$ (tương ứng với phép cộng và nhân): $$f(e)*f(e) = f(e \circ e) = f(e)$$ Phần tử $f(e)$ là phần tử đơn vị của cấu trúc $B$: $$\forall a \in A: $$f(a)*f(a^{-1}) = f(a \circ a^{-1}) = f(e)$$ Vì $f(a^{-1}) = f(a)^{-1} \forall a \in A$. Nếu tất cả cấu trúc $A$ và $B$ là đẳng cấu, chúng bao gồm cùng số lượng phần tử. Như vậy hai cấu trúc đẳng cấu được xây dựng như nhau.
- **Đặc số của cấu trúc đại số:** Đặc số của cấu trúc đại số $A$, ký hiệu $char(A)$, là số nguyên dương nhỏ nhất $n$ sao cho $na = 0$ đối với phần tử bất kỳ $a \in A$. Nếu $n$ không tồn tại thì đặc số của cấu trúc $A$ bằng không.
    - Đặc số của trường hữu hạn bất kỳ là số nguyên tố (nếu đặc số không bằng $0$).
    - Bậc của trường hữu hạn nguyên tố bằng đặc số của trường đó. Tuy nhiên trường nguyên tố không thể xem là điển hình, dạng chung hơn của trường hữu hạn có thể được xây dựng dựa trên các đa thức.
- **Đa thức trong trường hữu hạn:**
    - Cho $F_q$ là trường hữu hạn, đa thức xây dựng trên trường $F_q$ được cho dưới dạng: $$f(x) = \displaystyle \sum_{i = 0}^{n} a_ix^i$$ Với:
        - $n$ là số nguyên không âm, $a_i$ là hệ số, $0 \leq i \leq n$, $a_i \in F_q$, còn $x$ là ký hiệu không thuộc trường $F_q$. 
        - Hệ số $a_n$ có bậc cao nhất và không bằng $0$ khi $n > 0$.
            - Nếu hệ số cao nhất bằng $a_0$ thì đa thức $f$ là hằng số.
            - Nếu hệ sô cao nhất $a_0 = 0$ thì $f$ là đa thức không ($f = 0$).
        - Số nguyên $n$ gọi là bậc của đa thức $f(x)$, ký hiệu: $n = deg(f(x)) = deg(f)$. 
        - Tập tất cả các đa thức trên trường $F_q$ ký hiệu là $F_q[x]$. Việc cộng, trừ, nhân, chia đa thức thuộc $F_q[x]$ giống như cộng, trừ, nhân, chia đa thức bình thường, chỉ khác là tính các hệ số trong trường đã cho.
- **Đa thức bất khả quy:** Đa thức $f(x) \in F_q[x]$ được gọi là bất khả quy nếu không tồn tại các đa thức $f_1(x), f_2(x) \in F_q[x]$ sao cho: $$f(x) = f_!(x)*f_2(x)$$ Trong đó $deg(f_1(x)) > 0$ và $deg(f_2(x)) > 0$
    - Cho $F$ là một trường còn $f(x)$ là đa thức khác không từ tập $F_q[x]$, khi đó tập $F_q[x]/f(x)$ là một trường khi và chỉ khi $f(x)$ là đa thức bất khả quy trên trường $F$.
    - $f(x)$ thường được gọi là đa thức xác định của trường $F_q[x]/f(x)$.
> Xây dựng $F_8 = F_2[x]/(x^3 + x^2 + 1)$ là đa thức bất khả quy trong trường $F_2$. Các phần tử của trường là $0$, $x$, $x + 1$, $x^2$, $x^2 + 1$, $x^2 + x$, $x^2 + x + 1$. Bảng tính toán các phần tử trong trường trên bởi phép nhân:
> ![image](https://hackmd.io/_uploads/SJf5f6TZgx.png)
> Một số $n$ bit có thể biểu diễn dưới dạng đa thức bậc $n - 1$. Ví dụ $a = a_1a_2...a_n$ với $a_1, a_2,..., a_n \in F_2$ thì đa thức biểu diễn có dạng $f(x) = a_1x^{n - 1} + a_2x^{n - 2} + ... + a_{n - 1}x + a_n$.
    - Cho $F$ là một trường gồm $p$ phần tử, $f$ là đa thức bất khả quy bậc $n$ trên trường $F$. Lúc này số lượng phần tử của trường $F[x]/f(x)$ bằng $p^n$.
        - Hệ quả: Đối với một số nguyên tố $p$ và mỗi số dương $n$ tồn tại trường hữu hạn bao gồm $p^n$ phần tử.
    - Trường hữu hạn xây dựng trên đa thức cơ sở:
        - Cho $F$ là một trường hữu hạn, còn $f \in F[x]$ là đa thức bất khả quy bậc $n$ trên trường $F$. Nếu $\theta$ là một nghiệm bất kỳ của $f(x) = 0$ thì các phần tử $1$, $\theta$, $(\theta)^2$,..., $(\theta)^{n - 1}$ là độc lập tuyến tính trên trường $F$, nghĩa là đối với $r_i \in F$ (với $i = 0, 1, 2,..., n - 1$): $$r_0 + r_1\theta + r_2(\theta)^2 + ... + r_{n - 1}(\theta)^{n - 1} = 0 \rightarrow r_0 = r_1 = ... = r_{n - 1} = 0$$
- **Đa thức cơ sở:**
    - Cho $F$ là trường hữu hạn, $f(x)$ là đa thức bất khả quy bậc $n$ trên trương $F$. Khi đó $\alpha$ là nghiệm của phương trình $f(x) = 0$ và các thành phần $1$, $\alpha$, $(alpha)^2$,..., $(\alpha)^{n - 1}$ gọi là đa thức cơ sở của không gian hữu hạn trên trường $F$.
    - Trong đại số tuyến tính, một đa thức cơ sở gồm $n$ phần tử sẽ sinh ra không gian vector $n$ chiều. Để biểu diễn ý này, sử dụng tích vô hướng các độ lớn thuộc về trường $F$, tức là không gian vector $n$ chiều, sinh bởi đa thức cơ sở $1$, $\alpha$, $(alpha)^2$,..., $(\alpha)^{n - 1}$ có cấu trúc: $$\{\displaystyle \sum_{i = 0}^{n - 1} {r_i (\theta)^i} \mid r_0, r_1,..., r_{n - 1} \in F\}$$
        - Cho $F$ là trường hữu hạn, $f(x)$ là đa thức bất khả quy bậc $n$ trên trường $F$. Không gian vector đối với bất kỳ nghiệm $\theta$ nào của phương trình $f(x) = 0$ đều là trường hữu hạn và có số phần tử là (#$F$)$^n$.
- **Trường hữu hạn $F_{q^n}$:**: Cho $q$ là số lượng phần tử của trường hữu hạn $F$, ký hiệu $F_{q^n}$ cho trường hữu hạn được xây dựng trên cơ sở gồm $n$ phần tử trên trường $F$.
    - Cho $F$ là trường hữu hạn bao gồm $q$ phần tử, còn $F_{q^n}$ là trường hữu hạn xây dựng trên đa thức cơ sở trường $F$. Khi đó:
        - Trường $F$ là trường con của trường $F_{q^n}$.
        - Bất kỳ thành phần $a \in F_{q^n}$ thoả mãn điều kiện $a^q = a$ khi và chỉ khi $a \in F$.
> Trường $F_{2^8}$:
> - Có thể thấy $F_2[x]/(x^8 + x^4 + x^3 + x + 1)$ là một trường bao gồm $2^8$ phần tử và có thể biểu diễn dưới dạng không gian vector: $$\{b_7a^7 + b_6a^6 + b_5a^5 + b_4a^4 + b_3a^3 + b_2a^2 + b_1a^1 + b_0\}$$ Với: $a$ là nghiệm của phương trình $x^8 + x^4 + x^3 + x + 1 = 0$ và các hệ số $b_7, b_6, b_5, b_4, b_3, b_2, b_1 \in F_2$.
> - Việc tính toán trong trường $F_{2^8}$ đơn giản hơn trong trường $F_2[x]/(x^8 + x^4 + x^3 + x + 1)$ nhờ trực tiếp nhân hai thành phần và biểu diễn dưới dạng tổ hợp tuyến tính các thành phần $1$, $a$, $a^2$,..., $a^{n - 1}$.
> - Ví dụ cần tính '$57$'.'$83$' (với '$57$' $= 0101_-0111$, '$83$' $= 1000_-0011$): $$(a^6 + a^4 + a^2 + a + 1)(a^7 + a + 1) = a^13 + a^11 + a^9 + a^8 + a^6 + a^5 + a^4 + a^3 + 1$$ Bởi vì $a^8 + a^4 + a + 1 = 0$ nên nhận được các tổ hợp tuyến tính sau: $$a^8 = a^4 + a^3 + a + 1$$ $$a^9 = a^5 + a^4 + a^2 + a$$ $$a^11 = a^7 + a^6 + a^4 + a^3$$ $$a^13 = a^9 + a^8 + a^6 + a^5$$ Cho nên $a^13 + a^11 + a^9 + a^8 + a^6 + a^5 + a^4 + a^3 + 1 = a^7 + a^6 + 1$. Và có nghĩa là '$57$'.'$83$' $=$ '$C1$'. Nếu như dùng $F_2[x]/(x^8 + x^4 + x^3 + x + 1)$ thì cần phải thực hiện phép chia và phức tạp hơn.

### IV. XÂY DỰNG NHÓM TRÊN CƠ SỞ ĐƯỜNG CONG ELLIPTIC:
Các nhóm xây dựng trên cơ sở đường Elliptic (*Elliptic curves*) đóng vai trò rất quan trọng trong mật mã hiện đại. Ứng dụng của các nhóm này trong mật mã khoá công khai được để xuất đầu tiên bởi hai nhà khoa học Neal Koblitz và Victor S. Miller.

#### 1. Công thức Weierstrasse và đường cong Elliptic:
Gọi $K$ là một trường hữu hạn hoặc vô hạn, một đường cong Elliptic được định nghĩa trên trường $K$ bằng công thức Weiersstrass: $$y^2 + axy + by = x^3 + cx^2 + dx + e$$ Với $a$, $b$, $c$, $d$, $e$ là các số thoả mãn một vài điều kiện đơn giản nào đó và thuộc $K$.
> ![image](https://hackmd.io/_uploads/BkafUnC-ge.png)

#### 2. Đường cong Elliptic trên trường hữu hạn:
- Đường cong Elliptic được xây dựng trên các trường hữu hạn. Có hai trường hữu hạn thường được sử dụng là trường hữu hạn $F_q$ với:
    - $q$ là số nguyên tố $p$.
    - $q$ là $2^m$ ($m$ là số nguyên).
- Tuỳ thuộc vào trường hữu hạn $F_q$, với mỗi bậc của $q$, tồn tại nhiều đường cong Elliptic. Do đó với một trường hữu hạn cố định có $q$ phần tử và $q$ lớn, có nhiều sự lựa chọn nhóm đường cong Elliptic.

#### 3. Đường cong Elliptic trên trường $F_p$ ($p$ là số nguyên tố):
- **Đường cong Elliptic trên trường $F_p$:**
    - Cho $p$ là số nguyên tố ($p > 3$), cho $a, b \in F_p$ sao cho $4a^3 + 27b^2 \neq 0$ trong trường $F_p$. Một đường cong Elliptic $E(F_p)$ trên $F_p$ (được định nghĩa bởi các tham số $a$ và $b$) là một tập hợp các cặp giá trị $(x, y)$ (với $x, y \in F_p$) thoả mãn công thức: $$y^2 = x^3 + ax + b$ cùng với điểm $O$ đặc biệt gọi là điểm vô cực và có thể biểu diễn dưới dạng $O = (x, \infty)$.
    - Số lượng điểm của $E(F_p)$ là #$E(F_p)$ thoả mã định lý Hasse.
    - Độ phức tạp của thuật toán xây dựng dựa trên nhóm đường cong Elliptic phụ thuộc vào số điểm trên đường cong đó và xét định lý Hasse về số điểm trên đường cong Elliptic.
        - Định lý Hasse: Số các điểm trên đường cong Elliptic thoả mãn bất đẳng thức: $$p + 1 - 2\sqrt{p} \leq \#E(F_p) \leq p + 1 + 2\sqrt{p}$$
- **Bậc của điểm:**
    - Cho điểm $P(x, y)$ thuộc đường cong Elliptic, bậc $n$ của điểm $P(x, y)$ là số nguyên dương thoả mã biểu thức: $$nP = O$$
    - Tập hợp các điểm trên $E(F_p)$ tạo thành một nhóm thoả mãn các tính chất:
        - Tính đóng: $\forall a, b \in G, a + b \in G$
        - Tính kết hợp: Các phép toán trong nhóm có tính kết hợp: $(a + b) + c = a + (b + c)$
        - Phần tử đơn vị: Có một giá trị $0 \in G$ sao cho $a + 0 = 0 + a = a, \forall a \in G$.
        - Phần tử nghịch đảo: $f\forall a \in G, \exists -a \in G$ gọi là số nghịch đảo của $a$, sao cho $-a + a = a + (-a) = 0$.
> - Cho phương trình $y^2 = (x^3 + 6x + 4) \mod 7$. Nhận thấy $(4.6^3 + 27.4^2) \mod 7 = 1 \neq 0$ phù hợp với điều kiện của các nhóm Elliptic theo modulo $7$. Đường cong này có các điểm sau: $$E(F_7) = \{O, (0, 2), (1, 2), (1, 5). (3, 0), (4, 1), (4, 6), (6, 2), (6, 5)\}$$ Ta thấy $(3, 0) + (3, 0) = O$$. Phần tử sinh của nhóm $G = (1, 2)$. Nhóm các điểm trên đường cong Elliptic là nhóm Cyclic.
> - Giả sử $p = 23$, khảo sát đường cong Elliptic $y^2 = ^3 + x + 1$. Trong trường hợp này $a = b = 1$ và có $(4.1^3 + 27.1) \mod 23 = 8 \neq 0$ phù hợp với điều kiện của các nhóm Elliptic theo modulo $3$.
> ![image](https://hackmd.io/_uploads/B1aJ32CZgg.png)
> Trên hình $2$ biểu diễn đường cong liên tục, các điểm sẽ cho giá trị phù hợp với phương trình đã cho. Đối với các nhóm điểm trên Elliptic chỉ khảo sát các giá trị nguyên từ $(0, 0)$ đến $(p , p)$ trong góc phần tư của các số không âm (góc thứ nhất), phù hợp với phương trình theo modulo $p$.
> Bảng trình bày các điểm (khác $O$) là các phần tử của trường $E_{23}(1, 1)$:
> ![image](https://hackmd.io/_uploads/BJ7c3hCWxl.png)
- **Đường cong Elliptic trên trường $F_{2^m}$:**
    - Một đường cong Elliptic $E(F_{2^m})$ trên $F_{2^m}$ được định nghĩa bởi các tham số $a, b \in F_{2^m}$ (với $b \neq 0$) là tập các điểm $P(x, y)$ với $x \in F_{2^m}, y \in F_{2^m}$ thoả mãn công thức: $$y^2 + xy = x^3 + ax^2 + b$$ cùng với điểm $O$ là điểm tại vô cực. Số lượng các điểm thuộc $E(F_{2^m})$ (ký hiệu #$E(F_{2^m})$) thoả mãn định lý Hasse: $$q + 1 - 2\sqrt{q} \leq \#E(F_q) \leq q + 1 + 2\sqrt{q}$$ Trong đó $p = 2^m$ và $\#E(F_{2^m})$ là số chẵn.
    - Tập hợp các điểm trên $E(F_{2^m})$ tạo thành một nhóm thoả mãn các tính chất:
        - $O + O = O$
        - $(x, y) + O = (x, y), \forall (x, y) \in E(F_{2^m})$
        - $(x, y) + (x, -y) = O, \forall(x, y) \in E(F_{2^m})$. Khi đó $(x, -y)$ là điểm đối của $(x, y)$ trên $E(F_{2^m})$.
