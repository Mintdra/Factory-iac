mekong-iac/  
├── .gitignore  
├── README.md  
├── Makefile                  \# Level 3: \`make check\` runs fmt, validate, test, checkov  
├── versions.tf               \# terraform \+ provider version pins  
├── providers.tf              \# region, default tags  
├── variables.tf              \# CIDRs, AZs, sizes, thresholds (must match 1B)  
├── locals.tf                 \# shared tags, names  
├── main.tf                   \# wires the modules together  
├── outputs.tf  
├── .terraform.lock.hcl       \# commit this  
│  
├── modules/  
│   ├── network/              \# LEVEL 1  
│   │   ├── main.tf           \# VPC, subnets, IGW, NAT, route tables  
│   │   ├── security\_groups.tf \# alb-sg, app-sg, db-sg  
│   │   ├── endpoints.tf      \# S3 VPC endpoint  
│   │   ├── variables.tf  
│   │   └── outputs.tf  
│   │  
│   ├── alb/                  \# LEVEL 2  
│   │   └── main.tf           \# ALB, target group, listener, health check  
│   │  
│   ├── compute/  
│   │   ├── main.tf           \# ECS cluster, Fargate service, task definition  
│   │   ├── iam.tf            \# task role \+ execution role (JSON policies)  
│   │   └── scaling.tf        \# scheduled scaling 06:40 / 18:40 \+ target tracking  
│   │  
│   ├── database/  
│   │   ├── main.tf           \# RDS Multi-AZ, subnet group  
│   │   └── backup.tf         \# retention, final snapshot (S1)  
│   │  
│   ├── storage/  
│   │   ├── main.tf           \# payslip S3 bucket, versioning, public access block  
│   │   ├── kms.tf            \# customer-managed key \+ key policy (S2)  
│   │   └── bucket\_policy.tf  \# deny non-TLS, deny non-KMS, restrict to task role  
│   │  
│   └── monitoring/  
│       ├── alarms.tf         \# ALB TargetResponseTime alarm (R5)  
│       └── sns.tf            \# topic \+ HR email subscription  
│  
├── policies/                 \# IAM/bucket/KMS policy JSON, same text as in 1B  
│   ├── task-role.json  
│   ├── bucket-policy.json  
│   └── kms-key-policy.json  
│  
├── tests/  
│   ├── network.tftest.hcl    \# LEVEL 1: firewall rule proof  
│   ├── s1\_backup.tftest.hcl  \# LEVEL 2: one file per security rule  
│   ├── s2\_encryption.tftest.hcl  
│   ├── s3\_payslip\_access.tftest.hcl  
│   └── s4\_db\_private.tftest.hcl  
│  
└── docs/  
    ├── scan-results.md       \# LEVEL 3: each checkov finding fixed or justified in one line  
    └── level-notes.md        \# which files belong to which level  
