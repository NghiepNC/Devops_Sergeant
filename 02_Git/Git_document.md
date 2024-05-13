# Git và những thứ liên quan 

## 1. Git và tiểu sử 
Ở chổ này tui hẻm ghi nhiều đâu nhen

=> Nhưng mong đợi ai cũng hiểu chung về chức năng của nó là **quản lý source code**

## 2. Các thành phần và thao tác của git cần nắm

Dưới đây là hình ảnh mô tả git
![Git và các thành phần ](Picture/git_.png)

- GitRepository:

	Nơi lưu trữ code á =)) . Lưu trữ toàn bộ source code , history có thể nằm ở máy local, server hoặc trên cloud.
- Commit :

	Nhiều người hay hỏi " M commit chưa mà nói xong task mậy =)) ". Commit có thể hiểu là 1 mốc mình chốt code của mình và đưa code lên (Văn nói vậy thôi). Hay thầy Linh bảo là ảnh chụp (snapshot) của codebase tại 1 thời điểm cụ thể

- Branch: 

	Nhánh thui 

- Merge:

	Hành động kết hợp 1 branch và branch khác 

- Remote: 

	Đây là cơ chế hay của git như việc copy 1 bản repository nằm trên máy khác. Điều này có thể cho phép nhiều develop phát triển và đồng bộ các thay đỏi giữa các bản sao với nhau . Tránh mất code nữa ^^

- Clone: 

	Hành động tạo bản sao của repo á

- Pull: 

	Thao tác download thay đổi từ 1 remote repository và merge chúng vào local repository

- Push :

	Thao tác upload thay đổi từ local repo lên remote repo

## 3. Vòng đời của Git (Git life cycle)

Hình ảnh vòng đời của git đây nè!
![Git life cycle](Picture/git_life_cycle.png)

1. Create: Tạo mới repository trên máy
local hoặc trên remote server

2. Modify: add thêm các file code sử dụng các IDE. Git sẽ tự động detect các
thay đổi. 

	=> thật là quyền năng để xác định được code thay đổi những gì 😘
không có git thì mọi thay đổi nhìn bằng mắt ngu người luôn á 😤  

3. Stage: sử dụng *git add command* để chuẩn bị các thay đổi sẽ được commit
vào repository.

4. Commit: Apply các thay đổi vào repository, này là mới apply ở repo local thôi nhé 😉

5. Push: đẩy những thay đổi từ local
repository lên một repository khác (vd
Github, Git server công ty.

## 4. Các mô hình workflow với git

