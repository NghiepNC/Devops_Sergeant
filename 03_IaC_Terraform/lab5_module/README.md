# Module trong Terraform

## Giới thiệu
Module trong Terraform là một cách để tổ chức và tái sử dụng code Terraform. Module cho phép bạn đóng gói các resource liên quan vào một đơn vị có thể tái sử dụng, giúp quản lý cơ sở hạ tầng dễ dàng hơn.

## Cấu trúc Module
Trong dự án này, chúng ta có các module sau:
- `compute`: Module quản lý các tài nguyên tính toán (EC2 instances)
- `security`: Module quản lý các tài nguyên bảo mật (Security Groups)

## Cách sử dụng Module

### 1. Khai báo Module
Để sử dụng một module, bạn cần khai báo nó trong file `main.tf`:

```hcl
module "tên_module" {
  source = "./modules/tên_thư_mục"
  
  # Các biến đầu vào của module
  biến_1 = giá_trị_1
  biến_2 = giá_trị_2
}
```

### 2. Cấu trúc Module
Mỗi module thường bao gồm các file sau:
- `main.tf`: Chứa các resource chính của module
- `variables.tf`: Định nghĩa các biến đầu vào
- `outputs.tf`: Định nghĩa các giá trị đầu ra

### 3. Truyền biến vào Module
Bạn có thể truyền biến vào module thông qua file `terraform.tfvars` hoặc trực tiếp trong khai báo module.

### 4. Lấy giá trị từ Module
Để lấy giá trị từ module, sử dụng cú pháp:
```hcl
module.tên_module.tên_output
```

## Lợi ích của Module
1. **Tái sử dụng code**: Có thể sử dụng lại các module cho nhiều dự án khác nhau
2. **Tổ chức code**: Giúp tổ chức code một cách có cấu trúc và dễ quản lý
3. **Bảo mật**: Có thể kiểm soát quyền truy cập và thay đổi các module
4. **Dễ bảo trì**: Dễ dàng cập nhật và bảo trì code

## Best Practices
1. Đặt tên module rõ ràng và có ý nghĩa
2. Tài liệu hóa các biến đầu vào và đầu ra
3. Sử dụng version control cho module
4. Kiểm tra tính nhất quán của module trước khi sử dụng
5. Tách biệt các module theo chức năng

## Ví dụ
```hcl
# Khai báo module compute
module "compute" {
  source = "./modules/compute"
  
  instance_type = "t2.micro"
  ami_id        = "ami-123456"
}

# Khai báo module security
module "security" {
  source = "./modules/security"
  
  vpc_id = "vpc-123456"
  ports  = [22, 80, 443]
} 