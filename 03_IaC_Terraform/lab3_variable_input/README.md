# Lab 3: Sử dụng Variables trong Terraform

## Mục tiêu
- Hiểu cách sử dụng biến trong Terraform
- Tạo và quản lý EC2 instance với các tham số có thể tùy chỉnh
- Tái sử dụng keypair từ lab trước

## Cấu trúc thư mục
```
lab3_variable/
├── main.tf           # File chính định nghĩa EC2 instance
├── variables.tf      # Định nghĩa các biến
├── terraform.tfvars  # Giá trị của các biến
├── outputs.tf        # Định nghĩa output
└── keypair/         # Thư mục chứa keypair từ lab2
```

## Giải thích các file

### 1. variables.tf
File này định nghĩa các biến được sử dụng trong Terraform configuration:
```hcl
variable "instance_type" {
  description = "Loại instance EC2"
  type        = string
  default     = "t2.micro"
}

variable "ami_id" {
  description = "ID của AMI"
  type        = string
}

variable "key_name" {
  description = "Tên của keypair"
  type        = string
}
```

Các thành phần chính:
- `description`: Mô tả mục đích của biến
- `type`: Kiểu dữ liệu của biến (string, number, list, map, etc.)
- `default`: Giá trị mặc định (không bắt buộc)

### 2. terraform.tfvars
File này chứa giá trị thực tế cho các biến đã định nghĩa:
```hcl
instance_type = "t2.micro"
ami_id        = "ami-0c55b159cbfafe1f0"
key_name      = "udemy-key"
```

Lưu ý:
- File này thường chứa thông tin nhạy cảm nên được thêm vào .gitignore
- Có thể tạo nhiều file .tfvars khác nhau cho các môi trường khác nhau

## Các lệnh Terraform

1. Khởi tạo Terraform:
```bash
terraform init
```

2. Xem trước các thay đổi:
```bash
terraform plan --var-file "terraform.tfvars"
```

3. Áp dụng các thay đổi:
```bash
terraform apply --var-file "terraform.tfvars"
```

4. Xóa tài nguyên:
```bash
terraform destroy --var-file "terraform.tfvars"
```

## Kết nối đến EC2 Instance

Sau khi tạo instance thành công, kết nối bằng SSH:
```bash
ssh -i ./keypair/udemy-key ec2-user@<public_ip>
```

## Tài liệu tham khảo
- [Terraform Variables Documentation](https://developer.hashicorp.com/terraform/language/values/variables)
- [AWS EC2 Instance Resource](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)

## Lưu ý
- Đảm bảo keypair từ lab2 đã được copy vào thư mục `keypair/`
- Kiểm tra giá trị AMI ID phù hợp với region của bạn
- Không commit file terraform.tfvars lên git repository 