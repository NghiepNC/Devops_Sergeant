# Git Practice

### Lab1 - Thao tác cơ bản với Git – Cài đặt Git trên máy local.
• Cài đặt Git trên máy local.
• Cài đặt tool Source Tree trên máy local.
• Khởi động Gitbash
• Checkout 1 public repository trên Github.
• Mở repository bằng tool VSCode.
• Sử dụng SourceTree để xem các branch của repository.

### Lab2 - Thao tác cơ bản với Git – Tạo repo trên Github, cấu hình SSH key
• Tạo một repo trên Github (private repository).
• Tạo một SSH key theo guide của Github.
*Google keyword: “github create ssh key”
• Cấu hình private key tại máy local.
• Add public key lên Github account setting.
• Thử checkout một repo sử dụng SSH key.

=> hơi thêm thao tác ở add SSH key thôi


### Lab3 – Check out nhánh develop, thay đổi, commit, push, tạo Pull Request(PR).
• Tạo nhánh develop từ nhánh master của repository mới checkout.
• Tạo một vài files vd: index.html, README.md
• Push nhánh develop lên repository Github.

=> dể thôi!


### Lab4 – Tạo nhánh feature/bugfix từ nhánh develop.
• Thiết lập protect branch cho nhánh master và nhánh develop (chặn push trực tiếp).
• Tạo nhánh feature từ nhánh develop. Vd: feature/F001
• Modify một vài file code.
• Push nhánh feature/F001 lên remote repository.
• Tạo Pull Request (PR) từ feature/F001 vào nhánh develop.
• Tiến hành review và approve Pull Request.

=> dể luôn , khởi đầu cho quy trình gitflow

### Lab5 – Tạo nhánh release từ nhánh develop.
• Tạo nhánh release/v0.1.
• Merge nhánh release/v0.1 vào master.
• Đánh tag cho nhánh master, vd v0.1
• [Optional] merge nhánh release/v0.1 ->develop nếu có thay đổi

=> Quan trọng, ý nghĩa chổ này là sau khi test gì đó trên nhánh release thì có thay đổi. Chỉnh sửa xong thì merge lên main và đống thời phải đồng bộ người về develop cho dev phats triển . Nếu thấy version release này ổn định thì đánh tag cho nó ở main luôn, để muốn rollback version thì chỉ cần đi theo tag trước là đươc 