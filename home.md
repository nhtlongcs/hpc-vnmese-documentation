# **Guide to the HCMUS Research Computing Clusters**

Core HPC (High Performance Computing Core) được sử dụng để chạy nhiều cluster trong ĐH-KHTN. Hệ thống lập lịch - job scheduler mà chúng tôi sử dụng trên các cụm này được gọi là **Slurm**. Hướng dẫn này đã được chuẩn bị để giúp mọi người sử dụng hệ thống một cách hiệu quả. Mỗi trang tập hợp các liên kết đến các tài nguyên cơ bản trải rộng trên trang web này và giới thiệu chúng theo thứ tự thích hợp.

Tất cả người dùng hệ thống đều phải có kiến thức làm việc cơ bản về hệ thống. Điều này rất quan trọng vì người dùng thể *vô tình lãng phí tài nguyên hoặc thậm chí ảnh hưởng xấu đến công việc của người khác*.

Miễn trừ trách nhiệm: tài liệu này là một bản dịch phù hợp với hệ thống hiện tại của server ĐHKHTN- được dịch từ https://researchcomputing.princeton.edu/ 

Outline hướng dẫn này bao gồm các chủ đề sau:

1. Giới thiệu về cluster (xong)
    1. Mô tả kiến trúc cluster
    2. Làm thế nào để có được một tài khoản (anh Đăng viết)
    3. Cách kết nối với cluster (Phải đọc) (thiếu phần VPN?)
    4. **Cách làm việc với files và bộ nhớ (Phải đọc) (nhờ a Đăng viết)**
2. Software (xong)
    - Cách sử dụng phần mềm đã cài đặt trên cluster (thông qua module) (Phải đọc)
    - **Cách cài đặt phần mềm riêng (cần cài qua apt-install hoặc yêu cầu sudo) -solution1 psi 2 request (nhờ a Lễ xem)**
3. SLURM job (xong) (phải đọc)
    - Giới thiệu về Slurm
    - Hướng dẫn từng bước để chạy tác vụ Slurm đầu tiên
4. Tối ưu - các best practice (chưa xong)
    - Tối ưu hoá sử dụng tài nguyên trên Slurm
    - Sử dụng hiệu quả filesystem
5. Liên hệ nhờ giúp đỡ
    - FAQ (Các câu hỏi thường gặp)
    - Các bước hỗ trợ
6. Các thuật ngữ nên đọc 
