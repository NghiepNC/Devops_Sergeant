# Terraform State Management

## Tổng quan
Dự án này minh họa cách quản lý state trong Terraform sử dụng S3 backend và DynamoDB cho state locking. Đây là một best practice quan trọng khi làm việc với Terraform trong môi trường team.

## Kiến trúc State Management

```
                    [Terraform CLI]
                           |
                           |
                    [S3 Backend]
                    (State File)
                           |
                           |
                    [DynamoDB]
                    (State Locking)
```

## Các thành phần chính

### 1. S3 Backend
- Lưu trữ state file
- Versioning được bật
- Encryption được bật
- Access logging được bật
- Lifecycle rules được cấu hình

### 2. DynamoDB Table
- Sử dụng cho state locking
- Primary key: LockID (String)
- Đảm bảo không có nhiều người cùng thay đổi state

## Cấu trúc thư mục

```
lab9_storage_tf_stage/
├── keypair/           # SSH key pairs
├── main.tf           # Main configuration & backend config
├── variables.tf      # Input variables
├── outputs.tf        # Output values
└── terraform.tfvars  # Variable values
```

## Cấu hình Backend

### 1. Tạo S3 Bucket
```bash
aws s3api create-bucket \
    --bucket udemy-terraform-state-singapore-nghiepnc \
    --region ap-southeast-1 \
    --create-bucket-configuration LocationConstraint=ap-southeast-1

# Bật versioning
aws s3api put-bucket-versioning \
    --bucket udemy-terraform-state-singapore-nghiepnc \
    --versioning-configuration Status=Enabled

# Bật encryption
aws s3api put-bucket-encryption \
    --bucket udemy-terraform-state-singapore-nghiepnc \
    --server-side-encryption-configuration '{
        "Rules": [
            {
                "ApplyServerSideEncryptionByDefault": {
                    "SSEAlgorithm": "AES256"
                }
            }
        ]
    }'
```

### 2. Tạo DynamoDB Table
```bash
aws dynamodb create-table \
    --table-name udemy-terraform-state \
    --attribute-definitions AttributeName=LockID,AttributeType=S \
    --key-schema AttributeName=LockID,KeyType=HASH \
    --provisioned-throughput ReadCapacityUnits=5,WriteCapacityUnits=5 \
    --region ap-southeast-1
```

### 3. Cấu hình trong main.tf
```hcl
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = ">= 5.0.0"
    }
  }
  backend "s3" {
    bucket         = "udemy-terraform-state-singapore-nghiepnc"
    key            = "udemy-terraform"
    region         = "ap-southeast-1"
    dynamodb_table = "udemy-terraform-state"
  }
}
```

## Cách sử dụng

1. **Tạo S3 Bucket và DynamoDB Table**
   - Thực hiện các lệnh AWS CLI ở trên
   - Hoặc tạo thủ công qua AWS Console

2. **Cấu hình Backend**
   - Thêm cấu hình backend vào file `main.tf`
   - Đảm bảo các thông số khớp với resources đã tạo

3. **Khởi tạo Terraform**
   ```bash
   terraform init
   ```

4. **Kiểm tra cấu hình**
   ```bash
   terraform plan
   ```

5. **Triển khai infrastructure**
   ```bash
   terraform apply
   ```

## Biến số cấu hình

Các biến số chính trong `terraform.tfvars`:
- `region`: AWS region (ap-southeast-1)
- `environment`: Môi trường (dev, staging, prod)

## Lợi ích của State Management

1. **Team Collaboration**
   - Nhiều người có thể làm việc cùng lúc
   - Tránh conflict khi thay đổi state
   - Lịch sử thay đổi được lưu trữ

2. **Security**
   - State file được mã hóa
   - Access control thông qua IAM
   - Audit logging được bật

3. **Reliability**
   - State locking ngăn chặn race conditions
   - Versioning cho phép rollback
   - High availability với S3

## Best Practices

1. **State Security**
   - Luôn bật encryption
   - Sử dụng IAM roles
   - Bật versioning
   - Cấu hình access logging

2. **State Locking**
   - Luôn sử dụng DynamoDB
   - Cấu hình timeout phù hợp
   - Xử lý lock errors

3. **Backup & Recovery**
   - Sử dụng versioning
   - Cấu hình lifecycle rules
   - Regular backup testing

4. **Access Control**
   - Least privilege principle
   - IAM roles và policies
   - Regular audit

## Lưu ý quan trọng

1. **State File**
   - Không bao giờ commit state file
   - Sử dụng .gitignore
   - Backup thường xuyên

2. **Locking**
   - Luôn sử dụng state locking
   - Xử lý lock timeout
   - Cleanup stale locks

3. **Versioning**
   - Bật versioning cho S3
   - Cấu hình lifecycle rules
   - Regular cleanup

4. **Security**
   - Mã hóa state file
   - Access logging
   - Regular audit 