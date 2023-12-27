---
order: -800
---
# **Quy định về sử dụng tài nguyên**

- Toàn bộ source code và dữ liệu cần được lưu ở đường dẫn `/media/{username}`
- Tuyệt đối không lưu dữ liệu ở `/home/{username}` do không có nhiều dung lượng lưu trữ tại đây.
- **Lưu ý:** đối với thư mục ẩn (ví dụ như `/home/{username}/.cache`) cũng phải được chuyển qua `/media/{username}`.

# **Quy định về quản lý tác vụ**

**Quy tắc 10-10** Trước tiên, điều quan trọng cần biết là bạn có thể chạy các tác vụ thử nghiệm trên các nút chính chạy tối đa 10 phút và sử dụng tới 10% số lõi CPU và bộ nhớ. Bạn có thể sẽ làm gián đoạn công việc của người khác nếu bạn vượt quá những giới hạn này và bạn có thể bị quản trị viên hệ thống của chúng tôi cảnh cáo nếu bạn vượt quá những quy tắc này.