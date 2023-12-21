# FAQ (Các câu hỏi thường gặp)

## **Một số lời nhắc với bạn khi sử dụng Slurm**

- Đảm bảo rằng kịch bản Slurm của bạn tải bất kỳ phụ thuộc hoặc thay đổi đường dẫn nào (ví dụ, bạn cần python3, vì vậy **module load anaconda3/2022.5**). Xem thêm ở mục Software
- Đảm bảo bạn gọi tệp thực thi của mình với đường dẫn đầy đủ hoặc **cd** đến thư mục thích hợp.
- Nếu bạn gọi trực tiếp tệp thực thi mà không thông qua trình thông dịch như srun hoặc python hoặc Rscript, v.v., hãy đảm bảo bạn có quyền +x trên nó.
- Xem xét về hệ thống quản lý tệp. Các hệ thống khác nhau hữu ích cho các mục khác nhau, có kích thước khác nhau và chúng không giao tiếp với nhau (trừ các ổ dữ liệu). Xem thêm về các best practice được đề cập trong tài liệu

## **Tôi không biết trạng thái của công việc của mình, nó đang chạy hay đã hoàn thành?**
Kiểm tra trạng thái công việc bằng lệnh squeue. Bạn có thể thêm -u <username> để lọc công việc của một người dùng cụ thể. Bạn có thể thêm -j <job-id> để lọc công việc của một ID công việc cụ thể. Bạn có thể thêm -t <state> để lọc công việc của một trạng thái cụ thể. Ví dụ: squeue -u <username> -j <job-id> -t <state>.

## **Công việc của tôi đang PENDING quá lâu, tại sao và tôi nên làm gì?**
Nếu công việc đang đợi, kiểm tra nguyên nhân bằng lệnh scontrol show job <job-id>, có thể yêu cầu tài nguyên không khả dụng nên công việc đang chờ đợi. Để giải quyết điều này, bạn có thể (1) đợi cho tài nguyên trở nên khả dụng hoặc (2) thay đổi tài nguyên yêu cầu bằng cách sửa đổi kịch bản công việc.

```bash
srun --pty bash
srun --pty --gres=gpu:1 --cpus-per-task=8 --mem-per-cpu=3G bash

```

## **Làm thế nào để kiểm tra lượng tài nguyên khả dụng?**
Kiểm tra lượng bộ nhớ RAM và lõi CPU khả dụng bằng lệnh sinfo -o "%20P %20m %20c".

## **Công việc của tôi đang chạy quá lâu, tại sao và tôi nên làm gì?**
Kiểm tra đầu ra bằng lệnh scontrol show job <job-id>, bạn có thể xem nút mà công việc đang chạy trên đó. Sau đó, bạn có thể ssh vào nút và kiểm tra trạng thái công việc. Nếu công việc đang chạy, bạn có thể kiểm tra việc sử dụng tài nguyên bằng lệnh top hoặc htop.

## **Công việc của tôi đã hoàn thành, làm thế nào để kiểm tra đầu ra?**
Kiểm tra đầu ra bằng lệnh cat <job-id>.out.

## **Tôi muốn sử dụng một gói không được cài đặt trên cụm máy, làm thế nào?**
Có hai cách để cài đặt gói:

1. Sử dụng module để tải gói. Nếu gói đã được cài đặt, bạn có thể tải nó bằng lệnh module load <package-name>. Nếu không, bạn có thể cài đặt nó bằng lệnh module install <package-name>. Nếu bạn không có quyền, hãy liên lạc với quản trị viên.

2. Lỗi "module: command not found" khi sử dụng lệnh module. Lỗi này thường xuyên xuất hiện khi đường dẫn module không được thiết lập. Hành vi này thường xảy ra khi bạn sử dụng vscode remote ssh để kết nối với cụm máy. Terminal vscode sẽ không tải đường dẫn module. Giải pháp tạm thời là sử dụng tmux để tạo một terminal mới. Terminal mới này sẽ có đường dẫn module được thiết lập.

## **Tại sao Job của tôi không được chạy?** (WIP)

Thời gian bắt đầu công việc xác định bởi **[job priority](https://researchcomputing.princeton.edu/support/knowledge-base/job-priority)**.