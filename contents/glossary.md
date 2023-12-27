---
label: Bảng thuật ngữ
order: -1000
---

# **Bảng thuật ngữ**

| Cụm từ | Khái niệm |
| --- | --- |
| Bash | Đây là phần mềm shell cho phép người dùng tương tác với hệ điều hành. Đây là giao diện CLI mặc định trên hầu hết các hệ thống Linux. Nó cho phép tương tác CLI và chạy các tập lệnh được lập trình để tự động hóa các lệnh. |
| Cluster | Cluster là một nhóm các máy tính, được kết nối bằng mạng để hoạt động như thể chúng là một, máy tính lớn có nhiều tài nguyên |
| Command line | Lệnh đã nhập bắt đầu một thao tác hoặc chương trình từ dấu nhắc lệnh của hệ điều hành. |
| Compiler | *Một chương trình dịch các chương trình được viết bằng ngôn ngữ cấp cao như C hoặc Fortran sang dạng cấp độ thấp hơn như hợp ngữ. |
| Compute Node | Một máy tính được sử dụng để chạy các tính toán, đôi khi được quản lý bởi bộ lập lịch. |
| Core | Một cách viết tắt để chỉ số lượng lõi xử lý (thường là vật lý) của CPU trong một nút. |
| CPU | Viết tắt của Bộ xử lý trung tâm hay còn gọi là bộ xử lý của máy tính |
| Directory | Một thư mục. |
| Embarrassingly (Pleasingly) Parallel | Một nhóm các hoạt động độc lập với nhau có thể được thực hiện song song mà không bị hạn chế về thứ tự. Nếu có N thao tác song song thú vị, mỗi thao tác chiếm 1 đơn vị thời gian và có N bộ xử lý thì chúng có thể được thực hiện song song trong 1 đơn vị thời gian. |
| Executable | Tệp thực thi là một tệp có thể được nhập vào shell và chạy (thực thi) các lệnh trong tệp. Tệp thực thi có thể là tệp tập lệnh có văn bản có thể đọc được bằng ngôn ngữ dưới dạng được viết hoặc tệp nhị phân được biên dịch bằng chương trình được mã hóa. |
| GPU | Bộ xử lý đồ họa (GPU) là chip máy tính xử lý dữ liệu được hiển thị trên màn hình. Chúng có khả năng xử lý song song các khối dữ liệu lớn, điều này khiến chúng khác với CPU. GPU hiện cũng được sử dụng để tính toán chung các thuật toán có thể xử lý đầu vào song song trên GPU. |
| Head Node | Máy tính chính kết nối một cluster với mạng bên ngoài. Trên các cluster của Khoa CNTT chỉ có nút đầu mới có quyền truy cập internet. |
| Linux | *Một hệ điều hành mã nguồn mở giống Unix. Ví dụ, một giải pháp thay thế cho Windows của Microsoft hoặc OSX của Apple |
| Local machine | Một người thường kết nối với cluster bằng SSH từ máy tính xách tay hoặc máy tính để bàn. Trong trường hợp này, máy tính xách tay hoặc máy tính để bàn là máy cục bộ và nút đăng nhập của cluster là máy từ xa. |
| Memory | Máy tính lưu trữ dữ liệu ở hai nơi: Bộ nhớ truy cập ngẫu nhiên (RAM) và ổ cứng. Việc đọc và ghi vào RAM nhanh hơn nhiều so với một tệp trên ổ cứng nên các chương trình máy tính lưu trữ dữ liệu của chúng trong RAM nhiều nhất có thể. Dung lượng cần thiết để lưu trữ dữ liệu trong RAM được gọi là bộ nhớ. Ví dụ: một chương trình lưu trữ 1 tỷ số dấu phẩy động sẽ cần 8 GB bộ nhớ vì mỗi số cần 8 byte. Bạn sẽ cần yêu cầu một lượng bộ nhớ nhất định trong tập lệnh Slurm của mình khi chạy các tác vụ hàng loạt trên cluster. |
| Module | Các cluster có rất nhiều phần mềm được cài đặt sẵn. Mỗi phần mềm yêu cầu một cấu hình khác nhau nên không thể có sẵn tất cả phần mềm cùng một lúc. Thay vào đó, các module môi trường được sử dụng cho phép người dùng chỉ định phần mềm nào sẽ tải. Bạn có thể xem các module có sẵn bằng cách chạy lệnh "module tận dụng". Để kích hoạt phần mềm cụ thể, hãy xem lệnh "tải module" trên trang module môi trường. |
| Node | Từ đồng nghĩa với máy tính hoặc máy chủ. Trong cluster, một nút là một trong những máy tính độc lập. Mỗi nút trên cluster của Khoa CNTT có ít nhất hai CPU và mỗi CPU bao gồm nhiều lõi. Một công việc nối tiếp sẽ chạy trên một trong các lõi của một trong các CPU của nút. |
| Parallel Job | Công việc song song là công việc có thể sử dụng đồng thời nhiều lõi CPU. Điều này bao gồm các mã đa luồng được viết bằng OpenMP cũng như các công việc đa xử lý được viết bằng giao diện truyền tin nhắn (MPI). Sự khác biệt giữa công việc được tạo trên các quy trình song song và các quy trình đồng thời là các quy trình song song phải chạy đồng thời trong khi quy trình đồng thời thì không (nhưng có thể). |
| RAM | *Là viết tắt của Bộ nhớ truy cập ngẫu nhiên. Bộ nhớ chính trong máy tính. Xem Ký ức ở trên. |
| Remote machine | Trong thế giới điện toán hiệu năng cao (HPC), người ta thường kết nối với nút đăng nhập một cluster bằng SSH từ máy tính xách tay hoặc máy tính để bàn. Trong trường hợp này, nút đăng nhập của cluster là máy từ xa và máy tính xách tay hoặc máy tính để bàn của người dùng là máy cục bộ. |
| Serial Job | Một công việc nối tiếp bao gồm một tập hợp các hướng dẫn phải được thực hiện tuần tự hoặc theo trình tự. Các công việc nối tiếp chỉ có thể sử dụng một lõi CPU. |
| Server | *Máy tính hoặc các máy tính cung cấp quyền truy cập vào dữ liệu theo yêu cầu từ khách hàng |
| Shell | Shell là chương trình cho phép bạn chạy các lệnh trên dòng lệnh hoặc thực thi các lệnh trong tập lệnh. Khi bạn kết nối lần đầu với nút đăng nhập của cluster, bạn sẽ chạm tới shell. Có nhiều loại shell khác nhau như bash, tcsh và zsh. Mỗi shell sử dụng một cú pháp khác nhau. |
| Slurm | Một hệ thống giúp quản lý và sắp xếp công việc trên một Cluster. Viết tắt của “Quản lý tài nguyên người dùng Linux đơn giản”. |
| Source Code | *Một chương trình được viết bằng ngôn ngữ mà người lập trình có thể hiểu được, được biên dịch thành mã ở dạng nhị phân (0 và 1). |
| ssh | Shell bảo mật (ssh) là một giao thức an toàn để chạy các lệnh trên máy từ xa. |
| Task | Nhiệm vụ là một thuật ngữ chung dùng để mô tả một tập hợp công việc tính toán cần thực hiện. Nó thường có nghĩa là quá trình. |
| Thread | Đây là một chuỗi các hướng dẫn được hệ điều hành lên lịch để xử lý. |
| Hardware | Máy tính thực tế và các thành phần liên quan. Các bộ phận có thể chạm vào được. |
| Software | Các chương trình máy tính chạy trên phần cứng. |
| Processor | Thành phần của máy tính thực hiện số học và logic, đồng thời điều khiển phần còn lại của máy tính. |
| Shared memory parallelization | Một chương trình song song trong đó mỗi tiến trình (hoặc luồng) có quyền truy cập vào không gian bộ nhớ dùng chung ngoài bộ nhớ riêng của chúng. |
| Distributed memory parallelization | Khi một chương trình phải chạy nhiều tiến trình không độc lập với nhau; các tiến trình khác nhau cần liên lạc với nhau. |
| Array job | Một công việc chạy nhiều bản sao khác nhau của cùng một mã nhưng có đầu vào khác nhau. |
- copied from Understanding the Digital World by Brian W. Kernighan