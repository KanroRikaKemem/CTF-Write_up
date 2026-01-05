### I. ENCODING:
#### 1. ASCII:
##### ##### a) Mô tả đề bài:
- ASCII là tiêu chuẩn mã hóa 7 bit cho phép biểu diễn văn bản bằng các số nguyên 0 - 127.
- Sử dụng mảng số nguyên bên dưới, chuyển đổi các số thành ký tự ASCII tương ứng để thu được cờ.
**[99, 114, 121, 112, 116, 111, 123, 65, 83, 67, 73, 73, 95, 112, 114, 49, 110, 116, 52, 98, 108, 51, 125]**
- Trong Python, hàm `chr()` có thể được sử dụng để chuyển đổi số thứ tự ASCII thành ký tự (hàm `ord()` thực hiện ngược lại).
     
##### ##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/H1pBYqxfyx.png)
- Khai báo biến **a** gồm các số thứ tự ASCII đã cho.
- `join()` là một phương thức của kiểu dữ liệu chuỗi (`str`) trong Python, có chức năng nối các phần tử trong một iterable. `' '.join` có tác dụng nối các phần tử mà không sử dụng dấu cách (Nếu sử dụng `,` thì các phần tử được ngăn cách bằng dấu phẩy).
- `for value in a`: Duyệt qua từng phần tử `value` trong danh sách `a`.
- `chr(value)`: Với mỗi phần tử `value`, hàm `chr(value)` được gọi để chuyển giá trị số nguyên `value` thành ký tự Unicode tương ứng.
- `print(result)`: In ra kết quả sau khi nối phần tử.

##### ##### c) Kết quả:
`crypto{ASCII_pr1nt4bl3}`

#### 3. HEX:
##### ##### a) Mô tả đề bài:
- Khi mã hóa thứ gì đó, văn bản mã hóa thu được thường có các byte không thể in được ký tự ASCII. Nếu muốn chia sẻ dữ liệu được mã hóa của mình, thông thường phải mã hóa dữ liệu đó thành thứ gì đó thân thiện hơn với người dùng và di động hơn trên các hệ thống khác nhau.
- Hệ thập lục phân có thể được sử dụng theo cách như vậy để biểu diễn các chuỗi ASCII. Đầu tiên, mỗi chữ cái được chuyển đổi thành số thứ tự theo bảng ASCII. Sau đó, các số thập phân được chuyển đổi thành cơ số 16, còn được gọi là hệ thập lục phân. Các số có thể được kết hợp với nhau thành một chuỗi hex dài.
- Bên dưới là một lá cờ được mã hóa dưới dạng chuỗi hex. Giải mã lại thành byte để lấy cờ.
```
63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d
```
- Trong Python, hàm `bytes.fromhex()` có thể được sử dụng để chuyển đổi hex thành byte. Phương thức cá thể `.hex()` có thể được gọi trên chuỗi byte để lấy biểu diễn hex.
     
##### ##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/Sy-QlixMyx.png)
- Khai báo biến `string` là chuỗi đã cho.
- Khai báo biến `result` với giá trị là chuỗi hex đã chuyển đổi thành byte bởi cú pháp `bytes.fromhex(string)`.

##### ##### c) Kết quả:
`crypto{You_will_be_working_with_hex_strings_a_lot}`

#### 3. Base64:
##### ##### a) Mô tả đề bài:
- Một sơ đồ mã hóa phổ biến khác là Base64, cho phép biểu diễn dữ liệu nhị phân dưới dạng chuỗi ASCII bằng bảng chữ cái gồm 64 ký tự. Một ký tự của chuỗi Base64 mã hóa 6 chữ số nhị phân (bit) và do đó, 4 ký tự của Base64 mã hóa ba byte 8 bit.
- Base64 được sử dụng phổ biến nhất trên mạng, vì vậy dữ liệu nhị phân như hình ảnh có thể dễ dàng được đưa vào tệp HTML hoặc CSS.
- Lấy chuỗi hex bên dưới, giải mã thành byte và sau đó mã hóa thành Base64.
```
72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf
```
- Trong Python, sau khi nhập module base64 bằng `import base64`, có thể sử dụng hàm `base64.b64encode()`. Hãy nhớ giải mã hex trước khi mô tả thử thách nêu rõ.
     
##### ##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/B1414sxG1e.png)
- Khai báo biến `string` là chuỗi đã cho.
- Khai báo biến `byte` với giá trị là chuỗi hex đã chuyển đổi thành byte bởi cú pháp `bytes.fromhex(string)`.
- Khai báo thư viện base64 bằng cú pháp `import base64`.
- Khai báo biến `result` với giá trị là chuỗi byte đã chuyển đổi thành base64 bởi cú pháp `base64.b64encode(byte)`.
    
##### ##### c) Kết quả:
`crypto/Base+64+Encoding+is+Web+Safe/`

#### 4. Bytes and Big Intergers:
##### ##### a) Mô tả đề bài:
- Các hệ thống mật mã như RSA hoạt động trên các con số, nhưng các thông điệp được tạo thành từ các ký tự. Chúng ta nên chuyển thông điệp của mình thành số như thế nào để có thể áp dụng các phép toán?
- Cách phổ biến nhất là lấy byte thứ tự của tin nhắn, chuyển đổi chúng thành hệ thập lục phân và ghép nối. Điều này có thể được hiểu là cơ số 16/thập lục phân và cũng được biểu thị bằng cơ số 10/thập phân.
- Để minh họa:
```
Tin nhắn: XIN CHÀO
byte ascii: [72, 69, 76, 76, 79]
byte hex: [0x48, 0x45, 0x4c, 0x4c, 0x4f]
cơ sở-16: 0x48454c4c4f
cơ sở-10: 310400273487
```
- Thư viện `PyCryptodome` của Python thực hiện điều này bằng các phương thức `bytes_to_long()` và `long_to_bytes()`. Trước tiên, bạn sẽ phải cài đặt `PyCryptodome` và nhập nó bằng `from Crypto.Util.number import *`.
- Chuyển đổi số nguyên sau thành tin nhắn:
```
11515195063862318899931685488813747395775516287289682636499965282714637259206269
```

##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/BJ0abezfke.png)
- Khai báo thư viện `Crypto.Util.number` với các hàm `bytes_to_long()` và `long_to_bytes()` để chuyển đổi giữa dạng số nguyên lớn (big integer) với dạng chuỗi byte.
- Khai báo biến `bigint` với dữ liệu đã cho.
- Khai báo biến `string` với giá trị là `bigint` sau khi chuyển từ big integer sang hex. 
- `hex(bigint)` sẽ có giá trị dạng `0x...` nên sử dụng thêm `[2:]` để cắt bỏ hai ký tự đầu của chuỗi.
- Khai báo biến `result` với giá trị là `string` sau khi chuyển từ hex sang byte.

##### c) Kết quả:
`crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}`

#### 5. Encoding Challenge:
##### ##### a) Mô tả đề bài:
- Bây giờ bạn đã hiểu rõ về nhiều loại mã hóa khác nhau mà bạn sẽ gặp phải, hãy xem xét việc tự động hóa nó.
- Bạn có thể vượt qua tất cả 100 cấp độ để có được cờ không?
- Tệp `13377.py` đính kèm bên dưới là mã nguồn của những gì đang chạy trên máy chủ. Tệp `pwntools_example.py` cung cấp sự khởi đầu của một giải pháp.
- Kết nối tại `socket.cryptohack.org 13377`
    
##### b) Phân tích cách giải: 
![image](https://hackmd.io/_uploads/r1cf28fG1x.png)
- Khai báo các thư viện cần thiết.
- Các thiết lập như chú thích trong hình.
![image](https://hackmd.io/_uploads/rJsryPGM1e.png)
- Chạy vòng lặp giải mã 100 lần.
    
##### c) Kết quả:
`crypto{3nc0d3_d3c0d3_3nc0d3}`
    
### II. XOR:
#### 1. XOR Starter:
##### a) Mô tả đề bài:
- XOR là toán tử bitwise trả về 0 nếu các bit giống nhau và 1 nếu ngược lại. Trong sách giáo khoa, toán tử XOR được ký hiệu là ⊕, nhưng trong hầu hết các thử thách và ngôn ngữ lập trình, bạn sẽ thấy dấu mũ `^` được sử dụng thay thế.
![image](https://hackmd.io/_uploads/rkiBGc7z1l.png)
- Đối với các số nhị phân dài hơn, ta XOR từng bit: `0110 ^ 1010 = 1100`. Ta có thể XOR số nguyên bằng cách trước tiên chuyển đổi số nguyên từ thập phân sang nhị phân. Chúng ta có thể XOR chuỗi bằng cách trước tiên chuyển đổi từng ký tự thành số nguyên biểu thị ký tự Unicode.
- Với string `label`, XOR mỗi ký tự có số nguyên `13`. Chuyển đổi các số nguyên này trở lại thành chuỗi và gửi cờ dưới dạng `crypto{new_string}`.
- Thư viện `pwntools` của Python có hàm `xor()` tiện lợi có thể XOR dữ liệu thuộc nhiều loại và độ dài khác nhau. Nhưng trước tiên, bạn có thể muốn triển khai chức năng của riêng mình để giải quyết vấn đề này.
     
##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/SJqnT5QGJg.png)
- Khai báo thư viện `pwn`.
- Khai báo biến `dec_num` và `string` theo đề cho.
- Cách hoạt động của code:
    - Chuyển từng ký tự của `label` thành mã ASCII và XOR với `13`, thu được các số nguyên.
    - Chuyển các số nguyên vừa tìm được thành các ký tự tương ứng trong ASCII.
    - Nối các ký tự lại thành chuỗi bằng cú pháp `' '.join()`, gán chuỗi là giá trị của `new_string`.
    - Khai báo biến `flag` có giá trị là `crypto{new_string}` và in `flag`.

##### c) Kết quả:
`crypto{aloha}`

#### 2. XOR Properties:
##### a) Mô tả đề bài:
- Trong thử thách trước, bạn đã thấy cách XOR hoạt động ở cấp độ bit. Trong phần này, chúng ta sẽ đề cập đến các thuộc tính của thao tác XOR và sau đó sử dụng chúng để hoàn tác một chuỗi thao tác đã mã hóa cờ. Việc có được trực giác về cách thức hoạt động của tính năng này sẽ giúp ích rất nhiều khi bạn tấn công các hệ thống mật mã thực sự sau này, đặc biệt là trong danh mục mật mã khối.
- Có bốn thuộc tính chính chúng ta nên xem xét khi giải quyết các thách thức bằng toán tử XOR
    - **Giao hoán: A ⊕ B = B ⊕ A**
    - **Phân phối: A ⊕ (B ⊕ C) = (A ⊕ B) ⊕ C**
    - **Đồng nhất: A ⊕ 0 = A**
    - **Tự nghịch đảo: A ⊕ A = 0**
- Hãy phá vỡ điều này. Giao hoán có nghĩa là thứ tự của các phép toán XOR không quan trọng. Phân phối có nghĩa là một chuỗi các hoạt động có thể được thực hiện mà không cần có thứ tự (chúng ta không cần phải lo lắng về dấu ngoặc). Đồng nhất là 0, do đó XOR với 0 nghĩa là "không làm gì cả" và cuối cùng thứ gì đó XOR với chính nó sẽ trả về 0.
- Hãy áp dụng điều này vào thực tế! Dưới đây là một loạt kết quả đầu ra trong đó ba khóa ngẫu nhiên đã được XOR cùng nhau và có flag. Sử dụng các thuộc tính trên để hoàn tác mã hóa ở dòng cuối cùng để lấy flag.
```
KEY1 = a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313
KEY2 ^ KEY1 = 37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e
KEY2 ^ KEY3 = c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1
FLAG ^ KEY1 ^ KEY3 ^ KEY2 = 04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf
```
- Trước khi bạn XOR các đối tượng này, hãy đảm bảo giải mã từ hex sang byte.
     
##### b) Phân tích cách giải:
![image](https://hackmd.io/_uploads/SyEr2omfke.png)
- Khai báo thư viện.
- Chuyển các dữ liệu đề cho từ hex sang bytes.
- Thực hiện XOR, ta có:
```
(FLAG ^ KEY1 ^ KEY2 ^ KEY3) ^ (KEY2 ^ KEY3) = FLAG ^ KEY1
(FLAG ^ KEY1) ^ KEY1 = FLAG
```
- Thực hiện in FLAG ra màn hình.
    
##### c) Kết quả:
`crypto{x0r_i5_ass0c1at1v3}`
    
#### 3. Favourite byte:
##### a) Mô tả đề bài:
- Đối với một số thử thách tiếp theo, bạn sẽ sử dụng những gì vừa học để giải một số câu đố XOR khác.
- Tôi đã ẩn một số dữ liệu bằng XOR với một byte đơn, nhưng byte đó là bí mật. Đừng quên giải mã từ hex trước.
```
73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d
```

##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/Sylou1Szyx.png)
- Khai báo thư viện.
- Chuyển đổi chuỗi hex đề cho sang bytes và khai báo vào biến `by1`.
- Chạy vòng lặp từ 0 đến 256 (số mã trong ASCII) và XOR `by1 `với các số lần lượt từ 0 đến 256 và in các kết quả sau XOR, ta tìm được flag.

##### c) Kết quả:
`crypto{0x10_15_my_f4v0ur173_by7e}`
    
#### 4. You either know, XOR you don't:
##### a) Mô tả đề bài:
Tôi đã mã hóa cờ bằng khóa bí mật của mình, bạn sẽ không bao giờ đoán được.
> Hãy nhớ định dạng cờ và cách nó có thể giúp bạn trong thử thách này!

```
0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104
```
    
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/HkjeIeBfyl.png)
- Khai báo thư viện.
- Chuyển dữ liệu hex đề cho sang dạng byte và khai báo vào `he`.
- Khai báo `string` có giá trị là `crypto{` và in ra màn hình biến `string`.
- Ta được output: `b'myXORke+y_Q\x0bHOMe$~seG8bGURN\x04DFWg)a|\x1dTM!an\x7f'`
- Thấy rằng phần `myXORke+y` có ý nghĩa, khai báo biến `string` với giá trị là `myXORkey`.
![image](https://hackmd.io/_uploads/Hk-YDgBz1x.png)
- XOR `string` với `he` lại lần nữa, tìm được flag.

##### ##### c) Kết quả:
`crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`

#### 5. Lemur XOR: 
##### a) Mô tả đề bài:
- Tôi đã ẩn hai hình ảnh thú vị bằng XOR bằng cùng một khóa bí mật để bạn không thể nhìn thấy chúng!
- Thử thách này yêu cầu thực hiện XOR trực quan giữa các byte RGB của hai hình ảnh - không phải XOR của tất cả các byte dữ liệu của tệp.
    
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/B1RtKQLMJl.png)
    Phân tích như hình.
    
##### c) Kết quả:
`crypto{X0Rly_n0t!}`
![tmpe30u6f2t](https://hackmd.io/_uploads/rkfJFmIGye.png)

### III. MATHEMATICS:
#### 1. Greatest Common Divisor:
##### a) Mô tả đề bài:
- Ước chung lớn nhất (GCD), đôi khi được gọi là ước chung lớn nhất, là số lớn nhất được chia hết bởi hai số nguyên dương `(𝑎, 𝑏)`.
- Với `𝑎 = 12`, `𝑏 = 8` chúng ta có thể tính các ước của `𝑎: {1, 2, 3, 4, 6, 12}` và các ước của `𝑏: {1, 2, 4, 8}`. So sánh hai điều này, chúng ta thấy rằng `gcd(𝑎, 𝑏) = 4`.
- Bây giờ hãy tưởng tượng chúng ta lấy `𝑎 = 11`, `𝑏 = 17`. Cả hai `𝑎` và `𝑏` là số nguyên tố. Vì số nguyên tố chỉ có chính nó và `1` là ước số, `gcd(𝑎, 𝑏) = 1`.
- Ta nói rằng với hai số nguyên bất kỳ `𝑎`, `𝑏`, nếu `gcd(𝑎, 𝑏) = 1` thì `𝑎` và `𝑏` là các số nguyên tố cùng nhau.
- Nếu như `𝑎` và `𝑏` đều là số nguyên tố thì chúng là nguyên tố cùng nhau. Nếu như `𝑎` là số nguyên tố và `𝑏 < 𝑎` thì `𝑎` và `𝑏` là nguyên tố cùng nhau.
- Hãy nghĩ về trường hợp của `𝑎`, một số nguyên tố và `𝑏 > 𝑎`, tại sao những cái này không nhất thiết là nguyên tố cùng nhau?
- Có nhiều công cụ để tính GCD của hai số nguyên, nhưng đối với nhiệm vụ này, chúng tôi khuyên bạn nên tra cứu Thuật toán Euclid.
- Hãy thử mã hóa nó; nó chỉ có một vài dòng. Sử dụng `𝑎 = 12`, `𝑏 = 8` để kiểm tra nó.
- Tính toán `gcd(𝑎, 𝑏)` cho `𝑎 = 66528`, `𝑏 = 52920`.

##### b) Phân tích cách làm:
**Thuật toán Euclid**: UCLN của của hai số nguyên không thay đổi khi thay số lớn hơn bằng hiệu của nó với số nhỏ hơn. Quá trình thay thế này được lặp đi lặp lại cho tới khi hai số bằng nhau, khi đó UCLN chính là một trong hai số đó.
![image](https://hackmd.io/_uploads/ryyt1rLGkg.png)
- Khai báo `a` và `b` là hai số nguyên nhập từ bàn phím.
- Gọi hàm, thực hiện các điều kiện và vòng lặp.
    - Nếu `a = 0` và `b = 0` thì in ra `a + b`.
    - Nếu `a != b`, thực hiện vòng lặp theo **thuật toán Euclid**. Vòng lặp kết thúc khi `a = b`.
- In kết quả.

##### c) Kết quả:
`1512`

#### 2. Extended GCD:
##### a) Mô tả đề bài:
- Cho `𝑎` và `𝑏` là số nguyên dương. Thuật toán Euclide mở rộng là một cách hiệu quả để tìm số nguyên `𝑢`, `𝑣` thoả mãn `𝑎⋅𝑢 + 𝑏⋅𝑣 = gcd(𝑎, 𝑏)`.
- Sau này, khi ta học cách giải mã bản mã RSA, chúng ta sẽ cần thuật toán này để tính nghịch đảo module của số mũ công khai.
- Sử dụng hai số nguyên tố `𝑝 = 26513`, `𝑞 = 32321`, tìm các số nguyên `𝑢`, `𝑣` thoả mãn `𝑝⋅𝑢 + 𝑞⋅𝑣 = gcd⁡(𝑝, 𝑞)`.
- Chọn giữa `𝑢` và `𝑣` giá trị thấp hơn làm flag.
- Biết rằng 𝑝, 𝑞 là số nguyên tố, bạn mong đợi `gcd(𝑝, 𝑞)` là gì? Tìm hiểu chi tiết thêm về thuật toán Euclide mở rộng.
        
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/r1ubSiPG1l.png)
- Khai báo thư viện `sympy` và dùng `gcdex()` để tìm kết quả.
- Nhập `p`, `q` từ bàn phím và in ra kết quả (có dạng `(u, v, gcd(p, q))`).
    
##### c) Kết quả:
`-8480`

#### 3. Modular Arithmetic 1:
##### a) Mô tả đề bài:
- Hãy tưởng tượng bạn nghiêng người và nhìn vào sổ ghi chép của một nhà mật mã. Bạn thấy một số ghi chú ở lề:
```
4 + 9 = 1
5 - 7 = 10
2 + 3 = 5
```
- Lúc đầu bạn có thể nghĩ họ bị điên. Bạn có thể nghĩ đây là lý do tại sao ngày nay có rất nhiều vụ rò rỉ dữ liệu, nhưng đây không gì khác hơn là modulo số học modulo 12 (mặc dù có một số ký hiệu cẩu thả).
- Có thể bạn không gọi nó là số học modulo, nhưng bạn đã thực hiện những phép tính kiểu này kể từ khi bạn học cách xem giờ (xem lại các phương trình đó và nghĩ đến việc cộng giờ).
- Về mặt hình thức, "tính toán thời gian" được mô tả bằng lý thuyết về sự đồng dư. Ta nói hai số nguyên bằng nhau theo modulo m nếu `𝑎 ≡ 𝑏 mod 𝑚`.
- Một cách khác để nói điều này là khi chúng ta chia số nguyên `𝑎` bởi `𝑚`, phần còn lại là `𝑏`. Điều này cho biết rằng nếu `𝑚` chia `𝑎` (có thể được viết là `𝑚/𝑎`) thì `𝑎 ≡ 0 mod 𝑚`.
- Tính các số nguyên sau:
`11 ≡ 𝑥 mod 6`
`8146798528947 ≡ 𝑦 mod 17`
- Đáp án là số nhỏ hơn trong hai số nguyên `(𝑥,𝑦)` thu được sau khi giảm theo modulo.
- Module là phép toán chia lấy dư: `𝑎 ≡ 𝑏 mod 𝑚` hay `𝑎 ≡ (𝑏 mod 𝑚)` nghĩa là `𝑏 = 𝑎 % 𝑚`.
    
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/SJewvf9zkg.png)
- Thực hiện tính toán theo yêu cầu.
- In kết quả nhỏ hơn trong cặp `(𝑥,𝑦)` ra màn hình.

##### c) Kết quả:
`4`
    
#### 4. Modular Arthimetic 2:
##### a) Mô tả đề bài:
- Chúng ta sẽ tiếp thu thử thách cuối cùng và tưởng tượng rằng chúng ta đã chọn được một modulo `𝑝`, và chúng ta sẽ giới hạn mình vào trường hợp khi `𝑝` là số nguyên tố.
- Modulo số nguyên `𝑝` xác định một trường, ký hiệu `𝐹𝑝`.
Nếu modulus không phải là số nguyên tố thì tập hợp các số nguyên theo modulo `𝑛` xác định một vành.
- Một trường hữu hạn `𝐹𝑝` là tập hợp các số nguyên 
`0`, `1`,..., `𝑝 − 1`, và trong cả phép cộng và phép nhân đều có các phần tử nghịch đảo `𝑏+` và `𝑏∗` cho mọi phần tử `𝑎` trong tập hợp, sao cho `𝑎 + 𝑏+ = 0` và `𝑎⋅(𝑏∗) = 1`.
- Lưu ý rằng phần tử nhận dạng của phép cộng và phép nhân là khác nhau! Điều này là do danh tính khi được thực hiện với toán tử sẽ không làm gì cả: `𝑎 + 0 = 𝑎` và `𝑎 ⋅ 1 = 𝑎`.
- Ta chọn `𝑝 = 17`. Tính toán `3^17 mod 17`. Bây giờ làm tương tự nhưng với `5^17 mod 17`.
- Bạn mong đợi nhận được gì: `7^16 mod 17`? Thử tính toán điều đó.
- Sự thật thú vị này được gọi là định lý nhỏ Fermat. Chúng ta sẽ cần điều này (và những khái quát hóa của nó) khi xem xét mật mã RSA.
- Bây giờ lấy số nguyên tố `𝑝 = 65537`. Tính toán `273246787654^65536 mod 65537`.

##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/ByfAmFizyx.png)
- Khai báo thư viện.
- Chia lấy dư theo đề cho và in kết quả.

##### c) Kết quả:
`1`
    
#### 5. Modular Inverting:
##### a) Mô tả đề bài:
- Như ta thấy, có thể làm việc trong một trường hữu hạn `𝐹𝑝`, cộng và nhân các phần tử và luôn thu được phần tử khác của trường.
- Đối với tất cả các phần tử 𝑔 trong trường tồn tại một số nguyên duy nhất **𝑑** như vậy `𝑔⋅𝑑 ≡ 1 mod p`.
- Đây là nghịch đảo của phép nhân `𝑔`.
> Ví dụ: `7⋅8 = 56 ≡ 1 mod 11`
- Phần tử nghịch đảo là gì: `𝑑 = 3^(−1)` như vậy `3⋅𝑑 ≡ 1 mod 13`?
- Hãy nghĩ về định lý nhỏ mà chúng ta vừa làm. Điều này giúp bạn tìm nghịch đảo của một phần tử như thế nào?
- Định lý nhỏ Fermat: `a^(p−1) ≡ 1 (mod p)`.
        
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/Hyynx5iM1l.png)
- Khai báo thư viện.
- Chạy vòng lặp cho `d` tăng dần một đơn vị, nếu `(3.d) mod 13 = 1` thì in `d` ra màn hình.

##### c) Kết quả:
`9`
    
#### IV. DATA FORMATS:
### 1. Privacy - Enhanced Mail?
##### a) Mô tả đề bài:
- Như chúng ta đã thấy trong phần mã hóa, mật mã liên quan đến việc xử lý dữ liệu ở nhiều định dạng khác nhau: số nguyên lớn, byte thô, chuỗi hex, v.v. Một số định dạng có cấu trúc đã được chuẩn hóa để giúp gửi và nhận dữ liệu mật mã. Nó giúp có thể nhận biết và thao tác các định dạng dữ liệu phổ biến này.
- PEM là định dạng phổ biến để gửi khóa, chứng chỉ và tài liệu mật mã khác. Nó trông giống như:
```
**-----BEGIN RSA PUBLIC KEY-----
MIIBCgKC... (a whole bunch of base64)
-----END RSA PUBLIC KEY-----**
```
- Nó bao bọc dữ liệu được mã hóa base64 bằng đầu trang và chân trang một dòng để cho biết cách phân tích dữ liệu bên trong. Có lẽ thật bất ngờ, điều quan trọng là phải có số lượng dấu gạch ngang chính xác ở đầu trang và chân trang, nếu không các công cụ mật mã sẽ không thể nhận dạng tệp.
- Dữ liệu được mã hóa base64 là các giá trị ASN.1 được mã hóa DER. Bối rối? Các tài nguyên được liên kết bên dưới có thêm thông tin về ý nghĩa của những từ viết tắt này nhưng sự phức tạp là có vì lý do lịch sử và việc đi quá sâu vào chi tiết có thể khiến bạn phát điên.
- Trích xuất khóa riêng d dưới dạng số nguyên thập phân từ khóa RSA có định dạng PEM này.
- Có hai cách tiếp cận chính để giải quyết thách thức này. Dữ liệu trong chứng chỉ có thể được đọc bằng công cụ dòng lệnh openssl hoặc bằng Python bằng `PyCryptodome`. Chúng tôi khuyên bạn nên sử dụng `PyCryptodome`: trước tiên hãy nhập module RSA bằng `from Crypto.PublicKey import RSA` và bạn có thể đọc dữ liệu chính bằng `RSA.importKey()`.
    
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/rk1_kXfmJx.png)
- Khai báo thư viện.
- Khai báo biến với giá trị cần mã hoá.
- Mã hoá và giải mã.

#### c) Kết quả:
```
15682700288056331364787171045819973654991149949197959929860861228180021707316851924456205543665565810892674190059831330231436970914474774562714945620519144389785158908994181951348846017432506464163564960993784254153395406799101314760033445065193429592512349952020982932218524462341002102063435489318813316464511621736943938440710470694912336237680219746204595128959161800595216366237538296447335375818871952520026993102148328897083547184286493241191505953601668858941129790966909236941127851370202421135897091086763569884760099112291072056970636380417349019579768748054760104838790424708988260443926906673795975104689
```

#### 2. CERTainly not:
##### a) Mô tả đề bài:
- Xử lý file `DER` (giúp mã hoá và lưu các chứng chỉ) tương tự file PEM.
- Trích xuất thông tin public key. Lấy modulus `(n)` và in ra flag dưới dạng số thập phân.
    
##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/rJySIDmQ1e.png)

##### c) Kết quả:
```
22825373692019530804306212864609512775374171823993708516509897631547513634635856375624003737068034549047677999310941837454378829351398302382629658264078775456838626207507725494030600516872852306191255492926495965536379271875310457319107936020730050476235278671528265817571433919561175665096171189758406136453987966255236963782666066962654678464950075923060327358691356632908606498231755963567382339010985222623205586923466405809217426670333410014429905146941652293366212903733630083016398810887356019977409467374742266276267137547021576874204809506045914964491063393800499167416471949021995447722415959979785959569497
```

#### 3. SSH Keys:
##### a) Mô tả đề bài:
- Giới thiệu về SSH (Secure Shell): Là giao thức bảo mật giúp mã hoá và và xác thực kết nối từ xa giữa máy cilent và server, phổ biến trong quản trị hệ thống. Nó sử dụng cặp khoá công khai - tư nhân để xác thực và mã hoá dữ liệu.
- Đề bài cho một file `.pub` là một dạng lưu trữ dữ liệu tương tự PEM, yêu cầu trích xuất modulus `n` dưới dạng số nguyên thập phân từ khoá công khai SSH RSA của Bruce. Khoá công khai có dạng `ssh-rsa`, chứa thông tin về modulus và exponent, được sử dụng trong thuật toán RSA. 

##### b) Phân tích cách làm:
![image](https://hackmd.io/_uploads/BkAVB_mmJg.png)

##### c) Kết quả:
```
3931406272922523448436194599820093016241472658151801552845094518579507815990600459669259603645261532927611152984942840889898756532060894857045175300145765800633499005451738872081381267004069865557395638550041114206143085403607234109293286336393552756893984605214352988705258638979454736514997314223669075900783806715398880310695945945147755132919037973889075191785977797861557228678159538882153544717797100401096435062359474129755625453831882490603560134477043235433202708948615234536984715872113343812760102812323180391544496030163653046931414723851374554873036582282389904838597668286543337426581680817796038711228401443244655162199302352017964997866677317161014083116730535875521286631858102768961098851209400973899393964931605067856005410998631842673030901078008408649613538143799959803685041566964514489809211962984534322348394428010908984318940411698961150731204316670646676976361958828528229837610795843145048243492909
```

#### 4. Transparency:
##### a) Mô tả đề bài:
Tìm kiếm miền phụ subdomain trong `cryptohack.org` mà sử dụng khoá RSA được cung cấp trong tệp `transparency.pem`.
    
##### b) Phân tích cách làm:
- Sử dụng crt.sh để tìm subdomain.
- Search `cryptohack.org`.
![image](https://hackmd.io/_uploads/BkoPq24mJe.png)
- Tìm từ khoá `transparency` bằng `Ctrl` + `F` và vào phần link ở cột `Matching Identities`, ta tìm được flag.
    
##### c) Kết quả:
`crypto{thx_redpwn_for_inspiration}`
