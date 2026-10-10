# Xây dựng 1 hệ thống với sự trợ giúp của AI Agent
## Chuẩn bị các đặc tả bài toán
 - Chưa viết code ngay mà tạo file đặc tả yêu cầu (trong thư mục `/doc` của dự án)
## Lập quy tắc cho AI
 - Thiết lập file cấu hình có tên `.codyrules`: định nghĩa framework, chuẩn đặt tên, các quy tắc..
## Viết các promt để xây dựng
 - Nhờ AI tạo khung dự án theo các công nghệ sử dụng trong `/doc`
 - Yêu cầu AI xây dựng các UseCase cụ thể theo các đặc tả chi tiết trong doc
 - Dùng AI để review code

# Kiểm soát code
## Kiểm soát tính năng
 - khi thay đổi 1 chức năng nào đó nên sửa cả backend lẫn frontend để đảm bảo an toàn dữ liệu và validate form nhanh
 - các chức năng ở backend và frontend thì sửa ở `/services` trong thư mục `/src` của các phần
## Các skill đã dùng
 - file rule: định nghĩa luật lập trình toàn cục cho AI
 - Codebase Indexing $ Context Mentions: trỏ vào bối cảnh trogn dự án thay vì copy/paste
 - UI-to-code: chuyển đổi giao diện mẫu sang các component
 - Database migration & seeding: tạo dữ liệu mẫu để dễ phát triển các module song song
## Các công cụ giúp lead quản lý và tiêu chí làm việc hiệu quả
### Công cụ
 - Diff(git diff): dùng để so sánh sự khác biệt giữa các phiên bản code(file mới và cũ, giữa các nhánh)
 - Log(git log): xem lịch sử commit(ai làm gì, lúc nào, nội dung gì)
### Tiêu chí đánh giá:
 - code có cấu trúc rõ ràng
 - tuân thủ chuẩn chung
 - commit thường xuyên
 - tinh thần phối hợp & tích hợp
 - khả năng tự quản lý & fix bug(tỉ lệ bug sau khi merge thấp)