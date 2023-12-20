# Kiến thức cơ bản về cluster

[Cluster là gì](./definition.md)

Để có thêm kiến thức về hệ thống, cụ thể hơn là hệ thống được thiết lập tại HCMUS, vui lòng xem kĩ thông tin về cluster.

### Những lưu ý quan trọng khi sử dụng cụm HPC của HCMUS

**Quy tắc 10-10** Trước tiên, điều quan trọng cần biết là bạn có thể chạy các tác vụ thử nghiệm trên các nút chính chạy tối đa 10 phút và sử dụng tới 10% số lõi CPU và bộ nhớ. Bạn có thể sẽ làm gián đoạn công việc của người khác nếu bạn vượt quá những giới hạn này và bạn có thể bị quản trị viên hệ thống của chúng tôi cảnh cáo nếu bạn vượt quá những quy tắc này.

# Kết nối đến cluster

Trước khi có thể kết nối, hãy đảm bảo bạn đã thực hiện các bước cần thiết để có tài khoản. 

[Hướng dẫn đăng ký tài khoản](./registration.md)

## Kết nối thông qua SSH

Để kết nối với các cụm máy tính của trường đại học, bạn sẽ cần SSH, một phần mềm để thiết lập kết nối an toàn với các máy từ xa.

Trên MacOS và Linux, ứng dụng Terminal mặc định được tích hợp sẵn ứng dụng khách như vậy. 

Trên các máy Windows 10 cũng có ứng dụng hỗ trợ SSH. (Nếu vì lý do nào đó mà bạn không bật SSH trong Windows 10, hãy làm theo hướng dẫn [này](https://www.howtogeek.com/336775/how-to-enable-and-use-windows-10s-built-in-ssh-commands/) để kích hoạt nó).

### Để kết nối với cụm qua SSH trên Linux, macOS hoặc Windows 10:

1. **Truy cập Terminal trên máy tính của bạn**
    - Linux: mở cửa sổ Terminal (thường bằng cách nhấn Ctrl+Alt+t - tức là nhấn và giữ Ctrl và không nhả nó, nhấn và giữ Alt, sau đó không nhả một trong hai phím đó, gõ 't')
    - macOS: mở cửa sổ Terminal (bằng cách khởi chạy ứng dụng Terminal nằm trong /Ứng dụng/Tiện ích)
    - 
2. **SSH vào cluster**
    
    Cú pháp sử dụng ssh giống nhau trong tất cả các trường hợp trên. Hãy nhớ đảm bảo rằng bạn đang sử dụng VPN, sau đó trên dòng lệnh bạn đã truy cập ở bước trước, hãy nhập
    
    `ssh <YourAccountName>@<host> -p <port>` 
    
    Nếu đây là lần đầu tiên bạn kết nối với cụm này từ bất kỳ máy tính nào bạn đang sử dụng, thì bạn sẽ thấy nhận xét về finger print cùng với câu hỏi Trả lời 'có' và nhấn Enter.
    
    Bây giờ bạn sẽ được nhắc nhập mật khẩu thông thường của mình. Nhập nó.
    
    - LƯU Ý*: không có dấu hoa thị hoặc dấu chấm để cho biết bạn đã nhập bao nhiêu ký tự, vì vậy nếu bạn cho rằng mình đã mắc lỗi đánh máy, hãy nhấn Backspace nhiều lần và nhập mật khẩu từ đầu.
    - LƯU Ý*: Nếu trước đây bạn đã kết nối với một cụm và thiết lập khóa SSH, thì bạn sẽ được kết nối mà không bị nhắc nhập mật khẩu trước.
    
    Vậy là xong -- bây giờ bạn sẽ được kết nối với một cụm và xem command-line Linux của cụm đó (ví dụ: [selab1:~ ]$) thay vì command-line cho máy tính của bạn.
    
3. **Kết thúc kết nối SSH tới cụm**
    
    Để đóng kết nối SSH, chỉ cần nhập dòng lệnh và nhấn Enter. Thao tác này sẽ đóng kết nối và dấu nhắc dòng lệnh trên máy tính cục bộ của bạn sẽ xuất hiện lại. (hoặc sử dụng Ctrl-D)
    

## ****SSH Keys: ssh không cần sử dụng password****

Việc nhập mật khẩu mỗi khi bạn muốn kết nối với một máy hoặc khó chịu hơn là mỗi khi bạn muốn sao chép một tập tin đến/từ một máy từ xa sẽ nhanh chóng trở nên khó chịu. Một giải pháp là cho phép đăng nhập/hoạt động từ xa không cần mật khẩu bằng cách tạo một cặp khóa ssh công khai/riêng tư và sử dụng chúng để kết nối. Quy trình được giải thích trong hướng dẫn này

[Đăng nhập SSH không cần mật khẩu](./ssh.md)

## Sử dụng Tmux để duy trì kết nối

Nếu kết nối SSH của bạn đột ngột bị hỏng thì lệnh bạn đang chạy sẽ chấm dứt. Điều này xuất hiện rất thường xuyên, nơi các tác vụ được chạy trực tiếp từ dòng lệnh chứ không phải từ bộ lập lịch công việc.

Một giải pháp cho vấn đề này là `tmux`. Nó được cài đặt trên tất cả các cụm của trường đại học và cho phép bạn bắt đầu một phiên shell, thay vì điều khiển từ xa thông qua SSH, nó tồn tại trên máy chủ.

Một trường hợp sử dụng đơn giản sẽ là:

```
$ ssh <YourNetID>@<hostname>
$ tmux new -s download
$ wget https://www.bigdata.org/dataset.tar.gz
# ssh connection suddenly breaks!
# no problem just reconnect and attach
$ ssh <YourNetID>@<hostname>
$ tmux a -t download
```

Để thoát khỏi phiên tmux của bạn (mà không kết thúc nó), hãy nhấn ctrl-b, sau đó nhấn d. Để đóng phiên tmux, hãy chạy lệnh "exit" hoặc nhấn 'ctrl+d.' Phiên tmux sẽ chạy trên máy chủ từ xa cho đến khi máy chủ được khởi động lại hoặc bạn đóng chúng. Bất cứ điều gì bạn chạy trong khi gắn vào phiên tmux sẽ chạy trong phiên đó và do đó sẽ an toàn khi bị ngắt kết nối.

tmux là một công cụ mạnh mẽ và phức tạp. Ngoài hướng dẫn đơn giản được liên kết ở trên, bạn có thể đọc cheatsheet về tmux

## **Tìm hiểu thêm về một cụm bằng cách chạy lệnh**

Khi ở trong một cụm, hãy nhập từng lệnh bên dưới và kiểm tra đầu ra:

```
 hostname                  # get the name of the machine you are on
 whoami                    # get username of the account
 date                      # get the current date and time
 pwd                       # print working directory
 cat /etc/os-release       # info about operating system
 lscpu                     # info about the CPUs on head node
 sinfo                     # info about the compute nodes 
 squeue                    # which jobs are running or waiting to run
 qos                       # quality of service (job partitions and limits)
 slurmtop                  # shows a map of cluster usage
 who                       # list users on the head node
 nvidia-smi                # check gpu resources
```

# L**àm việc và quản lý bộ nhớ trên cluster**

Các cluster của Research Computing chứa một số thư mục (/home, /scratch và /media hoặc /projects) được thiết kế cho các mục đích sử dụng cụ thể. Để hiểu cách làm việc và lưu trữ đúng cách các loại tệp khác nhau. 

Để truyền tệp vào hoặc ra khỏi cụm, chúng tôi thường khuyên dùng lệnh 'scp'. Cấu trúc của lệnh như sau

```jsx
scp [options] -P [port] [netid]@source-host:file/location [netid@]destination-host:file/location
```

Tham khảo thêm tại [Data Storage | Princeton Research Computing](https://researchcomputing.princeton.edu/support/knowledge-base/data-storage) để viết thêm