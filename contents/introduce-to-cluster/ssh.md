# Đăng nhập SSH không cần mật khẩu

Khi kết nối với cluster  qua `ssh` hoặc truyền tệp qua `scp` , người ta cần nhập mật khẩu. Trang này giải thích cách sử dụng khóa SSH để xử lý bước xác thực để có thể thực hiện đăng nhập và truyền tệp mà không cần nhập mật khẩu.

Như được chỉ ra trong hình bên dưới, bước đầu tiên là tạo các "khóa" riêng tư và công khai trên máy cục bộ của bạn (ví dụ: máy tính xách tay). Những khóa này không gì khác hơn là các tập tin. Sau đó, khóa chung sẽ được thêm vào `~/.ssh/authorized_keys` trên cụm mong muốn trong khi khóa riêng vẫn còn trên máy cục bộ của bạn. Khi bạn cố gắng kết nối với cụm đó, nếu khóa chung và khóa riêng khớp nhau thì bạn sẽ được cấp quyền truy cập mà không cần cung cấp mật khẩu.

## Đối với Linux và Mac

### Bước 1: Tạo cặp khóa riêng/chung

Trên máy cục bộ của bạn (ví dụ: máy tính xách tay), trước tiên hãy tạo cặp khóa RSA. Điều này được thực hiện bằng cách chạy lệnh sau trong một thiết bị đầu cuối (nhấn phím "Enter" 3 lần sau khi chạy lệnh bên dưới, tức là không trả lời bất kỳ câu hỏi nào):

```
$ ssh-keygen -t rsa
 [Enter]
 [Enter]
 [Enter]

```

Đây là một ví dụ:

```
[aturing@mylaptop ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/aturing/.ssh/id_rsa):
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/aturing/.ssh/id_rsa.
Your public key has been saved in /home/aturing/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:WAmli4zRUQGWhIHkT4oXi1CjwNJd3KYKNe5OK8DpcZY aturing@mylaptop
The key's randomart image is:
+---[RSA 2048]----+
|+++==*=+.        |
|=+o+=..oo.       |
|+.o+...oo        |
|o.=*...+         |
|+.=++.o S        |
|.= E+            |
|..+o .           |
| .. o            |
|   .             |
+----[SHA256]-----+

```

Khóa công khai hiện nằm ở `~/.ssh/id_rsa.pub` . Khóa riêng (nhận dạng) nằm ở `~/.ssh/id_rsa` . Khóa riêng tương đương với mật khẩu của bạn nên bạn không bao giờ nên chia sẻ nó. Tuy nhiên, bạn có thể chia sẻ khóa công khai của mình và chúng tôi sẽ thực hiện việc đó tiếp theo.

### Bước 2: Sao chép public key vào cụm HPC

Sử dụng lệnh `ssh-copy-id` để sao chép khóa chung vào cụm mong muốn (nhập mật khẩu của bạn cho cụm HPC khi được nhắc):

```
$ ssh-copy-id <YourNetID>@<cluster-name>
# answer "yes" when asked "Are you sure you want to continue connecting (yes/no)?"

```

Lưu ý rằng lệnh `ssh-copy-id` sẽ chỉ chuyển khóa chung của bạn. Khóa riêng của bạn sẽ vẫn an toàn trên máy cục bộ của bạn trong `~/.ssh/id_rsa` .

Đây là một phiên ví dụ:

```
[aturing@mylaptop ~]$ ssh-copy-id aturing@selab.hcmus.edu.vn
The authenticity of host 'selab.hcmus.edu.vn (128.112.172.210)' can't be established.
ECDSA key fingerprint is SHA256:Hc3x7Tfs3ULz49U2jmpxzOGNwm2p8mkUnZVs8X1X7g8.
ECDSA key fingerprint is MD5:15:47:21:af:6c:ac:5e:e7:88:d5:de:73:d5:ea:c4:9f.
Are you sure you want to continue connecting (yes/no)? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are ...
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to ...
Password:

```

### Bước 3: Kết nối với cụm HPC

Hãy thử `ssh` vào cụm (bạn không cần phải nhập mật khẩu nữa):

```
$ ssh aturing@selab.hcmus.edu.vn

```

Nếu bạn gặp lỗi `Bad owner or permissions on ~/.ssh/config` thì hãy thử thực hiện `chmod 600 ~/.ssh/config` và có thể cả `chown $USER ~/.ssh/config` .

### Bước 4: Quay lại Bước 2 để thêm vào các cụm HPC còn thiếu

## Đối với Windows

### Windows với PowerShell

Hãy làm theo các hướng dẫn sau để tạo khóa nhưng không làm theo quy trình "Sao chép khóa chung một cách an toàn". Nếu khóa chung của bạn nằm trên máy tính xách tay của bạn trong `C:\Users\aturing\.ssh\id_rsa.pub` thì có thể sử dụng thông tin sau để sao chép nó vào cụm HPC mong muốn:

```
$ cat C:\Users\aturing\.ssh\id_rsa.pub | ssh <YourNetID>@<HPC-Cluster> "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"

```

Dưới đây là một ví dụ cụ thể:

```
$ cat C:\Users\aturing\.ssh\id_rsa.pub | ssh aturing@selab.hcmus.edu.vn "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >>  ~/.ssh/authorized_keys"

```

## Kiểm tra

Sau khi thiết lập thông tin đăng nhập không cần mật khẩu, bạn có thể chạy các lệnh trên một cụm mà không cần kết nối chính thức. Ví dụ: lệnh bên dưới sẽ tạo một tệp trống có tên `myfile` trong thư mục `/home` của bạn trên Della:

```
# on your laptop
$ ssh aturing@della "touch myfile"
```
