# Terraform Multi-Environment Management

## Tổng quan
Dự án này minh họa cách quản lý nhiều môi trường (dev, test) trên nhiều region khác nhau (Singapore, Tokyo) sử dụng Terraform. Cấu trúc này cho phép tái sử dụng code và dễ dàng quản lý các môi trường khác nhau.

## Kiến trúc

```
                    [Modules]
                           |
                           |
            +-------------+-------------+
            |                           |
    [Singapore (Dev)]           [Tokyo (Test)]
            |                           |
            |                           |
    [ap-southeast-1]           [ap-northeast-1]
```

## Cấu trúc thư mục

```
lab10_mutil_region_for_env/
├── modules/              # Shared modules
│   ├── network/         # Network configuration
│   ├── security/        # Security groups
│   └── compute/         # EC2 instances
├── singapore-dev/       # Dev environment in Singapore
│   ├── main.tf         # Main configuration
│   ├── variables.tf    # Environment variables
│   └── terraform.tfvars # Dev-specific values
├── tokyo-test/         # Test environment in Tokyo
│   ├── main.tf         # Main configuration
│   ├── variables.tf    # Environment variables
│   └── terraform.tfvars # Test-specific values
└── keypair/            # SSH key pairs
```

## Các Module

### 1. Network Module
- VPC configuration
- Subnets (public/private)
- NAT Gateway
- Internet Gateway
- Route tables

### 2. Security Module
- Security groups
- IAM roles
- Key pairs

### 3. Compute Module
- EC2 instances
- Auto Scaling Groups
- Load Balancers

## Cấu hình môi trường

### Singapore (Dev)
```hcl
# singapore-dev/main.tf
module "network" {
  source = "../../modules/network"
  # Dev-specific variables
}

module "security" {
  source = "../../modules/security"
  # Dev-specific variables
}

module "compute" {
  source = "../../modules/compute"
  # Dev-specific variables
}
```

### Tokyo (Test)
```hcl
# tokyo-test/main.tf
module "network" {
  source = "../../modules/network"
  # Test-specific variables
}

module "security" {
  source = "../../modules/security"
  # Test-specific variables
}

module "compute" {
  source = "../../modules/compute"
  # Test-specific variables
}
```

## Cách sử dụng

1. **Khởi tạo môi trường Dev (Singapore)**
   ```bash
   cd singapore-dev
   terraform init
   terraform plan
   terraform apply
   ```

2. **Khởi tạo môi trường Test (Tokyo)**
   ```bash
   cd tokyo-test
   terraform init
   terraform plan
   terraform apply
   ```

## Biến số cấu hình

### Singapore (Dev)
```hcl
# singapore-dev/terraform.tfvars
environment = "dev"
region     = "ap-southeast-1"
vpc_cidr   = "10.0.0.0/16"
# Other dev-specific variables
```

### Tokyo (Test)
```hcl
# tokyo-test/terraform.tfvars
environment = "test"
region     = "ap-northeast-1"
vpc_cidr   = "10.1.0.0/16"
# Other test-specific variables
```

## Lợi ích của Multi-Environment

1. **Code Reusability**
   - Tái sử dụng modules
   - Giảm duplicate code
   - Dễ dàng maintain

2. **Environment Isolation**
   - Mỗi môi trường độc lập
   - Không ảnh hưởng lẫn nhau
   - Dễ dàng test changes

3. **Region Flexibility**
   - Triển khai ở nhiều region
   - Disaster recovery
   - Compliance requirements

## Best Practices

1. **Module Design**
   - Modules nên độc lập
   - Sử dụng variables cho configuration
   - Cung cấp outputs cần thiết

2. **State Management**
   - Mỗi môi trường một state file
   - Sử dụng remote state
   - Implement state locking

3. **Variable Management**
   - Sử dụng terraform.tfvars
   - Không hardcode values
   - Validate variable types

4. **Documentation**
   - README cho mỗi module
   - README cho mỗi môi trường
   - Comment trong code

## Lưu ý quan trọng

1. **Naming Convention**
   - Nhất quán trong naming
   - Prefix cho mỗi môi trường
   - Tags cho resources

2. **Security**
   - IAM roles cho mỗi môi trường
   - Security groups riêng biệt
   - Encryption cho sensitive data

3. **Cost Management**
   - Monitor resources
   - Cleanup unused resources
   - Use appropriate instance types

4. **Maintenance**
   - Regular updates
   - Version control
   - Backup state files 