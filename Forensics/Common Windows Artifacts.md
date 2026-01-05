## A. TỔNG HỢP LAB WRITE - UP:
[Write - up](https://hackmd.io/wTkJzIUhS6Kbe6ScoER2EQ)

## B. LÝ THUYẾT:
- Hệ điều hành Windows gồm nhiều thành phần như file, logs, registries, lịch sử trình duyệt, user accounts và các dữ liệu quan trọng khác để giữ cho nó hoạt động bình thường.
- Một số thành phần hữu dụng trong Forensics, vị trí và cách trích xuất thông tin giá trị từ nó:
![image](https://hackmd.io/_uploads/Hyc-vQq0ll.png)

### 1. Windows Registry:
- Là một cơ sở dữ liệu phân cấp lưu trữ cấu hình cho người dùng, app và phần cứng. Microsoft mô tả nó: *"Registry bao gồm thông tin mà Windows tham chiếu tới trong quá trình vận hành như profiles của users, app được tải trên máy tính và loại tài liệu nó có thể tạo ra, bảng thuộc tính thiết lập cho các thư mục và icon app, hay phần cứng nào có trong hệ thống và cổng đang được sử dụng"*.
- Windows Registry gồm Keys và Values. Khi mở `regedit.exe` để xem registry, thư mục ta thấy là Registry Keys, Registry Values là dữ liệu nằm trong Registry Keys. Registry Hive là một nhóm Keys, Subkeys và giá trị ở file đơn trên đĩa.
- Windows Registry gồm năm khoá gốc chính được gọi là hives, nằm trong khoá gốc `Computer`:
![image](https://hackmd.io/_uploads/HkyuLb2Tge.png)
    - `HKEY_CURRENT_USER (HKCU)`: Gồm thông tin người dùng đăng nhập (màu screen, cài đặt control pannel).
    - `HKEY_USERS (HKU)`: Gồm thông tin người dùng đang được tải lên hệ thống (profiles và cài đặt).
    - `HKEY_LOCAL_MACHINE (HKLM)`: Gồm thông tin về cấu hình hệ thống (cho bất kỳ người dùng nào).
    - `HKEY_CLASSES_ROOT (HKCR)`: Gồm thông tin về các loại files và chương trình liên quan.
    - `HKEY_CURRENT_CONFIG (HKCC)`: Gồm thông tin về profile phần cứng của hệ thống.
- Bảng thông tin đầy đủ:
<table border="1" cellspacing="0" cellpadding="8" style="border-collapse: collapse; width: 100%; text-align: left; border: 1px solid #555;">
  <thead style="background-color: #2d2d2d; color: #fff;">
    <tr>
      <th style="width: 25%;">Khóa</th>
      <th>Mô tả (Tiếng Việt)</th>
    </tr>
  </thead>
  <tbody style="background-color: #1e1e1e; color: #ddd;">
    <tr>
        <td><code>HKEY_CURRENT_USER</code></td>
      <td>Chứa gốc của thông tin cấu hình cho người dùng đang đăng nhập hiện tại. Các thư mục người dùng, màu màn hình và cài đặt Control Panel của người dùng được lưu ở đây. Thông tin này liên kết với hồ sơ (profile) của người dùng. Khóa này đôi khi được viết tắt là <b>HKCU</b>.</td>
    </tr>
    <tr>
        <td><code>HKEY_USERS</code></td>
        <td>Chứa tất cả các hồ sơ người dùng đang được nạp trên máy tính. <code>HKEY_CURRENT_USER</code> là một subkey của <code>HKEY_USERS</code>. Khóa này đôi khi được viết tắt là <b>HKU</b>.</td>
    </tr>
    <tr>
      <td><code>HKEY_LOCAL_MACHINE</code></td>
      <td>Chứa thông tin cấu hình đặc thù cho máy tính (cho mọi người dùng). Khóa này đôi khi được viết tắt là <b>HKLM</b>.</td>
    </tr>
    <tr>
      <td><code>HKEY_CLASSES_ROOT</code></td>
      <td>
        Là một subkey của <code>HKEY_LOCAL_MACHINE\Software</code>. Thông tin được lưu ở đây đảm bảo chương trình đúng sẽ mở khi bạn mở một tệp bằng Windows Explorer. Khóa này đôi khi được viết tắt là <b>HKCR</b>.<br><br>
        Bắt đầu từ Windows 2000, thông tin này được lưu cả dưới hai khóa:<br>
        &nbsp;&nbsp;• <code>HKEY_LOCAL_MACHINE\Software\Classes</code>: chứa các cài đặt mặc định cho tất cả người dùng.<br>
        &nbsp;&nbsp;• <code>HKEY_CURRENT_USER\Software\Classes</code>: chứa các cài đặt ghi đè cho người dùng tương tác hiện tại.<br><br>
        Khóa <code>HKEY_CLASSES_ROOT</code> cung cấp cái nhìn hợp nhất của hai nguồn trên. Để thay đổi cài đặt cho người dùng hiện tại, hãy thay đổi dưới <code>HKEY_CURRENT_USER\Software\Classes</code>.<br><br>
        Để thay đổi cài đặt mặc định cho tất cả người dùng, hãy thay đổi dưới <code>HKEY_LOCAL_MACHINE\Software\Classes</code>.<br><br>
        Nếu bạn ghi khóa mới vào <code>HKEY_CLASSES_ROOT</code>, hệ thống sẽ lưu vào <code>HKEY_LOCAL_MACHINE\Software\Classes</code>. Nếu khóa đã tồn tại trong <code>HKEY_CURRENT_USER\Software\Classes</code>, hệ thống sẽ lưu ở đó thay vì HKLM.
      </td>
    </tr>
    <tr>
      <td><code>HKEY_CURRENT_CONFIG</code></td>
      <td>Chứa thông tin về cấu hình phần cứng được máy tính cục bộ sử dụng khi khởi động hệ thống.</td>
    </tr>
  </tbody>
</table>

- Mỗi một khoá gồm một hệ thống phân cấp khoá con và các giá trị lưu trữ thông tin đặc biệt về hệ thống và cấu hình của nó. Ví dụ, cấu hình cho chuột (độ nhạy,...) và tốc độ double click nằm ở `Computer\HKEY_CURRENT_USER\Control Panel\Mouse`. Có thể biểu diễn nó theo sơ đồ cây trong đó mỗi nhánh đại diện cho một khoá con:
```
Computer
|__ HKEY_CURRENT_USER
	|__ Control Panel
		|__ Mouse
```
- Khoá registry có thể thấy và sửa đổi bằng cách dùng Registry Editor tích hợp sẵn. Hai cách để mở:
    - Sử dụng Search bar:
    ![image](https://hackmd.io/_uploads/r1pbr-2all.png)
    - Mở Run (Windows + R):
    ![image](https://hackmd.io/_uploads/HyGrSbnpgg.png)
    - Giao diện:
    ![image](https://hackmd.io/_uploads/HkgdHKfRxe.png)
    Có thể thấy các root keys ở phần bên trái gồm các registry keys và dữ liệu khi nhấp vào nó.
- Ta có thể điều chỉnh độ nhạy của chuột trong phần cài đặt và quan sát sự thay đổi phản ánh trong registry keys:
![image](https://hackmd.io/_uploads/Hyf5B-nTgx.png)
- Cách tiếp cận thay thế là làm ngược lại bằng cách sửa đổ các khoá registry và quan sát sự thay đổi sau khi khởi động lại máy.
- Nếu truy cập duy nhất một disk image thì ta phải biết được vị trí của registry hives nằm ở đâu trên disk. Đa số ở `C:\Windows\System32\Config` là:
    - `DEFAULT` (gắn với `HKEY_USERS\DEFAULT`).
    - `SAM` (gắn với `HKEY_LOCAL_MACHINE\SAM`).
    - `SECURITY` (gắn với `HKEY_LOCAL_MACHINE\Security`).
    - `SOFTWARE` (gắn với `HKEY_LOCAL_MACHINE\Software`).
    - `SYSTEM` (gắn với `HKEY_LOCAL_MACHINE\System`).
- Ngoài các hives trên, hai loại hives khác bao gồm thông tin user có thể tìm thấy ở User profile directory. Trong Win7 và above, user’s profile directory nằm ở `C:\Users\<username>\` với hives là hidden file:
    - `NTUSER.DAT` (gắn với `HKEY_CURRENT_USER` khi user đăng nhập vào). Ở `C:\Users\<username>\`:
    ![image](https://hackmd.io/_uploads/ryOYzR7Age.png)
    - `USRCLASS.DAT` (gắn với `HKEY_CURRENT_USER\Software\CLASSES`). Ở `C:\Users\<username>\AppData\Local\Microsoft\Windows`: 
    ![image](https://hackmd.io/_uploads/HyEJXRmCee.png)
- Thông tin sâu hơn ở: [Windows registry information for advanced users](https://learn.microsoft.com/en-us/troubleshoot/windows-server/performance/windows-registry-advanced-users)

### 2. AmCache:
- Ngoài các hive trên ra, có một hive cực kì quan trọng khác là AmCache hive, nằm ở `C:\Windows\AppCompat\Programs\Amcache.hve`.
- Windows tạo ra hive này để lưu thông tin trên chương trình mà thường xuyên được chạy trên hệ thống. Nó lưu trữ các thông tin như file name, path, mã băm SHA-1 và lần thực thi đầu tiên.
- Artifact này có ích cho việc theo lịch sử dùng app và phát hiện sự hiện diện của phần mềm độc hại. Bằng cách phân tích AmCache, ta có thể xác định chương trình nào đang chạy, thực thi và thu thập bằng chứng của những apps trái phép.
- Tool để phân tích AmCache của Eric Zimmerman: AmCacheParser.
### 3. Registry transaction logs và Backups:
Sources dữ liệu forensics quan trọng khác là registry transaction logs và backups:
- Transaction logs được xem là nhật ký thay đổi của registry hive. Windows thường dùng nó khi viết dữ liệu đến registry hives, có nghĩa là transaction logs thường có những cập nhật mới nhất trong registry mà chưa được đưa vào registry hives.
- Transaction cho mỗi hive được lưu là `.LOG` trong thư mục chứa hive đó. Ví dụ, transaction log cho `SAM` hive sẽ nằm ở `C:\Windows\System32\Config` vớu tên `SAM.LOG`. 
- Thỉnh thoảng cũng có nhiều transaction logs, trong trường hợp này nó sẽ có phần mở rộng `.LOG1`, `.LOG2`,...
- Registry backups là sự đối lập với Transaction logs. Backups của registry hives nằm ở `C:\Windows\System32\Config`. Các hives này copy `C:\Windows\System32\Config\RegBack` mỗi mười ngày. Có thể là nơi hợp lý để kiểm tra nếu ta nghi ngờ một số registry keys có thể đã bị xoá hoặc điều chỉnh.
    
### 4. NTUSER.DAT:
- `NTUSER.DAT` là một tệp hồ sơ người dùng, một phần của registry. Cấu hình cho mỗi người dùng nằm ở `C:\Users\%USERNAME%\NTUSER.DAT`.
- Nội dung của file `NTUSER.DAT` có thể truy cập bằng cách dùng Registry Explorer của Eric Zimmerman ([Link](https://ericzimmerman.github.io/#!index.md) tải tool):
![image](https://hackmd.io/_uploads/S1xZzhAnTxx.png)

### 5. LNK Files:
- Là một shortcut file giúp truy cập nhanh đến các file thường dùng, thư mục hoặc chương trình trên hệ thống. Những file này đặc thù có một file extension `.lnk` và có thể tìm thấy ở những vị trí như desktops, start menu và thư mục documents gần đây.
- Bất cứ khi nào một file được truy cập lần đầu,`.lnk` sẽ được tạo ra trong thư mục thường dùng. Thông tin này đặc biệt hữu dụng để xác định xem khi nào một file lần đầu được truy cập, vị trí, ngày tạo, lần cuối điều chỉnh và thời gian truy cập.
- Có thể nằm ở:
    - `C:\Users\%USERNAME%\Recent`
    - `C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Windows\Recent`
    - `C:\Users\%USERNAME%\AppData\Roaming\Microsoft\Office\Recent`
    - `C:\Users\%USERNAME%\Desktop`
- Một cách để thăm dò file `.lnk` và tìm được thông tin hữu ích từ nó là dùng tool LECmd. Có thể dùng nó để trích ra đầy đủ địa chỉ gốc của file, thời gian file được tạo, sửa đổi lần cuối, truy cập lần cuối, tên desktop mà địa chỉ MAC của hệ thống nơi chứa file được tạo. Hoặc dùng tool LnkParse3 để phân tích LNK Files.
- Tool LECmd có thể download ở [link](https://github.com/EricZimmerman/LECmd). Cách dùng nó và trích xuất thông tin hữu dụng từ `.lnk`:
```
PS C:\Tools\LECmd> LECmd.exe -f passwd.lnk
LECmd version 1.5.0.0

Author: Eric Zimmerman (saericzimmerman@gmail.com)
https://github.com/EricZimmerman/LECmd

Command line: -f .\passwd.lnk

Warning: Administrator privileges not found!

Processing C:\Tools\LECmd\passwd.lnk

Source file: C:\Tools\LECmd\passwd.lnk
  Source created:  2021-03-15 08:25:27
  Source modified: 2021-03-15 10:36:47
  Source accessed: 2023-01-28 16:24:36

--- Header ---
  Target created:  2021-03-15 08:25:07
  Target modified: 2021-03-15 08:26:59
  Target accessed: 2021-03-15 08:27:07

  File size: 13
  Flags: HasTargetIdList, HasLinkInfo, HasRelativePath, HasWorkingDir, IsUnicode, DisableKnownFolderTracking
  File attributes: FileAttributeArchive
  Icon index: 0
  Show window: SwNormal (Activates and displays the window. The window is restored to its original size and position if the window is minimized or maximized.)

Relative Path: ..\..\..\..\..\Desktop\passwd.txt
Working Directory: C:\Users\Challenger\Desktop

--- Link information ---
Flags: VolumeIdAndLocalBasePath

>> Volume information
  Drive type: Fixed storage media (Hard drive)
  Serial number: 02916957
  Label: (No label)
  Local path: C:\Users\Challenger\Desktop\passwd.txt

--- Target ID information (Format: Type ==> Value) ---

  Absolute path:

  -File ==> (None)
    Short name: passwd.txt
    Modified:    2021-03-15 08:27:00
    Extension block count: 1

<SNIP>

--- End Target ID information ---

--- Extra blocks information ---

>> Tracker database block
   Machine ID:  desktop-rsrl4hd
   MAC Address: 08:00:27:2a:dc:e0
   MAC Vendor:  PCS SYSTEMTECHNIK
   Creation:    2021-03-11 06:29:01

   Volume Droid:       c2c1754c-762e-4361-8c04-e690782765de
   Volume Droid Birth: c2c1754c-762e-4361-8c04-e690782765de
   File Droid:         119740ae-8233-11eb-9779-0800272adce0
   File Droid birth:   119740ae-8233-11eb-9779-0800272adce0

<SNIP>

---------- Processed C:\Tools\LECmd\passwd.lnk in 0.25970300 seconds ----------
```
- Một số link chứa kiến thức sâu hơn về LNK files:
    - [Rise of LNK (Shortcut files) Malware](https://www.mcafee.com/blogs/other-blogs/mcafee-labs/rise-of-lnk-shortcut-files-malware/)
    - [LNK File Analysis: LNKing It Together!](https://syedhasan010.medium.com/forensics-analysis-of-an-lnk-file-da68a98b8415)

### 6. Web Browsers:
- Trình duyệt web dùng để truy cập các websites toàn cầu. Trong bối cảnh của forensics kỹ thuật số, trình duyệt web có thể cung cấp một lượng thông tin lớn về lịch sử truy cập, cookies, file download, mật khẩu đã lưu,... của một user. Thông tin này có thể dùng để xác định những gì user đã làm và một số hoạt động khả nghi.
- Có rất nhiều trình duyệt trực tuyến khả dụng, nhưng ta chỉ xem xét hai loại phổ biến nhất là Firefox và Chrome:

#### a) Firefox:
- Firefox lưu trữ những dữ liệu giá trị cho việc điều tra digital forensics ở đường dẫn `C:\Users\%USERNAME%\AppData\Roaming\Mozilla\Firefox\Profiles`. Nó gồm cookies, thông tin đăng nhập đã lưu, lịch sử trình duyệt, dấu trang và những thông tin hữu ích khác trong file `.sqlite` và `json`.
- Để xem file `json`, có thể dùng bất kỳ trình soạn thảo văn bản nào:
![image](https://hackmd.io/_uploads/SyZhfJppee.png)
- Tuy nhiên, file `sqlite` yêu cầu DB Browser như `sqlitebrowser` được tích hợp sẵn trong Kali Linux. Có thể dùng `sqlitebrowser` để xem file `.sqlite` bằng cách:
```
$ sqlitebrowser places.sqlite
```
![image](https://hackmd.io/_uploads/r1GgryTpgx.png)
- Firefox mã hoá các thông tin đăng nhập đã lưu bằng cách sử dụng master key trong file `key4.db` ở profile's directory. Để trích xuất usernames và passwords, ta có thể dùng tool `firefox_decrypt` tải ở [link](https://github.com/unode/firefox_decrypt).
- Những ví dụ cho lệnh hướng dẫn làm cách nào để tải và dùng tool:
``` ubuntu
$ git clone https://github.com/unode/firefox_decrypt 
Cloning into 'firefox_decrypt'...
remote: Enumerating objects: 1163, done.
remote: Counting objects: 100% (275/275), done.
remote: Compressing objects: 100% (40/40), done.
remote: Total 1163 (delta 250), reused 238 (delta 233), pack-reused 888
Receiving objects: 100% (1163/1163), 414.55 KiB | 1.14 MiB/s, done.
Resolving deltas: 100% (732/732), done.
```
``` ubuntu
$ python3 firefox_decrypt.py hxdvwqnb.default-release 

Website:   https://www.facebook.com
Username: 'john.doe@example.com'
Password: 'my_password123'

Website:   https://twitter.com
Username: 'john_doe'
Password: 'my_twitter_password123'

Website:   https://google.com
Username: 'john.doe@gmail.com'
Password: 'some_random_p4ssw0rd'
```

#### b) Chrome:
Khi phân tích chi tiết cách tìm passwords đã lưu từ Firefox bằng cách dùng `firefox_decrypt`, ta không đi sâu vào quá trình này trong Chrome. Tuy nhiên, chú ý là Chrome lưu trữ dữ liệu (mật khẩu đã lưu, cookies và nhiều thông tin hữu ích khác) ở `C:\Users\%USERNAME%\AppData\Local\Google\Chrome\User Data\Default`. Những dữ liệu này được mã hoá mặc định nhưng có thể tìm thấy key ở `C:\Users\%USERNAME%\AppData\Local\Google\Chrome\User Data\Local State`.

### 7. Event Logs:
- Logs sự kiện cung cấp event logs quan trọng của hệ thống, bảo mật và các app. Nó ghi lại tự động bởi Windows và có thể cung cấp những thông tin giá trị cho việc điều tra digital forensics.
- Các logs này nằm ở `C:\Windows\System32\winevt\Logs`, có thể được dùng và phân tích bằng Event Viewer, tool tích hợp sẵn trong Windows.
- Trong ba loại event logs hệ thống, bảo mật và app, chỉ có logs bảo mật là đáng quan tâm. File log này chứa những event liên quan đến bảo mật có bao gồm:
    - `Security.evtx`
    - `Microsoft-Windows-Windows Defender%4Operational.evtx`
    - `Microsoft-Windows-Windows Firewall With Advanced Security%4Firewall.evtx`
    - `Microsoft-Windows-PowerShell%4Operational.evtx`
> *Chúng không phải file duy nhất hữu ích cho việc điều tra digital foresics.*
- Mỗi một sự kiện có một ID cụ thể liên quan đến chúng, có thể dùng để xác định và lọc ra những event cụ thể đáng quan tâm.
> Ví dụ: Một ID event cho một người dùng đăng nhập có thể là `4624`, trong khi ID sự kiện cho một người dùng đăng xuất có thể là `4634`.
- Bảng danh sách một số ID event bảo mật phổ biến (cung cấp bản tóm tắt ngắn vê thông tin có thể thu thập được từ mỗi event):
<table style="border-collapse:collapse; width:100%; font-size:15px;">
  <thead>
    <tr style="background-color:#f2f2f2;">
      <th style="border:1px solid #444; padding:8px; text-align:right; width:80px;">Event ID</th>
      <th style="border:1px solid #444; padding:8px; text-align:left;">Tóm tắt event</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1006</td>
      <td style="border:1px solid #444; padding:6px;">Công cụ chống phần mềm độc hại phát hiện phần mềm độc hại hoặc phần mềm có khả năng không mong muốn.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1007</td>
      <td style="border:1px solid #444; padding:6px;">Nền tảng chống phần mềm độc hại đã thực hiện hành động để bảo vệ hệ thống khỏi phần mềm độc hại hoặc phần mềm có khả năng không mong muốn.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1015</td>
      <td style="border:1px solid #444; padding:6px;">Nền tảng chống phần mềm độc hại phát hiện hành vi đáng ngờ.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1116</td>
      <td style="border:1px solid #444; padding:6px;">Nền tảng chống phần mềm độc hại phát hiện phần mềm độc hại hoặc phần mềm có khả năng không mong muốn.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1117</td>
      <td style="border:1px solid #444; padding:6px;">Nền tảng chống phần mềm độc hại đã thực hiện hành động để bảo vệ hệ thống khỏi phần mềm độc hại hoặc phần mềm có khả năng không mong muốn.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1100</td>
      <td style="border:1px solid #444; padding:6px;">Dịch vụ ghi sự kiện đã tắt.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">1102</td>
      <td style="border:1px solid #444; padding:6px;">Nhật ký kiểm toán đã bị xóa.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4624</td>
      <td style="border:1px solid #444; padding:6px;">Một tài khoản đã đăng nhập thành công.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4625</td>
      <td style="border:1px solid #444; padding:6px;">Một tài khoản đăng nhập không thành công (đăng nhập thất bại).</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4634</td>
      <td style="border:1px solid #444; padding:6px;">Một tài khoản đã đăng xuất.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4648</td>
      <td style="border:1px solid #444; padding:6px;">Có nỗ lực đăng nhập sử dụng thông tin xác thực rõ ràng (explicit credentials).</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4720</td>
      <td style="border:1px solid #444; padding:6px;">Một tài khoản người dùng đã được tạo.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4722</td>
      <td style="border:1px solid #444; padding:6px;">Một tài khoản người dùng đã được bật (kích hoạt).</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4723</td>
      <td style="border:1px solid #444; padding:6px;">Có nỗ lực thay đổi mật khẩu của một tài khoản.</td>
    </tr>
    <tr>
      <td style="border:1px solid #444; padding:6px; text-align:right;">4724</td>
      <td style="border:1px solid #444; padding:6px;">Có nỗ lực đặt lại mật khẩu của một tài khoản.</td>
    </tr>
  </tbody>
</table>
> Một ví dụ của ID sự kiện `1117` trong file `Microsoft-Windows-Windows Defender%4Operational.evtx` được xem bằng cách sử dụng Event Viewer được tích hợp sẵn:
> ![image](https://hackmd.io/_uploads/r1lnJsyTage.png)

### 8. Thông tin và tài khoản hệ thống:
Để đọc dữ liệu registry, phải biết về nơi cần tìm nó.

#### a) OS Version:
Nếu chỉ có dữ liệu phân loại để làm forensics, có thể xác định OS version mà dữ liệu được kéo qua registry. Để tìm OS version, dùng registry key `SOFTWARE\Microsoft\Windows NT\CurrentVersion`. Cách Registry Explorer hiển thị:
![image](https://hackmd.io/_uploads/H1J8_-S0gx.png)

#### b) Current control set:
- Các hives gồm dữ liệu về cấu hình máy dùng để kiểm soát hệ thống khởi động gọi là Control Sets. Thông thường, ta thấy hai Control Sets là `ControlSet001` và `ControlSet002` trong `SYSTEM` hive. Trong nhiều trường hợp (không phải tất cả), `ControlSet001` chỉ tới Control Set mà máy khởi động với, và `ControlSet002` là cấu hình `last known good`. Vị trí của chúng là `SYSTEM\ControlSet001` và `SYSTEM\ControlSet002`.
- Windows tạo ra một Control Set biến mất khi máy đang hoạt động gọi là `CurrentControlSet `(`HKLM\SYSTEM\CurrentControlSet`). Để lấy các thông tin máy chính xác, đây là hive nên tham khảo tới. Có thể tìm cái Control Set đang được dùng như `CurrentControlSet` bằng các nhìn giá trị registry `SYSTEM\Select\Current`.
- Tương tự, cấu hình `last known good` có thể được tìm bằng cách dùng giá trị registry `SYSTEM\Select\LastKnownGood`.
- Một ví dụ trong Registry Explorer:
![image](https://hackmd.io/_uploads/ryOwibBRge.png)
- Cực quan trọng để xác định thông tin này trước khi phân tích. Dễ thấy nhiều aritfacts được thu thập trong Control Sets.

#### c) Computer Name:
- Việc xác định Computer Name trong khi phân tích forensics đảm bảo rằng ta đang làm việc đúng trên máy cần giám định. Có thể tìm Computer Name ở `SYSTEM\CurrentControlSet\Control\ComputerName\ComputerName`.
- Hiển thị trong Registry Explorer:
![image](https://hackmd.io/_uploads/SkVET-B0lg.png)

#### d) Thông tin về Time Zone:
- Để chính xác hơn thì cần xác định time zone của máy nằm ở đâu. Điều này giúp ta biết thời gian các event xảy ra. Để tìm thông tin time zone, vào phần `SYSTEM\CurrentControlSet\Control\TimeZoneInformation`.
- Hiển thị trong Registry Explorer:
![image](https://hackmd.io/_uploads/S1MiyGBRle.png)
- Thông tin về time zone rất quan trọng vì một số dữ liệu có thời UTC/GMT khác nhau và số khác thì giờ địa phương. Kiến thức về thời gian địa phương giúp xác định timeline khi thu thập dữ liệu từ tất cả sources.

#### e) Network Interfaces và Past Networks:
- Registry Key `SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces` cho một danh sách giao diện mạng trên máy đang điều tra.
![image](https://hackmd.io/_uploads/B1-zZzSCxe.png)
- Mỗi giao diện biểu diễn với một khoá con định danh duy nhất (GUID), gồm các giá trị liên quan tới cấu hình TCP/IP của giao diện. Key này cung cấp những thông tin như địa chỉ IP, DHCP IP và Subnet Mask, DNS Server,... Nó giúp đảm bảo ta đang thực hiện forensics trên máy cần điều tra.
- Các past network máy tính đã kết nối có thể thấy ở:
    - `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanaged`
    - `SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed`
![image](https://hackmd.io/_uploads/B10GzzSRxx.png)
- Các registry keys trên gồm past networks cũng như thời gian chúng được kết nối tới. Thời gian ghi nhận mới nhất mà registry key trỏ tới là lần cuối cùng mà các networks này kết nối.

#### f) Autostart Programs (Autoruns):
- Registry keys chứa thông tin về programs hoặc commands được chạy khi một user đăng nhập:
    - `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Run`
    - `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\RunOnce`
    - `SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce`
    - `SOFTWARE\Microsoft\Windows\CurrentVersion\policies\Explorer\Run`
    - `SOFTWARE\Microsoft\Windows\CurrentVersion\Run`
![image](https://hackmd.io/_uploads/S1VUXMrClx.png)
- Các registry keys chứa thông tin về services: `SYSTEM\CurrentControlSet\Service`.
- Chú ý giá trị Star key trong ví dụ:
![image](https://hackmd.io/_uploads/HyI5mzrRgg.png)
Nếu `star` key để là `0X02` thì nó có nghĩa là service này sẽ bắt đầu khi khởi động.

#### g) SAM hive và thông tin user:
- SAM hive chứa thông tin user account, thông tin đăng nhập và thông tin nhóm. Các thông tin này chủ yếu nằm ở `SAM\Domains\Account\Users`.
![image](https://hackmd.io/_uploads/rJmPBGHRlx.png)
- Các thông tin ở đây gồm định danh tương đối (RID) của user, số lần user đăng nhập, lần cuối đăng nhập, lần cuối đăng nhập thất bại, lần cuối mật khẩu được đổi, mật khẩu hết hạn, chính sách mật khẩu và hint, và một số group mà user là thành viên.

### 9. Files và folders:
#### a) Các files gần đây (Recent Files):
- Windows duy trì danh sách các files vừa mở của mỗi user. Có thể thấy rằng Registry Explorer shows list files vừa mở, thông tin này nằm ở `NTUSER.DAT` hive: `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs`.
![image](https://hackmd.io/_uploads/BJVe_hD0gl.png)
- Registry Explorer cho phép sắp xếp dữ liệu trong registry keys một cách nhanh chóng. Ví dụ, `Recent documents` tab sắp xếp Most Recently Used (MRU) file ở đầu danh sách. Ta thấy the Most Recently Used (MRU) file nằm ở đầu và các file cũ hơn nằm dưới.
- Có các keys khác với file extensions như `.pdf`, `.jpg`, `.docx`,... Keys này cung cấp thông tin về các files đã mở gần đây nhất của file extension cụ thể. Nếu tìm kiếm cụ thể file PDF gần nhất đã mở, có thể xem registry key `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs\.pdf`.
- Registry Explorer cũng liệt kê theo thời gian mở file gần nhất (the Last Opened time).

#### b) Office Recent Files:
- Tương tự Recent Docs được bảo quản bởi Windows Explorer, Microsoft Office giữ danh sách các documents đã mở gần đây, nằm ở `NTUSER.DAT` hive, địa chỉ là `NTUSER.DAT\Software\Microsoft\Office\<VERSION>`. Mỗi version của mỗi Microsoft Office là khác nhau.
![image](https://hackmd.io/_uploads/BJClhhPAlg.png)
- Bắt đầu từ Office 365, hiện tại Microsoft liên kết vị trí với ID trực tiếp của user. Trong trường hợp này, recent files có thể tìm thấy ở `NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU`. Vị trí này cũng lưu đường dẫn đầy đủ của các files mở gầy đây.

#### c) ShellBags:
- Các cài đặt folder đã sửa đổi vẫn tồn tại khi truy cập chúng nằm ở Shellbags. Đây là các registry keys sử dụng bởi Windows để theo dõi folder settings, gồm views, sizes, positions,... Chúng không chỉ theo dõi mỗi settings cho các local folders mà còn mở rộng chức năng sang folders trên network drives và các thiết bị có thể tháo rời. Thông tin trong Shellbag vẫn có thể tồn tại sau khi folder bị xoá.
- Nếu có user mở một folder thì nó mở trong một layout cụ thể và user có thể thay đổi layout này tuỳ sở thích. Các layout có thể khác nhau tuỳ folder khác nhau. Thông tin về Windows `shell` có thể được lưu trữ và nhận diện the Most Recently Used files và folders. Từ khi các cài đặt cho mỗi user này khác nhau, nó nằm ở user hive và có thể thấy thông tin này ở:
    - `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags`
    - `USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU`
    - `NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU`
    - `NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags`
- Bằng cách truy cập ShellBags thông qua Explorer trên local machine và network, có thể xác định chi tiết các thư mục đã truy cập, bao gồm những gì đã xoá hoặc di chuyển. Nó có thể tiết lộ file ẩn hoặc đã bị xoá, thư mục liên quan tới việc điều tra.
- Registry Explorer không đưa thông tin về ShellBags, tuy nhiên tool ShellBag Explorer của Eric Zimmerman thì có và nó đưa ra thông tin với format dễ sử dụng. Ta chỉ cần đi đến hive file cần trích xuất, nó sẽ phân tích dữ liệu và cho kết quả. Ví dụ:
![image](https://hackmd.io/_uploads/HycaYTP0lx.png)
- Tool để phân tích ShellBags của Eric Zimmermans: ShellBags Explorer.
- Kiến thức sâu hơn:
    - [Forensic Investigation: Shellbags](https://www.hackingarticles.in/forensic-investigation-shellbags/)
    - [Enterprise Threat Hunting: Shimmy Cache Style](https://www.digitalguardian.com/blog/enterprise-threat-hunting-shimmy-cache-style)
    - [Shellbags Analysis](https://medium.com/ce-digital-forensics/shellbag-analysis-18c9b2e87ac7)
    - [Forensic Analysis of Windows Shellbags](https://www.magnetforensics.com/blog/forensic-analysis-of-windows-shellbags/)

#### d) Open/Save và Dialog (hộp thoại) MRUs LastVisited:
Khi mở hoặc lưu file, một hộp thoại xuất hiện và hỏi nơi lưu hoặc mở file. Có thể thấy rằng một khi mở/lưu file tại địa chỉ cụ thể, Windows sẽ nhớ địa chỉ đó. Điều này cho thấy có thể tìm ra các file được mở gần đây nếu có được thông tin bằng cách xem xét registry keys:
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU`
![image](https://hackmd.io/_uploads/ByGbBCDRxe.png)
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\LastVisitedPidlMRU`
![image](https://hackmd.io/_uploads/SkFGrRv0lg.png)

#### e) Windows Explorer Address/Search Bars:
Cách khác để nhận diện hoạt động gần đây nhất của user là nhìn vào loại địa chỉ trong Windows Explorer address bar hoặc các tìm kiếm được thực hiện bằng cách mở lần lượt các registry keys:
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\TypedPaths`
![image](https://hackmd.io/_uploads/Bk6SLCPAxe.png)
- `NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery`

### 10. Bằng chứng của việc thực thi:
#### a) UserAssist:
Windows theo dõi các ứng dụng users khởi chạy bằng cách sử dụng Windows Explorer để thống kê trong User Assist registry keys. Các keys này bao gồm thông tin về các chương trình được khởi chạy, thời gian khởi chạy và số lần chúng thực thi. User Assist key có trong `NTUSER` hive, ánh xạ tới GUID của mỗi user. Có thể tìm thấy ở `NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count`.
![image](https://hackmd.io/_uploads/B1Qjgpu0gg.png)

#### b) ShimCache:
- ShimCache cho phép users chạy versions của apps trên hệ thống Windows hiện đại, là cơ chế theo dõi, đảm bảo khả năng tương thích ứng dụng với OS và theo vết tất cả apps khởi chạy trên máy, giúp tạo ra timeline của các chương trình thực thi. Không giống như những artifacts ghi dấu thời gian chính xác, ShimCache có thể cung cấp bằng chứng về sự thực thi nếu các logs khác bị xoá. Mục đích chính trong Windows là đảm bảo khả năng tương thích ngược của ứng dụng, được gọi là Application Compatibility Cache (AppCompatCache). Nằm ở `SYSTEM` hive với địa chỉ `SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache`.
- ShimCache bao gồm file name, file size và thời gian lần cuối sửa đổi của tệp thực thi.
- Registry Expolorer không phân tích dữ liệu ShimCache trong format người bình thường có thể đọc được nên ta dùng tool AppCompatCache Parser của Eric Zimmerman's. Nó dùng `SYSTEM` hive như input, phân tích dữ liệu và output là một CSV file, ví dụ:
![image](https://hackmd.io/_uploads/B1mqrTd0xx.png)
- Có thể dùng `AppCompatCacheParser.exe --csv <path to save output> -f <path to SYSTEM hive for data parsing> -c <control set to parse>` để chạy AppCompatCache Utility.
- Có thể xem output bằng EZviewer hoặc AppCompactCacheParser, tools khác của Eric Zimmerman.
- Kiến thức sâu hơn:
    - [Investigating ShimCache with Artifast ShimCache Artifact Parser](https://forensafe.com/blogs/shimcache.html)
    - [Let’s Talk About Shimcache — The Most Misunderstood Artifact](https://www.youtube.com/watch?v=7byz1dR_CLg)

#### c) AmCache:
- `AmCache` hive là artifact liên quan tới ShimCache, có chức năng tương tự với ShimCache, theo dõi các apps, loaded drivers và lưu trữ các dữ liệu được thêm vào liên quan tới program thực thi. Các dữ liệu này gồm địa chỉ thực thi, cài đặt, thời gian thực thi và xoá, hàm băm SHA1 của program thực thi. Hive này nằm ở file hệ thống `C:\Windows\appcompat\Programs\Amcache.hve` hay `%SystemRoot%\appcompat\Programs\Amcache.hve
Subkey: InventoryApplicationFile`:
![image](https://hackmd.io/_uploads/SyBfv6uCll.png)
- Thông tin về program thực thi mới nhất có thể tìm thấy trong hive `Amcache.hve\Root\File\{Volume GUID}\`. Cách Registry Explorer phân tích `AmCache` hive:
![image](https://hackmd.io/_uploads/Byhz_6OAex.png)
Ngoài ra có thể dùng tool AmcacheParser.
- Kiến thức sâu hơn:
    - [ShimCache & AmCache Forensic Analysis](https://medium.com/@mehrnoush/shimcache-amcache-forensic-analysis-99a8a9733772)
    - [AmCacheParser: Reducing the noise, fidign the signal](https://binaryforay.blogspot.com/2015/07/amcacheparser-reducing-noise-finding.html)
    - [Leveraging the Windows Amcache.hve File in Forensic Investiagations](https://web.archive.org/web/20180720134529id_/https://commons.erau.edu/cgi/viewcontent.cgi?article=1429&context=jdfsl)

#### d) BAM/DAM:
- BAM hay Background Activity Monitor (Giám sát hoạt động nền) giữ một tab hoạt động nền của các apps. Tương tự với DAM hay Desktop Activity Moderator (Điều hành hoạt động Desktop), theo dõi hoạt động của các ứng dụng có thể tương tác trên desktop, là một phần của Microsoft Windows để tối ưu hoá mức tiêu thụ điện năng của thiết bị. Cả hai đều là một phần của hệ thống Modern Standby của Microsoft Windows.
- Trong Windows registry, các vị trí chứa thông tin liên quan đến BAM và DAM gồm thông tin về program chạy mới nhất, địa chỉ đầy đủ và lần cuối thực thi:
    - `SYSTEM\CurrentControlSet\Services\bam\UserSettings\{SID}`
    ![image](https://hackmd.io/_uploads/Sy0Sh6_0xg.png)
    `{SID}` tượng trưng cho Security Identifier của user, gồm các entries của mỗi ứng dụng nền, ghi lại chi tiết lần cuối chúng đã chạy.
    - `SYSTEM\CurrentControlSet\Services\dam\UserSettings\{SID}`
    ![image](https://hackmd.io/_uploads/SJrOn6uCll.png)
    Giống như BAM, `{SID}` tượng trưng cho Security Identifier của user, ghi lại hoặc động của các desktop apps, cung cấp thời gian và dữ liệu sử dụng.
    
### 11. Thiết bị ngoài/Thiết bị USB forensics:
Khi thực hiện forensics trên máy phải xác minh xem có USB hoặc ổ đĩa ngoài nào gắn vào máy hay không. Nếu có, thông tin liên quan đến các thiết bị này rất quan trọng cho việc điều tra forensics. Các cách tìm các thông tin này bằng cách sử dụng registry:

#### a) Nhận diện thiết bị:
Vị trí theo dõi USB keys được cắm vào máy chứa ID nhà cung cấp, ID sản phẩm và version USB và nó có thể dùng để nhận diện các thiết bị lạ, nó cũng chứa thời gian USB được cắm vào hệ thống:
- `SYSTEM\CurrentControlSet\Enum\USBSTOR`
![image](https://hackmd.io/_uploads/r1Sz7ZF0eg.png)
- `SYSTEM\CurrentControlSet\Enum\USB`
![image](https://hackmd.io/_uploads/S1lEQbtAgl.png)

#### b) Lần đầu tiên/cuối cùng:
- Registry key theo dấu lần đầu tiên và lần cuối cùng thiết bị được kết nối, lần cuối cùng USB rút khỏi hệ thống: `SYSTEM\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326-97a6-4088-9453-a19231573b29}\####`
`####` có thể thay đổi để có được thông tin cần tìm:
![image](https://hackmd.io/_uploads/HJB6EZtAle.png)
- Mặc dù có thể kiểm tra các giá trị thủ công, Registry Explorer có thể phân tích và hiển thị các dữ liệu này nếu ta `USBTOR key`.
![image](https://hackmd.io/_uploads/SJN2VZKCel.png)

#### c) Tên USB device Volume:
- Tên thiết bị của ổ đĩa được kết nối có thể tìm ở `SOFTWARE\Microsoft\Windows Portable Devices\Devices`.
![image](https://hackmd.io/_uploads/r1fpr-Y0ex.png)
- Có thể so sánh GUID trong registry key này với Disk ID trong keys được đề cập ở thiết bị nhận diện để liên hệ tên với các thiết bị lạ.
- Kết hợp tất cả các thông tin này, có thể tạo ra một bức tranh về những USB đã kết nối tới thiết bị đang điều tra.

### 12. Jumplists:
- Là artifact cung cấp danh sách files và documents đã truy cập gần đây bằng cách nhấp chuột phải trên taskbar icon. Phân tích các files này cung cấp các thông tin tương tự LNK Files. Lists này chủ yếu gồm hai loại:
    - Automatic Destinations: 
        - Tạo bởi OS cho mỗi app, chứa các URLs, files và folders thường mở. Nó tuân theo OLE compound file format, chứa nhiều luồng. Các luồng này chứa dữ liệu tương tự như trong LNK files.
        - Vị trí: `C:\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations`
    - Destinations and Custom Destinations:
        - Được tạo khi users pin items vào jumplist. Cung cấp chức năng dành riêng cho ứng dụng, có thể thay đổi tuỳ quyết định của nhà phát triển.
        - Vị trí: `C:\Users\<Username>\AppData\Roaming\Microsoft\Windows\Recent\CustomDestinations`
- Jumplist có thể tìm ở các thư mục cụ thể và được liên kết với các ứng dụng cụ thể. Việc phân tích Jumplists cho phép xem hoạt động gần đây của user, cung cấp cái nhìn về các files và documents được mở hoặc làm việc gần đây trên Windows TaskBar hoặc Start menu.
- Tool phân tích Jumplists: Jumplists Explorer (của Eric Zimmerman) hoặc JLEcmd.
- Kiến thức sâu hơn:
    - [Jump Lists](https://forensics.wiki/jump_lists/)
    - [Windows 10 Jump List and Link File Artifacts - Savedm Copied and Moved](https://dfir.pubpub.org/pub/wfuxlu9v/release/1)
    - [Analyzing Jump Lists with Belkasoft Evidence Center](https://belkasoft.com/analyzing_jump_lists_with_belkasoft_evidence_center)

### 13. Page và Hibernation Files:
- Page files (`pagefile.sys`) và Hibernation file (`hiberfil.sys`) dùng bởi Windows để quản lý bộ nhớ và trạng thái hệ thống. Khi một hệ thống Windows ở mode Hibernation, nó tạo ra Hibernation File trong root folder nơi đặt hệ thống vận hành (`C:\`). File này chứa bản sao của RAM trước khi hệ thống vào trạng thái Hiberation, là source giá trị để thực hiện forensics khi hệ thống bị tắt.
- Paging file là một phần của sơ đồ quản lý bộ nhớ Windows, chứa những phần bộ nhớ ảo, giúp Windows di chuyển nội dung bộ nhớ không truy cập thường xuyên từ bộ nhớ vật lý hoặc RAM vào nó. Nó nằm trong `C:\`, cung cấp cách thực hiện forensics khi hệ thống tắt.
- Một số dữ liệu có thể lấy ra từ files trên gồm passwords, key mã hoá và các mảnh tài liệu. Registry Path của `pagefile.sys` và `hiberfil.sys`:
    - `C:\pagefile.sys`
    - `C:\hiberfil.sys`

### 14. Một số Forensic Artifacts khác:
#### a) New Technology File System (NTFS):
- Là hệ thống file dùng trong Windows versions hiện tại trước NTFS có FAT16/FAT32 (File Allocation Table) và HPFS (High Performance File System). Ngày nay có thể thấy FAT vẫn được sử dụng, ví dụ trong USB, MicroSD cards,... nhưng thường không trong Windows computers/laptops hay Windows Server.
- NTFS biết đến như một hệ thống log file. Trong trường hợp thất bại, hệ thống file có thể tự động sửa folders/files trên ổ đĩa bằng cách sử dụng thông tin được lưu trữ trọng log file. Chức năng này không khả dụng với FAT.
- NTFS giải quyết nhiều hạn chế của các hệ thống file trước đó:
    - Hỗ trợ files lớn hơn 4GB.
    - Set các quyền cụ thể trên folders và files.
    - Nén folder và file.
    - Mã hoá (Encryption File System hay EFS).
- Nếu đang chạy Windows, Windows installation đang dùng file system nào? Có thể check `Properties` (chuột phải) vào drive mà hệ thống vận hành được cài đặt, chọn `C:\`.
- Có thể đọc tài liệu chính thức của Microsoft về FAT, HPFS và NTFS ở [đây](https://learn.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems).
- Với NTFS, có thể set một số [quyền](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/bb727008(v=technet.10)?redirectedfrom=MSDN) cho phép hoặc từ chối truy cập tới files và folders:
![image](https://hackmd.io/_uploads/B15FZdQlZe.png)
- Cách xem các quyền của một file hay folder:
    - Chuột phải vào file hoặc folder.
    - Ở context menu, chọn `Properties`.
    - Chọn tab `Security`.
    - Trong danh sách `Group or user names`, chọn user, computer hay group chứa quyền muốn xem.
    ![image](https://hackmd.io/_uploads/ry0rfOmgWe.png)

#### b) Master File Table (MFT):
##### Định nghĩa:
- Là thành phần core của hệ thống NTFS file, gồm bản ghi chi tiết tất cả files và thư mục trên một ổ đĩa (tên, thời gian, thuộc tính). MFT làm việc như một index tới tất cả files và thư mục trên ổ đĩa, cung cấp việc truy cập nhanh tới thông tin cần thiết để lấy lại file.
- Bằng cách phân tích MFT, có thể khám phá thông tin về cách tạo file, sự điều chỉnh và thời gian xoá, giúp thành lập timeline và phục hồi các file đã xoá.
- Mỗi một file và thư mục trên một đĩa NTFS có một bản ghi đặc biệt trong MFT, như mục nhập MFT. Mục nhập MFT chứa các thông tin như tên file, dấu thời gian, các quyền và con trỏ tới file's data. Mục nhập MFT tương ứng được cập nhất khi một file được tạo hoặc điều chỉnh.
- Khi file bị xoá, mục nhập MFT tương ứng được đánh dấu là trống, nhưng file data thực sự vẫn nằm trên đĩa cho đến khi nó bị viết lại bởi data mới. Việc này có thể hữu dụng trong trường hợp khôi phục lại data, như các file's data bị xoá vẫn có thể khôi phục. Việc phục hồi thành công dữ liệu là ổ đĩa được chiếm bởi data đã bị xoá không bị ghi đè.
##### Cấu trúc của bản ghi MFT:
- Cấu trúc của MFT trong hệ thống NTFS file rất phức tạp và bao gồm nhiều bản ghi, mỗi một cái đại diện một file hay thư mục trên đĩa NTFS. Mỗi bản ghi MFT là 1024 bytes, giúp MFT dễ dàng được phân tích. Một bản ghi MFT có chung cấu trúc:
    - File Record Header: Gồm các thông tin về bản ghi (size, độ lệch của chuỗi update, flags biểu thị tình trạng của file hay thư mục).
    - File Atribute List: Gồm danh sách các thuộc tính mô tả file hoặc thư mục, gồm tên, dấu thời gian, size và dữ liệu. Mỗi thuộc tính được lưu trữ như một cấu trúc riêng lẻ với format của nó.
    - Data Runs: Mô tả vị trí của dữ liệu file hoặc thư mục trên đĩa. Data runs được lưu trữ như một loạt các phần mở rộng mô tả các nhóm bắt đầu và độ dài của mỗi data block liền kề.
- Format và cấu trúc chính xác của MFT trong NTFS có thể phụ thuộc vào version của hệ thống file sử dụng. Tuy nhiên, phần cấu trúc chung vẫn giống nhau với ba thành phần chính vừa nên trên của mục nhập MFT.
##### Các tools phân tích MFT:
- MFT là một hệ thống file trong hệ thống NTFS file và users không thể truy cập trực tiếp hay xem nó. Tuy nhiên, có các tools có thể dùng để đọc và phân tích nội dung MFT:
    - Tool của Eric Zimmerman: MFTParser.
    - DiskInternals NTFS Recovery: Có thể phục hồi các file hư tổn hoặc đã bị xoá bằng cách truy cập MFT.
    - HxD là một hex editor miễn phí cho phép xem raw data của MFT và các files đã phục hồi.
- Việc truy cập trực tiếp MFT có thể nguy hiểm và gây ra việc làm hệ thống file bị hỏng hay mất dữ liệu. Trước khi cố gắng truy cập hay điều chỉnh MFT, ta nên thực hiện nó trên backup của MFT hay hoặc sao lưu sẵn data quan trọng.
##### Sâu hơn về MFT:
- [Master File Table (MFT), NTFS, $LogFile, and $UsnJrnl: Forensics](https://mahmoud-shaker.gitbook.io/dfir-notes/master-file-table-mft-ntfs-usdlogfile-and-usdusnjrnl-forensics)
- [Master File Table](https://www.sciencedirect.com/topics/computer-science/master-file-table) và [Master File Table](https://learn.microsoft.com/en-us/windows/win32/devnotes/master-file-table)
- [Master File Table (Local File Systems)](https://learn.microsoft.com/en-us/windows/win32/fileio/master-file-table)
- [Windows Master file table, MFT forensics](https://cavementech.com/2022/05/mft-forensics.html)

#### c) Prefetch Files:
- Dùng bởi Windows để tăng tốc thời gian khởi chạy các apps. Ghi lại các files đã load bởi một app khi nó khởi động, cung cấp cái nhìn về việc thực thi chương trình và tần suất.
- Một prefetch file được tạo khi một app khởi chạy lần đầu, mục đích chính là nâng cao hiệu suất khởi động của app bằng cách load các dữ liệu cần thiết, đảm bảo trải nghiệm người dùng mượt mà hơn. Tất cả thời gian app được khởi chạy đều kết nối đến prefetch file để nhanh chóng lấy được dữ liệu yêu cầu.
- Việc phân tích prefetch file đóng vai trò quan trọng trong việc bóc tách thông tin có giá trị trong lịch sử thực thi của app.
- Format tên là `ApplicationName-Hash.pf` với `Hash` là vị trí khởi chạy của app. Prefetch files vẫn có thể tồn tại sau khi app đã bị gỡ bỏ.
- Những thông tin có được khi phân tích prefetch files:
    - Timestamps.
    - Tần suất thực thi.
    - Địa chỉ file thực thi.
    - Thư mục và files được tham chiếu.
    - Thông tin Volume.
- Vị trí: `C:\Windows\Prefetch\ApplicationName-Hash.pf`
![image](https://hackmd.io/_uploads/SJ3LsxWy-e.png)
- Tool: PECmd
- Tham khảo sâu hơn:
    - [Prefetch](https://forensics.wiki/prefetch/)
    - [Forensic Investigation: Prefetch File](https://www.hackingarticles.in/forensic-investigation-prefetch-file/)
    - [Disabling Prefetch](https://learn.microsoft.com/en-us/previous-versions/windows/embedded/ms940847(v=winembedded.5)?redirectedfrom=MSDN)

#### d) EML File:
- Một file `.eml` là định dạng thư điện tử hay plaintext version của một email mà user có thể lưu về máy. User có thể nhận và gửi files này qua RFC-822 compliant email programs như Microsoft Outlook. RFC-822 cấu thành format đơn giản và code cho các emails trên Internet.
- Mỗi cá nhân có thể xuất các file `.eml` này để lưu trữ và quét virus. Loại extension này có lợi từ khi nó cho phép bảo vệ format và các headers HTML gốc của emails. File `.eml` thường bao gồm các phần thông tin:
    - Nội dung email.
    - Dòng chủ đề.
    - Thông tin người gửi.
    - Thông tin người nhận.
    - Thời gian.
    - Tệp đính kèm (ảnh, files tài liệu,...).
    - Hyperlinks.
- Cách mở `.eml` bằng Windows:
    - Mở website kết nối email, có thể gồm các trang web như Microsoft Outlook. Click vào email icon hoặc nhập vào trình duyệt để mở email.
    - Khi vào đến email account, đi đến phần inbox rồi click vào email mà ta muốn mở file `.eml` rồi double - click vào nó để mở file `.eml`.
    - Sau khi double - click, `.eml` version sẽ xuất hiện trên màn hình. Có thể tải file này bằng cách nhấp chuột phải và nhấn phím download và lưu nó.

#### e) Alternate Data Streams (ADS):
- Là một tính năng trong hệ thống vận hành của Windows (New Technology File System - NTFS) cho phép dữ liệu được liên kết và ẩn trong files, nói cách khác là cho phép files chứa nhiều hơn hai luồng dữ liệu và user có thể gán luồng dữ liệu thứ hai đến một file ẩn mà đa số các apps hoặc users không thể thấy được. Tất cả file có ít nhất một luồng dữ liệu và mặc định trong Windows là `:$DATA`.
- Windows Explorer không cung cấp cách xem các luồng dữ liệu xen kẽ trong một file (hay cách loại bỏ chúng mà không xoá file) nhưng nó có thể được tạo và kết nối rất dễ. Vì nó khó để tìm nên thường được sử dụng bởi hackers để ẩn files trên máy mà chúng đã xâm nhập (có thể là files rootkit). Các tệp thực thi trong alternate data streams có thể thực thi từ các command nhưng nó không hiển thị trong Windows Explorer hay Console.
> Ví dụ cách tạo và kết nối alternate data streams:
> - Tạo Alternate Data Streams:
> `C:\> type C:\windows\system32\notepad.exe > c:\windows\system32\calc.exe:notepad.exe`
> `C:\> start c:\windows\system32\calc.exe:notepad.exe`
> - Kết nối `:$DATA` Alternate Data Stream: `C:\> start c:\textfile.txt::$DATA`
- Một ADS có thể dùng để lưu trữ các thông tin được thêm vào file như metadata hoặc comments mà không cần thay đổi file đó.
- ADS được đặt tên bằng cách dùng dấu hai chấm, ví dụ `myfile.txt:ads.txt`.
- ADS có thể dùng cho các ý định hợp pháp, như add metadata tới một file, nhưng nó có thể tận dụng cho các ý định hiểm độc như giấu malware hoặc những thông tin nhạy cảm khác trong file. Kết quả là ADS được dùng trong một số loại cyberattacks như rò rỉ dữ liệu âm thầm hay chỉ huy và điều khiển giao tiếp.
- Vì `:$DATA` alternate stream ở trong mọi tập tin nên nó có thể là cách thay thế để kết nối tới bất kì file nào. Bất kì apps nào tạo files hoặc xem được hoặc phụ thuộc vào đuôi của file name hay extension nên biết được khả năng về các ADS này. Nếu input của một user chưa được sanitized, tạo hoặc hoặc tham chiếu đến một file name, hacker có thể dùng luồng `:$DATA` để thay đổi hành vi phần mềm. Một vulnerability (lỗ hổng bảo mật) nổi tiếng có bản chất như thế này tồn tại trong các versions cũa của IIS. Khi IIS thấy request cho một file với một ASP extension, nó gửi ASP file tới app liên kết với extension. App này sẽ chạy server-side code trong ASP file và tạo ra HTML response cho request. Do sai lầm trong phân tích extension của các IIS versions này, `filename.asp::$DATA` đã không nối được với extension và khi đó không có app nào được đăng ký cho `asp::$DATA` extension và ASP source code quay lại với attacker.
> Exploiting the ASP Alternate Data Stream Show Code Vulnerability:
> - Kết nối bình thường: `http://www.alternate-data-streams.com/default.asp`
> - Show code bypass accessing the `:$DATA` alternate data stream: `http://www.alternate-data-streams.com/default.asp::$DAT`
> - Trong versions lỗ hổng bảo mật này, IIS phân tích extensions của file này như `asp::$DATA` chứ không phải ASP. Như vậy app liên kết với ASP extension đã không được gọi và ASP source code đã bị attacker xem được.
- Để xem và quản lý ADS, có thể dùng Windows command prompt hoặc các tools của bên thứ ba như ADS Spy, ADS Scanner,...
- Tham khảo thêm về ADS: [Introduction to Alternate Data Streams](https://www.malwarebytes.com/blog/101/2015/07/introduction-to-alternate-data-streams).

#### f) Scheduled Tasks:
- Là một trong những tính năng trên hệ điều hành Windows hữu ích với người dùng. Có thể dễ dàng thiết lập chương trình để chúng tự động khởi động ở một thời điểm nhất định đã đặt trước bằng Scheduled Tasks.
> Ví dụ:
> ![image](https://hackmd.io/_uploads/S1-XftklWx.png)
> - Từ giao diện có thể thấy tên task, status, thời gian mà user thiết lập để khởi chạy, lần chạy tới, lần gần nhất, và action của task. Ví dụ với task `GoogleUpdateTaskMachineCore` có action là chạy `GoogleUpdate.exe`:
> ![image](https://hackmd.io/_uploads/H1jjMF1x-x.png)
- Từ ví dụ, có thể thấy rằng điều này mở ra những khả năng thú vị cho attackers. Các tác vụ được lên lịch thường được sử dụng để khởi chạy phần mềm độc hại (chẳng hạn như một backdoor nào đó được thiết lập để tự động khởi chạy, hay một keylogger được cài đặt để tự động gửi những dữ liệu được thu thập trên máy tính nạn nhân đến nơi khác). Đây là vị trí không thể bỏ qua khi rà soát mã độc.

### 15. Remote Desktop Protocol (RDP) Cache:
#### a) Định nghĩa:
- Theo tài liệu chính thức của Microsoft: *"Bitmap caches được sử dụng bởi client và server để lưu trữ đồ hoạ bitmaps. Mỗi bitmap cache giữ các bitmaps của một size pixels chuyên dụng (size ô). Nếu một bitmap không vừa với một mục nhập cache, server dùng thuật toán tiling để phân chia bitmap thành các ô nhỏ vứa với mục nhập cach và nó có thể được lưu trữ tách riêng trong cache."*
- Về cơ bản, các images được lưu trữ nhiều hơn một lần. Đây là một chức năng hợp pháp từ xa từ thời Internet cực kì chậm và các RDP sessions diễn ra chậm chạp.
- Nơi lưu trữ cục bộ các images này là trên hệ thống client chứ không phải các server hỗ trợ kết nối, nên nếu có attackers sử dụng hệ thống của chúng để RDP đến host thì các files này sẽ không khả dụng để phân tích. Tuy nhiên, nếu chúng tình cờ sử dụng RDP trên các images trên hệ thống mạng của victim để di chuyển ngang, đó là lúc RDP Bitmap Cache phát huy tác dụng.
- Vị trí trong Win7: `C:\Users\<USER>\AppData\Local\Microsoft\Terminal Server Client\Cache\`
- Dưới thư mục này có thể thấy một số files khác. Cái đầu tiên tên là `bcache.bmc` và những cái khác là `Cachexxxx.bin` (`x` thay thế bởi các con số).
- Các `bcache` files có các con số gắn vào cuối tượng trưng cho chất lượng của images. Các số cao hơn gắn với bcache tương ứng với chất lượng cao hơn mà các bitmap images được tạo ra.
- Trong khi đó, `Cache` files được tạo với mỗi phiên trên hệ thống. Nếu quan sát một hệ thống bận rộn mà nó có nhiều phiên RDP của một user, cần trỏ đến vị trí bitmap caches liên quan để phân tích dữ liệu dựa trên dấu thời gian Windows.

#### b) BMC Tools:
- Là một công cụ open-source mạnh mẽ cho phép phân tích RDP Bitmap Cache phục vụ việc điều tra. BMC Tools được viết trong Python và có thể dễ dàng chạy trên Linux hoặc Windows với command như: `./bmc-tools.py -s cache0000 -d cache0000_parsed -b`. Bao gồm cả các option `-b` vì nó tạo ra một ảnh ghép tập hợp tất cả các ô cho ta một cách nhanh và dễ dàng để xem toàn bộ output. Những ô cá nhân sẽ trở nên lộn xộn:
![image](https://hackmd.io/_uploads/r1T9XOJeZx.png)
![image](https://hackmd.io/_uploads/rJ_sQOkxbe.png)

#### c) RDPieces:
Là một command line tool khác được viết ở Perl, theo mô tả repo trong GitHub, "*... sẽ phân tất cả dữ liệu được trích xuất trong thư mục RDP Bitmap Cache và nỗ lực để tự động tạo lại một số screenshots*".
> Ví dụ: `perl rdpieces.pl -source cache0002/ -output cache0002_rebuilt/`
> - Toàn bộ idea sau tool này là để lưu khi tạo lại các screenshots tới điểm mà ta có thể quan sát và hiểu nó. Ví dụ, nếu user chạy một command prompt và enter một command, ta hy vọng sẽ nhận đủ từ nó để biết được mục đích chính của command và hiểu những gì nó đang chạy trên hệ thống.
> - Trên thực tế, kết quả có thể đạt được hoặc không:
> ![image](https://hackmd.io/_uploads/HyJPB_klbx.png)

#### d) RDP Cache Sticher:
- Cho ai thích UI và chơi xếp hình, hoặc muốn nhờ một người nào đó ghép lại cache thay vì phụ thuộc vào một script để thử và làm công việc đó một cách tốt nhất có thể, dùng [RDP Cache Sticher](https://github.com/BSI-Bund/RdpCacheStitcher). Từ GitHub của nó: *"RDP là một tool hỗ trợ các nhà phân tích forensic trong việc dựng lại các images hữu dụng của RDP cache bitmaps. Bằng việc sử dụng các ô raw RDP cache bitmaps với các công cụ như [ANSSI's BMC-Tools](https://github.com/ANSSI-FR/bmc-tools) như input, nó cung cấp một giao diện user đồ hoạ và vài phương pháp tìm kiếm vị trí để xếp các ô lại với nhau và tạo thành các ảnh có ý nghĩa hoặc full screenshots."*
- Công cụ này giúp thấy được tất cả trong một cửa sổ riêng lẻ (có thể bị rời ra hoặc kéo đi xung quanh) và tiếp đó khi add images vào canvas, nó tự động suggest các images tương tự để giúp hoàn thành bức ảnh. Ví dụ:
![image](https://hackmd.io/_uploads/S1TesdkeZe.png)
![image](https://hackmd.io/_uploads/rJIketJebx.png)
![image](https://hackmd.io/_uploads/rJ5Jgt1gWx.png)

### 16. PowerShell history:
> [Link đầy đủ hơn](https://www.sharepointdiary.com/2020/11/powershell-command-history.html)

- Command Prompt (`cmd`) là cách tương tác với hệ thống vận hành. Khi GUI được tạo ra, cho phép users thực hiện tasks phức tạp với vài clicks thay vì gõ lệnh. Mặc dù GUI là cách chính để tương tác với hệ thống vận hành, người dùng vẫn có thể tương tác qua command prompt. 
- Mỗi lệnh sẽ có phần hướng dẫn để giải thích cú pháp dự kiến để command chạy, với bất kì tham số được add vào để mở rộng việc thực thi. Xem phần hướng dẫn bằng cách thêm `/?` sau mỗi lệnh:
![image](https://hackmd.io/_uploads/H1w3z6YlWg.png)
- `net` chủ yếu để quản lý tài nguyên mạng, hỗ trợ sub-commands. Nếu gõ `net` mà không có sub-command, output hiển thị cú phpá của root command cho thấy một vài sub-commands có thể dùng:
![image](https://hackmd.io/_uploads/rkXo7TtxZx.png)
Dùng `net help` thay vì `net /?`:
![image](https://hackmd.io/_uploads/HyTxN6Yg-x.png)
`net help user`:
![image](https://hackmd.io/_uploads/S1ozNptlWg.png)
Có thể dùng command giống nhau để xem thông tin hỗ trợ cho các `net` sub-commands hữu ích khác (`localgroup`, `use`, `share`, `session`).
- Một số commands đơn giản để thu thập thông tin về hệ thống máy:
    - `hostname` output tên máy:
    ![image](https://hackmd.io/_uploads/ByviWaYeZl.png)
    - `whoami` output tên user đăng nhập:
    ![image](https://hackmd.io/_uploads/HyjaWTKxZg.png)
    - `ipconfig` hiển thị cài đặt địa chỉ mạng của máy:
    ![image](https://hackmd.io/_uploads/B1JgGpYxbx.png)
    - `netstat` hiển thị số liệu thống kê giao thức và kết nối TCP/IP hiện tại:
    - ![image](https://hackmd.io/_uploads/ByEbmaFx-g.png)
- Tìm hiểu sâu hơn về các commands: [An A-Z Index of Windows CMD commands](https://ss64.com/nt/).
- Mặc định, Windows lưu tất cả các commands gõ trong Powershell console vào một text log file. Điều này cho phép chạy lại command bất kì và xem lịch sử PowerShell commands đã chạy, kể cả sau khi đóng console hoặc restart máy. PowerShell hiện nay dùng hai nhà cung cấp lịch sử command:
    - Lịch sử command theo phiên hiện tại (hiển thị bởi Get-History cmdlet).
    - Text log với các commands trước mà PSReadLine module đã lưu.

#### a) Xem PowerShell Command History trên Windows:
- Trong PowerShell console, command cuối cùng ta gõ xuất hiện khi nhấn phím Up (mũi tên lên), nếu tiếp tục nhấn Up, ta thấy các commands đã thực thi trước đó. Vì vậy, dùng phím `Up arrow` và `Down arrow` để cuộn toàn bộ lịch sử PowerShell Command đã chạy. Điều này hữu ích để cần chạy lại commands trước mà không cần gõ lại.
- PowerShell console giữ lịch sử command từ Windows PowerShell 5.1 (Win10). Trong các versions trước của Windows PowerShell (và cmnd command prompt), lịch sử của các commands đã chỉ có hiệu lực trong PowerShell session hiện tại. Dùng `Get-History` cmdlet để xem lịch sử các commands trước đó trong session.
- Có thể hiển thị chi tiết thông tin về các commands đã chạu trước đó trong Powershell session hiện tại, bao gồm các command status và thời gian bắt đầu/kết thúc/khoảng thời gian bằng: `Get-History | Format-List -Property *`
![image](https://hackmd.io/_uploads/SyWaSKeebe.png)
- Ta có thể command trước đó bằng ID của nó: `Invoke-History 6`
- Lịch sử command reset lại và danh sách liệt kê trong `Get-History` bị xoá khi ta đóng PowerShell console.
- Tuy nhiên, Windows PowerShell 5.1 và PowerShell Core lưu 4096 commands cuối cùng trong một plaintext file của mỗi user's profile: `%userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt`
![image](https://hackmd.io/_uploads/ByH7Ptxx-l.png)
- Có thể mở file này và xem lịch sử command bằng cách dùng bất kỳ text editor, ví dụ Notepad:
![image](https://hackmd.io/_uploads/BJqewFxlZg.png)
- History log được giữ tách biệt cho PowerShell console và PowerShell ISE.
> - Trong `cmd.exe`, vó thể hiển thị lịch sử command trong session hiện tại với: `doskey /history`.
> - Phím `F7` để search cmd history.
> ![image](https://hackmd.io/_uploads/rkV03jxlWl.png)

#### b) Cách search trong PowerShell Command History:
- Nếu không muốn cuộn toàn bộ PowerShell command history bằng cách dùng phím Up/Down, có thể search command history bằng cách dùng phím tắt `Ctrl` + `R` (tìm ngược) và `Ctrl` + `S` (tìm xuôi). Nhấn tổ hợp này và gõ một phần của command muốn tìm trong các commands đã chạy, phần text được entered sẽ tìm thấy trong bất kì vị trí nào (không giống tìm kiếm trong PowerShell bằng các dùng `F8` hay `Shift` + `F8`, cho phép tìm thấy các kết quả khớp duy nhất từ đầu dòng). PowerShell console nên hiển thị các commands trước đó tương ứng với từ đã search. Các dòng khớp sẽ được highlight trong command.
- Nhấn `Ctrl` + `R`/`Ctrl` + `S` một lần nữa để tiếp tục tìm kiếm nếu command hiện ra không phải cái cái cần tìm, những command tiếp theo tương ứng với từ khoá sẽ xuất hiện trên màn hình.
![image](https://hackmd.io/_uploads/r1tTnjge-e.png)
- Sử dụng phím `F8`, có thể tìm command khớp với text trên dòng command hiện tại. Ví dụ, gõ `get-` và nhấn `F8`, mục nhập cuối cùng trong lịch sử command khớp với text sẽ được tìm thấy. Để đi tới command tiếp theo, nhấn `F8` thêm lần nữa.
![image](https://hackmd.io/_uploads/H1FgTsxlZl.png)
- Có thể dùng `#` để tìm khắp command history. Ví dụ, để tìm command cuối cùng bắt đầu bằng `get-wmi`, gõ `#get-wmi` và nhấn phím `Tab`, command cuối cùng khớp với mẫu sẽ xuất hiện trong console.
![image](https://hackmd.io/_uploads/r1XWTieebg.png)
- Lịch sử command làm việc giống nhau trong cả classic Windows PowerShell và new PowerShell Core. Có thể update nhanh version PowerShell trong Win10 và Win11 bằng `winget install --id=Microsoft.PowerShell -e`.
- Để hiển thị danh sách các commands trong lịch sử khớp với truy vấn, có thể dùng `Get-History | Select-String -Pattern "Get-"` và grep kết quả bằng cách dùng pipe: `Get-Content (Get-PSReadlineOption).HistorySavePath| Select-String - Pattern "Get-"`.
![image](https://hackmd.io/_uploads/HkRWpoxeWg.png)

#### c) Định hình PowerShell Command History với PSReadLine Module:
- Chức năng command history trong PowerShell không được tích hợp sẵn trong Windows Management Framework nhưng phụ thuộc vào PSReadLine module, giúp nâng cao đáng kể chức năng của PowerShell console. PSReadLine module trong Windows nằm ở `C:\Program Files\WindowsPowerShell\Modules\PSReadline` và được tự động import khi khởi động PowerShell console.
- PSReadLine cung cấp cú pháp được highlight trong console, đảm nhiệm khả năng sử dụng lựa chọn text với chuột và copy/paste của nó bằng cách dùng `Ctrl` + `C` và `Ctrl` + `V`.
- Kiểm tra xem module đã load trong PowerShell session hiện tại chưa bằng `Get-Module`.
![image](https://hackmd.io/_uploads/ByWhpoelWg.png)
- Nếu PSReadLine chưa load, xác nhận nó được tải chưa. Nếu cần thiết, tải nó từ PowerShell Gallery online:
```powershell
Get-Module -ListAvailable | where {$_.name -like "*PSReadline"}
Install-Module PSReadLine
```
- Xem danh sách các chức năng của PSReadLine module để quản lý lịch sử commands trong PowerShell và các phím được gán cho chúng bằng `Get-PSReadLineKeyHandler | ? {$_.function -like '*hist*'}`:
```powershell
Key       Function                Description
---       --------                -----------
UpArrow   PreviousHistory         Replace the input with the previous item in the history
DownArrow NextHistory             Replace the input with the next item in the history
Ctrl+r    ReverseSearchHistory    Search history backwards interactively
Ctrl+s    ForwardSearchHistory    Search history forward interactively
Alt+F7    ClearHistory            Remove all items from the command line history (not PowerShell history)
F8        HistorySearchBackward   Search for the previous item in the history that starts with the current input - like NextHistory if the input is empty
Shift+F8  HistorySearchForward    Search for the next item in the history that starts with the current input - like NextHistory if the input is empty
Unbound   ViSearchHistoryBackward Starts a new seach backward in the history.
Unbound   BeginningOfHistory      Move to the first item in the history
Unbound   EndOfHistory            Move to the last item (the current input) in the history
```
![image](https://hackmd.io/_uploads/SkzgJnelWl.png)
- Để mở cài đặt lịch sử PowerShell command hiện tại trong PSReadLine module:
```powershell
Get-PSReadlineOption | select HistoryNoDuplicates, MaximumHistoryCount, HistorySearchCursorMovesToEnd, HistorySearchCaseSensitive, HistorySavePath, HistorySaveStyle
```
![image](https://hackmd.io/_uploads/H1WrJ2le-g.png)
- Có thể xem xét các PSReadLine parameters (tham số):
    - `HistoryNoDuplicates`: Có lưu lại các commands giống nhau không.
    - `MaximumHistoryCount`: Số lượng commands lớn nhất có thể lưu trữ (mặc định là 4096).
    - `HistorySearchCursorMovesToEnd`: Có cần thiết nhảy tới phần cuối của command khi search không.
    - `istorySearchCaseSensitive`: Một search có phải trường hợp nhạy cảm không (Lịch sử Powershell command mặc định không có trường hợp nhạy cảm).
    - `HistorySavePath`: Path của text file nơi lưu trữ lịch sử PowerShell commands.
    - `HistorySaveStyle`: Các options để lưu lại lịch sử:
        - `SaveIncrementally`: Những lệnh được lưu khi chạy (mặc định).
        - `SaveAtExit`: Lịch sử được lưu khi đóng PowerShell console.
        - `SaveNothing`: Không lưu lịch sử command.
- Có thể thay đổi cài đặt PSReadLine module với `Set-PSReadlineOption`. Ví dụ, để tăng số lượng PowerShell commands được lưu trữ trong log: `Set-PSReadlineOption -MaximumHistoryCount 10000`
- Nếu muốn lưu không chỉ mỗi các commands được chạy mà cả output của nó trong lịch sử PowerShell commands, dùng transcript. Add chức năng sau đến user's PowerShell profile (`notepad $profile.CurrentUserAllHosts`):
```powershell
Function StartTranscript {
Trap {
Continue
}
$TranScriptFolder = $($(Split-Path $profile) + '\TranscriptLog\')
if (!(Test-Path -Path $TranScriptFolder )) { New-Item -ItemType directory -Path $TranScriptFolder }
Start-Transcript -Append ($($TranScriptFolder + $(get-date -format 'yyyyMMdd-HHmmss') + '.txt')) -ErrorVariable Transcript -ErrorAction stop
}
StartTranscript
```
![image](https://hackmd.io/_uploads/r1djrAggZg.png)
- User profile bây giờ chứa chi tiết log file cho mỗi PowerShell session trong thư mục `%USERPROFILE%\Documents\WindowsPowerShell\TranscriptLog`.

#### d) Cách chạy PowerShell Command mà nó không lưu trong lịch sử:
- Trong Linux bash shell, có thể vô hiệu hoá lịch sử cho các lệnh bắt đầu với spaces (với `HISTCONTROL= ignorespace`). Ta có thể làm tương tự cho PowerShell.
- Add đoạn code sau vào user's PowerShell profile hiện tại (`$profile.CurrentUserAllHosts`):
```
Set-PSReadLineOption -AddToHistoryHandler {
    param($command)
    if ($command -like ' *') {
        return $false
    }
    return $true
}
```
![image](https://hackmd.io/_uploads/SkTnORxx-x.png)
- Điểm này yêu cầu set chính sách PowerShell Execution trên máy tính thành `Remotesigned`: `Set-ExecutionPolicy Remotesigned`. Từ giờ, bắt đầu command với space thì nó sẽ không được lưu trong lịch sử PowerShell command.
- Ngoài ra, từ version Readline v2.0.4 module, commands bao gồm các keywords tự động lờ đi và không lưu trong lịch sử: `Password`, `Asplaintext`, `Token`, `Apikey`, `Secret`.

#### e) Dùng Predictive IntelliSense với PowerShell Command History:
- Đặc trưng PowerShell mới Predictive IntelliSense là có thể dùng trong PSReadLine 2.2.2+. Điểm này hiển thị các commands thích hợp nhất từ local command history khi gõ một command vào PowerShell console.
> Ví dụ, gõ `get-wm` vào console, Predictive IntelliSense gợi ý một trong những command đã được nhập trước đó và khớp với input. Nếu command này phù hợp, nhấn phím `Right arrow` để accept command này mà không cần nhập lại các ký tự tiếp theo bằng cách thủ công.
> ![image](https://hackmd.io/_uploads/S1oxh0elWl.png)
- Mặc định, các Predictive IntelliSense hints được hiễn thị trong text xám và khá khó để đọc trên nền đen của PowerShell console. Để nó trở nên rõ ràng hơn: `Set-PSReadLineOption -Colors @{ InlinePrediction = '#7A957B'}`
- Từ PSReadLine 2.2.6, Predictive IntellinSense mặc định có thể dùng, có thể kích hoạt nó bằng cách thủ công: `Set-PSReadLineOption -PredictionSource History`
- Để reset gợi ý Predictive IntelliSense, nhấn phím `Esc`.
- Có thể chuyển sang chế độ xem khác bằng phím `F2`. Từ giờ, thay vì hiển thị một command gợi ý (`InlineView`), một danh sách sẽ thả xuống với tất cả commands (`ListView`).
![image](https://hackmd.io/_uploads/SJ2DAAxgWg.png)
Dùng phím up/down để chọn nhanh command ta cần.
> *Predictive IntelliSense làm việc trong tiêu chuẩn `pwsh.exe`/`powershell.exe` command line, cũng như trong Windows Terminal và Visual Studio Code.*

#### f) Cách xoá Command History trong PowerShell:
- PSReadLine module lưu tất cả PowerShell console commands thành một text file. Tuy nhiên trong một số trường hợp, admin phải enter nhiều thông tin nhạy cảm khác nhau trong PowewShell console (thông tin đăng nhập, mật khẩu, tokens, địa chỉ, dữ liệu cá nhân,...). Lịch sử dữ liệu là một plaintext file có thể truy cập bởi admin server hay attacker nào đó. Vì một vài lý do bảo mật, ta phải xoá lịch sử PowerShell commands đã chạy hay tắt hoàn toàn lịch sử command.
- `Clear-History` cmdlet cho phép xoá lịch sử commands trong PowerShell session hiện tại. Nó chỉ xoá các lệnh mà trước đó `Get-History` trả về.
- Có thể xoá duy nhất một command trong lịch sử: `Clear-History -count 1 -newest`. Hay xoá toàn bộ commands với một mẫu cụ thể: `Clear-History -CommandLine *set-ad*`
- Để xoá hoàn toàn lịch sử các PowerShell commands trước đó, cần xoá file `ConsoleHost_history.txt` mà PSReadLine module đã ghi lại. Có thể lấy địa chỉ file lịch sử PowerShell hiện tại và xoá nó bằng `Remove-Item (Get-PSReadlineOption).HistorySavePath`. Sau đó, đóng cửa sổ PowerShell console.
- Nếu muốn tắt hoàn toàn việc lưu lịch sử PowerShell commands vào một text file: `Set-PSReadlineOption -HistorySaveStyle SaveNothing`
![image](https://hackmd.io/_uploads/BkeTOaWe-x.png)

#### g) Cách export/import PowerShell Command History tới một Session khác:
- Thỉnh thoảng việc có một set các PowerShell commands thường dùng rất tiện lợi trên các máy tính khác nhau. Có thể export lịch sử commands hiện tại trên máy tính vào một `XML` file và import nó đến máy khác. Ta làm điều này bằng các copy file `ConsoleHost_history.txt` trong user's profile tới máy tính muốn đến.
- Có thể dùng `Export-Clixml` cmdlet để export lịch sử commands từ session hiện tại vào một text file: `Get-History | Export-Clixml -Path c:\ps\commands_hist.xml`
- Để import lịch sử commands từ một file ở PowerShell session nào đó (trên local computer hay máy khác): `Add-History -InputObject (Import-Clixml -Path c:\ps\commands_hist.xml)`
![image](https://hackmd.io/_uploads/HyfW5pbx-e.png)
- Để tự động export các commands trước đó vào một file khi PowerShell session kết thúc, có thể liên kết script với việc PowerShell session kết thúc (Session phải kết thúc với lệnh `exit`, không phải bằng cách đóng PowerShell console):
```powershell
$HistFile = Join-Path ([Environment]::GetFolderPath('UserProfile')) .ps_history
Register-EngineEvent PowerShell.Exiting -Action {Get-History | Export-Clixml $HistFile } | out-null
if (Test-path $HistFile) { Import-Clixml $HistFile | Add-History}
```

### 17. OST/PST File và tool Outlook Forensics Toolbox:
> *[Link tham khảo](https://outlook-forensic.recoverytoolbox.com/)*
- Outlook Forensic Toolbox được thiết kế để tiết lộ các thông tin ẩn và đã bị xoá từ các files Microsoft Outlook data (files với PST và OST extensions). Tool này thực hiện việc phân tích forensics dữ liệu lưu trữ của Microsoft Outlook, với các chủ đề được lưu như thư, danh bạ, lời nhắc, files, tệp đính kèm,...
- Outlook Forensic Toolbox khôi phục dữ liệu ẩn và đã xoá mà nó không hiển thị dưới dạng quan sát bình thường của PST/OST files trong Microsoft Outlook:
- Các thông tin ẩn:
    - Thư
    - Danh bạ
    - Tệp đính kèm
    - Cuộc họp
    - Lời nhắc
    - Tasks
    - Notes
    - Lịch
    - Magazines
![image](https://hackmd.io/_uploads/H131zCWgbl.png)
- Các objects được khôi phục một phần hay các thông tin từ objects:
    - Service headers của thư.
    - Các đoạn text.
    - Các phần của files.
    - Các phần, mảnh, blocks chữ cái.
    - Các objects lồng nhau.
- Nguyên tắc của chương trình là tìm kiếm các blocks mà dữ liệu không được cung cấp trong Microsoft Outlook. Những blocks ẩn với user data có thể hiển thị trong khi chạy Microsoft Outlook khi tối ưu hoá dữ liệu lưu trữ hay dữ liệu sau của user.
> Ví dụ, user nhận được một email không mong muốn hay nội dung của một tin nhắn kín và phải bị xoá. Trong trường hợp này, Microsoft Outlook user nhấn `Delete` và email sẽ dừng lại trong chương trình mail. Khi phím `Delete` bị huỷ, Outlook xoá thư/danh bạ/nhật ký hoặc các objects khác trong danh sách các objects. Nhưng blocks với các dữ liệu tin nhắn đã xoá vẫn nằm trong mục lưu trữ của Microsoft Outlook (trong PST/OST Files). Outlook Forensic Toolbox tìm thấy các block dữ liệu, phân tích nội dung của nói và dùng các phương pháp phân tích forensic phức tạp để tái tạo lại object trong tất cả hay chỉ một phần.
- Trong một số trường hợp, việc tạo lại một object hoàn toàn không khả dụng khi tất cả các blocks với thông tin về object không đủ vì bất kỳ lý do gì (bị viết đè lên trong file với dữ liệu khác, đường link tới object lồng nhau bị mất,...). Trong trường hợp phục hồi không đầy đủ một object đã bị xoá, các files riêng lẻ sẽ được lưu để nhận diện và tái tạo lại.
- Tất cả object được lưu bởi Outlook vào một file được lưu trữ trong vài blocks lên đến 512 bytes. Khi một tin nhắn bị xoá, các blocks này không bị xoá và vẫn ở trong file. Tuy nhiên, khi emails mới được gửi hay nhận, các blocks có thể bị ghi đè lên. Kết quả là, email bị xoá có thể chỉ được phục hồi một phần, ví dụ:
    - Nếu có một subject, phần body hay tệp đính kèm của tin nhắn có thể mất.
    - Nếu subject bị mất nhưng vẫn còn phần body, email sẽ được lưu trữ lại mà không cần phần From và To.
    - Nếu subject và body của tin nhắn bị ghi đè, tệp đính kèm sẽ được lưu như một file độc lập.
    - ...
- Giao diện:
    - Chọn file `*.pst` để phục hồi:
    ![image](https://hackmd.io/_uploads/ByfJHfmlbl.png)
    - Phần preview của data đã phục hồi:
    ![image](https://hackmd.io/_uploads/rJvRNG7gZx.png)
    - Chọn path để lưu data đã phục hồi:
    ![image](https://hackmd.io/_uploads/SyTTEGXgWx.png)
    - Chọn loại save của data đã phục hồi:
    ![image](https://hackmd.io/_uploads/HkCn4Gmebg.png)
    - Kết quả phục hồi:
    ![image](https://hackmd.io/_uploads/Bkr2EGmxZe.png)
- Cách hoạt động:
    - [Tải](https://recoverytoolbox.com/download/OutlookForensicToolboxInstall.exe) phần mềm.
    - Bắt đầu cài đặt phần mềm:
        - Nhấn vào downloaded installation file trong browser.
        - Chọn một downloaded installation file trong Windows Explorer và nhấn `Enter` hoặc double click vào nó.
    - Chạy trình hướng dẫn cài đặt:
    ![image](https://hackmd.io/_uploads/B1kcPf7gZe.png)
    - Chạy chương trình: Nháy chuột vào icon trên desktop hoặc chọn từ main menu: `Start`|`Programs`|`Outlook`|`Forensic Toolbox`|`Outlook Forensic Toolbox`.
    - Chọn file `*.pst` cần sửa từ Microsoft Mail profile hoặc từ ổ đĩa và `Next`.
    ![image](https://hackmd.io/_uploads/HJ3BDfXlZg.png)
    - Confirm việc bắt đầu phục hồi:
    ![image](https://hackmd.io/_uploads/BysLwfQeWg.png)
    - Review và chọn objects dã phục hồi để lưu rồi `Next`.
    - Chọn một folder để lưu và `Next`.
    - Chọn loại save.

### 18. Windows Fundamentals:
Hệ thống vận hành Windows là hệ thống vận hành vượt trội trong việc sử dụng tại nhà và doanh nghiệp, vì vậy Windows luôn là mục tiêu của hackers và người viết mã độc.

#### a) The Desktop (GUI):
- Windows Desktop, là giao diện đồ hoạ user hay ngắn gọn là GUI, là màn hình chính khi user đăng nhập vào máy.
- Thông thường, cần phải nhập mật khẩu đăng nhập. Màn hình đăng nhập là nơi nhập thông tin account hợp lệ, thường đó là username và password sẵn có trên Windows account trên hệ thống cụ thể hay trong môi trường Active Directory (nếu nó là máy đã tham gia domain).
![image](https://hackmd.io/_uploads/HJZ3GIXlWg.png)
- Ảnh trên là một ví dụ của Windows 10 Desktop thông thường. Các thành phần tạo nên GUI:
##### Desktop:
- Nnơi chứa các shortcuts đến các chương trình, folders, files,... Các icon này được tổ chức trong các folders theo Alphabet hay nằm rải rác ngẫu nhiên. Trong cả hai trường hợp, các items này được đặt trên trên desktop để truy cập nhanh.
- Phần hiển thị có thể được thay đổi tuỳ sở thích. Bằng cách nhấn chuột phải vào bất cứ đâu, một context menu sẽ xuất hiện. Menu này cho phép thay đổi sizes của desktop icons, cách ta sắp xếp, copy/paste items đến desktops và tạo items mới như folder, shortcut hay text document.
![image](https://hackmd.io/_uploads/HJXD4LQgZe.png)
- Dưới phần `Display settings`, có thể thay đổi độ phân giải hay hướng của màn hình. Trong trường hợp có nhiều computer screens, có thể thiết lập cấu hình cho nhiều màn hình:
![image](https://hackmd.io/_uploads/BJzyHLXeZg.png)
- Trong Remote Desktop session, một số cài đặt hiển thị sẽ không khả dụng:
![image](https://hackmd.io/_uploads/rydWHUQgbe.png)
- Trong mục `Personalize`, có thể thay đổi hình nền, font chữ, themes, bảng màu,...:
![image](https://hackmd.io/_uploads/S13kIIXxbg.png)
![image](https://hackmd.io/_uploads/ryeblIUXe-x.png)
##### Start Menu:
- Trong các versions trước của Windows, `Start` hiển thị ở góc trái của desktop GUI. Trong version hiện tại, từ này không xuât hiện nữa mà được thay bằng Windows Logo.
- Start Menu giúp truy cập tới tất cả apps/chương trình, files, tools có ích,...
![image](https://hackmd.io/_uploads/ByRoLImeWg.png)
    - 1. Cung cấp nhanh các shortcuts để thực hiện với account hoặc session đăng nhập, như thực hiện thay đổi với user account, khoá screen hay đăng xuất. Các shortcuts cụ thể khác cho account là Document folder và Pictures folder. Cuối cùng icon bánh răng đưa tới Settting screen và power icon cho phép ngắt kết nối Remote Desktop session, shut down hay restart máy.
    ![image](https://hackmd.io/_uploads/By0av8Qebe.png)
    - 2. Show tất cả `Recently added` apps/programs tại phần top và các apps/programs đã cài đặt (xuất hiện trong Star Menu) được sắp xếp theo Alphabet. Phần bo:
    ![image](https://hackmd.io/_uploads/Bk7E_LXebg.png)
    - 3. Phần cạnh phải là nơi tìm các icons cho các apps/programs cụ thể, một số ô được mặc định added tới phần này. Nếu nhấn chuột phải vào bất kì ô nào, một menu sẽ xuất hiện và cho nhiều options như thay đổi kích thước ô, unpinning khỏi Start Menu, xem Properties của nó:
    ![image](https://hackmd.io/_uploads/SkmLKLXxWe.png)
    Các apps/programs có thể được add tới Start Menu bằng cách nhấp chuột phải vào app/program và chọn `Pin to Start`:
    ![image](https://hackmd.io/_uploads/rJy2K87lbe.png)
##### Taskbar:
- Một số thành phần mặc định là khả dụng và có thể thấy được. Nếu muốn tắt một số thành phần, có thể nhấp chuột phải trên Taskbar, một context menu sẽ hiện ra và cho phép thực hiện thay đổi.
![image](https://hackmd.io/_uploads/SJ4a68meWx.png)
- Bất kì apps/programs, folders, files được mở hoặc chạy đều xuất hiện trên Taskbar:
![image](https://hackmd.io/_uploads/HkG7RU7lbg.png)
- Di chuột tới icon giúp xem bản thu nhỏ và chú giải (tooltip). Phần tooltip này hữu ích khi ta có nhiều apps/programs đang mở, như Google Chrome và ta muốn tìm phần nào của Chrome là phần cần chú ý vào.
- Khi đóng bất kỳ các items này, nó biến mất khỏi taskbar (trừ khi pin nó).
##### Notification Area:
- Nằm ở góc phải của màn hình Windows, là nơi hiển thị thời gian. Các icons khác hiển thị trong phần này là volume icon, network/wireless icon,... Icons có thể được added hoặc loại bỏ khỏi Notification Area trong Taskbar Settings.
![image](https://hackmd.io/_uploads/S1hZWvQlbg.png)
- Từ phần này, cuộn xuống Notification Area để thực hiện thay đổi:
![image](https://hackmd.io/_uploads/rku7WvXeZg.png)
> - Tài liệu tóm tắt cho [Start Menu](https://support.microsoft.com/en-us/windows/customize-the-windows-start-menu-fde6f576-0fc0-0813-6b0d-d3ec1d244c50) và [Notification Area](https://support.microsoft.com/en-us/windows/customize-the-taskbar-in-windows-0657a50f-0cc7-dbfd-ae6b-05020b195b07#WindowsVersion=Windows_10&windowsversion=windows_10).
> - Có thể nhấn chuột phải vào bất kì folder, file, app/program hoặc icon để xem nhiều thông tin hoặc hiển thị các hành động khác.

#### b) The Windows\System32 Folder:
- Folder `C:\Windows` thường được biết như folder chứa hệ thống vận hành Windows, nó không cần thiết nằm ở ổ C mà có thể nằm ở ổ khác. Đây là nơi chứa các môi trường, hệ thống môi trường có giá trị (`%windir%`).
- Theo Microsoft: *"Môi trường có giá trị chứa thông tin về môi trường vận hành, gồm những chi tiết như operating system path, số nhân dùng bởi hệ thống vận hành, vị trí các folder tạm thời"*.
![image](https://hackmd.io/_uploads/H13_8P4xWx.png)
- Một trong những folders nằm trong `Windows` là `System32`, nơi chứa những files quan trọng với hệ thống vận hành:
![image](https://hackmd.io/_uploads/HkI2Iw4lbg.png)
- Nên làm việc cẩn thận với folder này, việc [xoá](https://www.howtogeek.com/346997/what-is-the-system32-directory-and-why-you-shouldnt-delete-it/) ngẫu nhiên bất kì files hay folders trong nó đều có thể khiến Windows OS không hoạt động.

#### c) User Account, Profiles và Quyền:
- User accounts có thể là một trong hai loại trên local Windows system điển hình: `Administrator` và `Standard User`. Hai loại trên quyết định hành động nào user có thể thực hiện trên hệ thống Windows cụ thể:
    - `Administrator`: Có thể thực hiện thay đổi với hệ thống: add/xoá users, sửa đổi nhóm, cài đặt hệ thống,...
    - `Stardar User`: Có thể thực hiện thay đổi với folders/files gán với người dùng và không thể thay đổi hệ thống, như install programs.
- Có thể đăng nhập như một Administrator, có một vài cách quyết định user account nào tồn tại trên hệ thống:
    - Click `Start Menu` và `Other User`, phần shortcut tới `System Settings` > `Other users` hiện ra:
    ![image](https://hackmd.io/_uploads/S1jwjwEe-e.png)
    - Nếu click vào nó, cửa sổ Settings hiện ra. Khi là Administrator, ta thấy `Add someone else to this PC`:
    ![image](https://hackmd.io/_uploads/ry_3jvNeZg.png)
    - Click vào local user account, hai option xuất hiện là `Change account type` và `Remove`:
    ![image](https://hackmd.io/_uploads/H1E_3vElZg.png)
    - Chọn `Change account type`, giá trị trong box thả xuống (hoặc giá trị được highlight nếu click vào) là account hiện tại.
- Khi một user account được tạo thì một profile tương ứng được tạo khi đăng nhập lần đầu, vị trí nằm trong `C:\Users`. Khi một user account mới đăng nhập lần đầu, họ sẽ thấy vài messages trên màn hình đăng nhập, một trong số đó, User Profile Service, ở trên màn hình đăng nhập một lúc để tạo user profile:
![image](https://hackmd.io/_uploads/H1y66wNxZl.png)
- Một khi đăng nhập vào, user sẽ thấy một dialog box cho biết profile đang được tạo:
![image](https://hackmd.io/_uploads/rkgqJ_NeZe.png)
- Mỗi user profile sẽ có các folders giống nhau (`Desktop`, `Documents`, `Downloads`, `Music`, `Pictures`,...). Cách khác để truy cập thông tin này và một số khác là dùng `Local User and Group Management`. Chuột phải vào Start Menu và chọn `Run` (để mở các items nhanh chóng), gõ `lusrmgr.msc`, ta thấy hai folders: `Users`, `Groups`. Nếu chọn `Groups`, ta thấy tất cả tên của local groups và mô tả của từng cái. Mỗi groups có các quyền set cho nó và users được chỉ định/add bởi Administrator. Khi một user được chỉ định tới một group, user thường hưởng quyền của group đó. Một user có thể được chỉ định tới nhiều groups. 
- Nếu chọn `Add someone else to this PC` từ `Other users`, nó mở `Local Users and Management`.

#### d) User Account Control:
- Phần lớn home users đăng nhập vào hệ thống Windows như local administrators. Một user không cần chạy với các đặc quyền cao trên hệ thống để chạy các tasks không cần tới những quyền đó. Các đặc quyền này làm tăng rủi ro việc xâm phạm hệ thống vì nó dễ dàng khiến mã độc xâm nhập. Hậu quả là, hi user account có thể thực hiện thay đổi trong hệ thống, mã độc có thể chạy trong context của user đăng nhập vào.
- Để bảo vệ local user với các đặc quyền, Microsoft giới thiệu User Account Control (UAC). UAC mặc định không apply vào local administrator account tích hợp sẵn.
- Khi một user với administrator account đăng nhập vào hệ thống, session hiện tại sẽ không chạy với quyền cao. Khi một quá trình yêu cầu quyền cao hơn để chạy, user sẽ được yêu cầu confirm nếu họ cho phép quá trình đó chạy.
- Với chương trình trên account đang đăng nhập vào, chuột phải để xem `Properties` và `Security` tab của administrator account tích hợp sẵn, ta thấy users/groups và quyền của họ với file này, user tiêu chuẩn sẽ không được liệt kê:
![image](https://hackmd.io/_uploads/SJcKfGLxWx.png)
Đăng nhập như một user tiêu chuẩn (username và password xem trong `lusrmgr.msc`) và thử install chương trình này, ta thấy icon khiên trên icon chương trình mặc định, cho thấy UAC yêu cầu cho phép quyền cao hơn để install chương trình:
![image](https://hackmd.io/_uploads/B1xjNGIxWg.png)
Double-click vào chương trình, ta thấy UAC prompt, để ý rằng administrator account tích hợp sẵn được set như username và yêu cầu mật khẩu account:
![image](https://hackmd.io/_uploads/HJ0fSfUx-x.png)
Nếu mật khẩu không được nhập, UAC prompt biến mất, chương trình không được install. Đặc điểm này giảm khả năng mã độc xâm nhập thành công vào hệ thống.
- UAC settings có thể thay đổi hoặc tắt hoàn toàn (không recommended). Có thể di chuyển slider để xem cách setting thay đổi UAC settings và lập trường của Microsoft:
![image](https://hackmd.io/_uploads/SJG4MX8g-g.png)
- Sâu hơn về UAC ở [How User Account Control works](https://learn.microsoft.com/en-us/windows/security/application-security/application-control/user-account-control/how-it-works).

#### e) Settings and the Control Panel:
- Trong hệ thống Windows, các vị trí chính để thực hiện thay đổi là Settings menu và Control Panel. Thực tế là hiện tại Settings menu là vị trí chính để user tìm cách thay đổi hệ thống. Có một số sự tương đồng và khác biệt giữa hai menus:
    - Settings (Icons có thể khác nhau tuỳ phiên bản Windows):
    ![image](https://hackmd.io/_uploads/Hk0q_fUe-g.png)
    - Control Panel:
    ![image](https://hackmd.io/_uploads/BkBodzLeZe.png)
- Cả hai đều có thể truy cập từ Start Menu:
![image](https://hackmd.io/_uploads/rJyyKzUgbe.png)
- Control Panel là nơi truy cập nhiều settings và thực hiện các hành động phức tạp. Trong một số trường hợp, ta có thể bắt đầu ở Settings và kết thúc ở Control Panel. Nếu ta không biết mở cái nào để thay đổi một cài đặt, search ở Start menu.

#### f) Task Manager:
- Cung cấp thông tin về applications và quá trình đang chạy trên hệ thống, một số thông tin khác như CPU và RAM đang dùng,...
- Chuột phải vào Taskbar để truy cập Task Manager:
![image](https://hackmd.io/_uploads/By5fjzUlZe.png)
![image](https://hackmd.io/_uploads/HJXy2fLg-e.png)
- Thông tin sâu hơn về Task Manager: [Windows Task Manager: The Complete Guide](https://www.howtogeek.com/405806/windows-task-manager-the-complete-guide/)

#### g) System Configuration:
- `MSConfig` để xử lý sự cố nâng cao, và mục đích chính là chẩn đoán các sự cố lúc khởi động. Thông tin sâu hơn ở: [link](https://learn.microsoft.com/en-us/troubleshoot/windows-client/performance/system-configuration-utility-troubleshoot-configuration-errors).
- Có vài cách để mở System Configuration, một trong số đó là Start Menu (cần quyền administrator):
![image](https://hackmd.io/_uploads/SJyvPSDxZe.png)
- Có năm tabs ở phần top:
    - `General`: Có thể chọn devices và services nào cho Windows để load khi khởi động. Các options là `Normal`, `Diagnostic`, `Selective`.
    ![image](https://hackmd.io/_uploads/BJ52vrwxWe.png)
    - `Boot`: Có thể định rõ các boot options khác nhau cho hệ thống vận hành:
    ![image](https://hackmd.io/_uploads/r1dH_Hwe-l.png)
    - `Services`: Liệt kê tất cả services được cấu hình cho hệ thống bất kể trạng thái của nó (đang chạy/tắt). Một service là một loại app đặc biệt chạy ngầm:
    ![image](https://hackmd.io/_uploads/ryBnurPlZe.png)
    - `Startup`: Microsoft khuyên dùng Task Manager (`taskmgr`) để quản lý các items khởi động.
    ![image](https://hackmd.io/_uploads/SkWRuSDebg.png)
    - `Tools`: Danh sách các tools khác nhau có thể chạy để cấu hình nhiều hệ thống vận hành. Có một bản mô tả tóm tắt của mỗi tool. Phần thông tin trong `Selected command` sẽ thay đổi mỗi tool. Để chạy tool, có thể dùng command qua run prompt, command prompt hay click phím `Launch`:
    ![image](https://hackmd.io/_uploads/rkTwtSPgbe.png)

#### h) Computer Management:
![image](https://hackmd.io/_uploads/r1MVtUvgZl.png)
`compmgmt` có ba phần chính:
##### `System Tools`:
- `Task Scheduler`: Có thể tạo và quản lý các tasks chung mà máy thực hiện tự động tại thời điểm ta chỉ định. Một task có thể chạy một app, script,... và tasks có thể được cấu hình để chạy ở bất kì chỗ nào. Nó có thể chạy khi đăng nhập hoặc đăng xuất. Tasks cũng có thể được cấu hình để chạy với một lịch trình cụ thể. Để tạo một task đơn giản, chọn `Create Basic Task` dưới phần `Actions` (ô phải):
![image](https://hackmd.io/_uploads/r1Is5UPlZg.png)
- `Event Viewer`: Cho phép xem events xảy ra trên máy. Các records của events có thể xem như dấu vết kiểm tra để hiểu hoạt động của hệ thống máy. Thông tin này thường dùng để đoán các vấn đề và điều tra các hành động thực thi trên hệ thống. `Event Viewer` có ba ô:
    - Ô trái là cây danh sách các event log.
    - Ô giữa hiển thị phần overview chung mà tổng hợp các events cụ thể của provider đã chọn.
    - Ô phải là ô actions.
![image](https://hackmd.io/_uploads/rkRVsIPg-g.png)
Năm [loại events](https://learn.microsoft.com/en-us/windows/win32/eventlog/event-types) có thể ghi log:
![image](https://hackmd.io/_uploads/SJ3S3IwgWx.png)
Các logs tiêu chuẩn hiển thị dưới Windows Logs. Mô tả tóm tắt trong [tài liệu](https://learn.microsoft.com/en-us/windows/win32/eventlog/eventlog-key):
![image](https://hackmd.io/_uploads/SJA2nIwgbl.png)
- `Shared Folders`: Nơi hiển thị danh sách của shares và folders được shared mà người khác có thể kết nối. Dưới phần `Shares` là các share mặc định của Windows, `C$` và các remote administration shares mặc định tạo bởi Windows. Với bất kỳ object trong Windows, chuột phải vào folder để xem properties.
![image](https://hackmd.io/_uploads/Hyjm6Uwx-e.png)
Dưới `Sessions` là danh sách users thường kết nối tới shares. Tất cả folders, files kết nối truy cập users được liệt kê dưới `Open Files`.
- `Local Users and Groups` là `lusrmgr.msc`.
- `Performance`: Ta thấy Performance Monitor (`perfmon`), dùng để hiển thị data trong thời gian thực hay từ log-file, hữu ích trong việc thực hiện xử lý sự cố trên hệ thống máy local hay remote.
![image](https://hackmd.io/_uploads/SkUPzwPgWl.png)
- `Device Manager` cho phép xem và cấu hình phần cứng:
![image](https://hackmd.io/_uploads/H199fwDgWg.png)
##### `Storage`:
- Dưới `Storage` là `Windows Server Backup` và `Disk Management`. `Disk Management` là hệ thống hữu ích trong Windows cho phép thực hiện các tasks lưu trữ nâng cao như thiết lập drive mới, mở rộng hay thu hẹp phân vùng, chỉ đinh hay thay đổi ký tự ổ đĩa,...
##### `Services and Applications`:
![image](https://hackmd.io/_uploads/SJxi4vvgWl.png)
- Service là một loại app đặc biệt chạy ngầm, ta có thể làm nhiều cái hơn việc bật/tắt service, như xem Properties:
![image](https://hackmd.io/_uploads/SyPgHwwlWl.png)
- Windows Management Instrumentation (WMI) Control cấu hình và điều khiển WMI service. Theo Wikipedia: *"WMI cho phép ngôn ngữ scripting quản lý máy Microsoft Windows cá nhân và servers, theo cả local và remote. Micosoft cũng cung cấp giao diện command-line cho WMI gọi là Windows Management Instrumentation Command-line (WMIC)."*

#### i) System Information:
- Theo Microsoft: *"Windows có một tool là Microsoft System Information (`msinfo32.exe`). Tool này thu thập thông tin về máy tính và hiển thị cái nhìn toàn diện về phần cứng, thành phần hệ thống, môi trường phần mềm mà ta dùng để chẩn đoán vấn đề máy."*
- System Summary hiển thị thông số kỹ thuật chung của máy:
![image](https://hackmd.io/_uploads/rJMW72KxZe.png)
    - Thông tin hiển thị trong `Hardware Resources` không phải cho user trung bình, có thể tìm hiểu thêm ở [Hardware resources](https://learn.microsoft.com/en-us/windows-hardware/drivers/kernel/hardware-resources).
    ![image](https://hackmd.io/_uploads/rktqX3tx-x.png)
    - Dưới `Components`, có thể thấy thông tin về thiết bị phần cứng được cài trên máy. Một số phần không hiển thị thông tin, một số thì có:
    ![image](https://hackmd.io/_uploads/HkPA7hFgbl.png)
    - Trong `Software Environtment`, có thể thấy thông tin về phần mềm tích hợp trong hệ thống vận hành, phần mềm đã tải. Các chi tiết khác có thể thấy trong phần này:
    ![image](https://hackmd.io/_uploads/rkkU42Ygbx.png)
- Theo Windows: *"Environment variables chứa thông tin về môi trường hệ thống vận hành. Thông tin này gồm các chi tiết như path hệ thống vận hành, số nhân dùng bởi hệ thống vận hành và vị trí thư mục ngắn hạn.
Environment variables chứa dữ liệu dùng bởi hệ thống vận hành và programs khác. Ví dụ, môi trường biến đổi WINDIR chứa vị trí thư mục cài đặt Windows. Programs có thể truy vấn giá trị của biến đổi để quyết định vị trí của file hệ thống vận hành Windows."*
- Click `Environment Variables` để xem giá trị được gán cho máy ảo:
![image](https://hackmd.io/_uploads/S1oAS3tl-e.png)
- Cách khác để xem: `Control Panel`>`System and Security`>`System`>`Advanced system settings`>`Environment Variables` hay `Settings`>`System`>`About`>`system info`>`Advance system settings`>`Environment Varibles`:
![image](https://hackmd.io/_uploads/r11982FlZx.png)
- Quay lại với `msinfo32`, ở dưới tiện ích này có một search bar, chọn `Components` và seachr `IP address`:
![image](https://hackmd.io/_uploads/rkzlP2KxWl.png)

#### k) Resource Monitor:
- Theo Microsoft: *"Resource Monitor (`resmon`) hiển thị CPU theo quy trình và tổng hợp, bộ nhớ, ổ đĩa và thoong tin network đã dùng, ngoài việc cung cấp chi tiết về quy trình nào đang dùng file và modules riêng lẻ. Bộ lọc nâng cao cho phép users tách dữ liệu liên quan đến một hay nhiều quá trình (apps hay services), start, stop, pause và resume services, và đóng apps không phản hồi từ giao diện user. Nó cũng chứa tính năng phân tích quy trình giúp nhận diện quá trình đình trệ và file xung đột khoá để user có thể giải quyết xung đột thay vì đóng app và mất data."*. Tiện ích này chủ yếu hướng đến users nâng cao, người cần khắc phục sự cố nâng cao trên hệ thống máy.
- Trong tab `Overview`:
![image](https://hackmd.io/_uploads/B1KvRntgZx.png)
![image](https://hackmd.io/_uploads/ryJ9R2tlZx.png)
    - CPU:
    ![image](https://hackmd.io/_uploads/HkRqA3Kg-g.png)
    - Disk:
    ![image](https://hackmd.io/_uploads/ByzTRhFx-g.png)
    - Network:
    ![image](https://hackmd.io/_uploads/H15pCntlbe.png)
    - Memory:
    ![image](https://hackmd.io/_uploads/Byj2CnFgbe.png)
- Resource Monitor có một ô ở phía bên phải, hiển thị góc nhìn đồ hoạ trong thời gian thực cho mỗi phần.

### 19. Một số tools để thu thập dữ liệu:
- Để thực hiện phân tích forensics, đôi khi phải phân tích một hệ thống đang hoạt động hoặc bức ảnh chụp lại hệ thống đó. Để chính xác hơn, ta thường được khuyến nghị làm việc với bản sao của phần dữ liệu cần phân tích.
> Ví dụ khi phân tích dữ liệu registry từ một hệ thống đang hoạt động hoặc một disk image, có thể xem nó với Registry Editor, và phương pháp phân tích chính xác là thực hiện phân tích phần dữ liệu đã được copy. Tuy nhiên, ta không thể copy registry hives từ `%WINDIR%\System32\Config` vì nó là file bị hạn chế.
- Để làm việc và xem các files như ví dụ trên, có thể dùng tools:

#### a) KAPE:
Là công cụ thu thập và phân tích dữ liệu trực tiếp, chủ yếu dùng command nhưng được hiển thị với GUI. Ví dụ đã thiết lập tất cả cài đặt để trích xuất dữ liệu:
![image](https://hackmd.io/_uploads/r1GwkbH0ge.png)

#### b) Autospy:
Cho hai option để thu thập dữ liệu từ hệ thống trực tiếp hoặc disk image. Sau khi add source dữ liệu vào, điều hướng đến vị trí của các files muốn extract, sau đó click chuột phải và chọn `Extract File(s)`. Ví dụ:
![image](https://hackmd.io/_uploads/ryJ-xbrAex.png)

#### c) FTK Imager: 
- Tương tự Autospy và cho phép extract files từ disk image hoặc hệ thống trực tiếp bằng cách gắn disk image hoặc drive vào FTK Imager. Ví dụ xuất các tệp bôi đậm:
![image](https://hackmd.io/_uploads/rJY9lbHRee.png)
- Cách khác để extract Registry files từ FTK Imager là `Obtain Protected Files`. Option này cho phép extract tất cả registry hives đến được vị trí đã chọn. Tuy nhiên, nó không copy `Amcache.hve` vì file này thường rất cần cho việc điều tra bằng chứng của chương trình vừa thay đổi mới nhất. Option này hợp lệ cho các hệ thống trực tiếp và nó được highlight ở ảnh dưới:
![image](https://hackmd.io/_uploads/SJf5b-rRgx.png)

#### d) Registry Viewer:
AcessData's Registry Viewer có sự tương đồng với Windows Registry Editor, tuy nhiên hạn chế là load duy nhất một hive tại một thời điểm và không tính đến transaction logs:
![image](https://hackmd.io/_uploads/HJvH7WH0gg.png)

#### e) Zimmerman's Registry Explorer:
- Eric Zimmerman viết rất nhiều tools hữu dụng cho Digital Forensics và phản hồi sự cố. Một trong số đó là Registry Explorer.
- Registry Explorer có thể load nhiều hives cùng một lúc và add dữ liệu từ transaction logs vào hive để tạo một cleaner hive với nhiều dữ liệu cập nhật mới. Nó có option `Bookmarks` tiện dụng gồm các registry keys thường được các nhà điều tra forensics tìm kiếm. Các nhà điều tra có thể đi thẳng tới các registry keys và giá trị với các bookmarks menu item.
![image](https://hackmd.io/_uploads/B1yaNZBCeg.png)

#### f) RegRipper:
- Công cụ hữu ích để xuất báo cáo input và output về các dữ liệu được trích xuất từ keys và giá trị trong registry hives. Báo cáo output là một text file và cho thấy tất cả kết quả theo thứ tự.
- RegRipper hữu dụng trong cả CLI và GUI:
![image](https://hackmd.io/_uploads/SydsBZSAeg.png)
- Một hạn chế là nó không dùng trong transaction logs. Ta dùng Registry Explorer để làm nổi các transaction logs tương ứng với registry hives trước khi gửi output đến RegRipper để kết quả chính xác hơn.
