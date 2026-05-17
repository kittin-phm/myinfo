# Hi, I'm Kittin Phummarawong 👋

## 🙋 About Me
- ☁️ Cloud & Data Engineer — transitioning from 6 years of Service & Quality Engineering
- 📊 Data Analytics Engineer — Python, SQL, BigQuery, Prefect, Power BI
- 🎯 Pursuing AWS SAA-C03 Certification (Udemy · Stephane Maarek, 2026)
- 🎓 B.Eng. Industrial Engineering — Kasetsart University
- 📍 Bangkok, Thailand

---

## 🛠️ Tech Stack

### ☁️ AWS Cloud
![AWS](https://img.shields.io/badge/AWS-232F3E?style=flat&logo=amazon-aws&logoColor=white)
![VPC](https://img.shields.io/badge/VPC-8C4FFF?style=flat&logo=amazon-aws&logoColor=white)
![EC2](https://img.shields.io/badge/EC2-FF9900?style=flat&logo=amazon-ec2&logoColor=white)
![ALB](https://img.shields.io/badge/ALB-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Auto Scaling](https://img.shields.io/badge/Auto_Scaling-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![RDS](https://img.shields.io/badge/RDS-527FFF?style=flat&logo=amazon-rds&logoColor=white)
![DynamoDB](https://img.shields.io/badge/DynamoDB-4053D6?style=flat&logo=amazon-dynamodb&logoColor=white)
![S3](https://img.shields.io/badge/S3-569A31?style=flat&logo=amazon-s3&logoColor=white)
![Lambda](https://img.shields.io/badge/Lambda-FF9900?style=flat&logo=aws-lambda&logoColor=white)
![API Gateway](https://img.shields.io/badge/API_Gateway-FF4F8B?style=flat&logo=amazon-aws&logoColor=white)
![CloudFront](https://img.shields.io/badge/CloudFront-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![CloudWatch](https://img.shields.io/badge/CloudWatch-FF4F8B?style=flat&logo=amazon-cloudwatch&logoColor=white)
![IAM](https://img.shields.io/badge/IAM-DD344C?style=flat&logo=amazon-aws&logoColor=white)
![Route53](https://img.shields.io/badge/Route53-8C4FFF?style=flat&logo=amazon-aws&logoColor=white)

### 📊 Data Engineering
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat&logo=mysql&logoColor=white)
![BigQuery](https://img.shields.io/badge/BigQuery-4285F4?style=flat&logo=google-cloud&logoColor=white)
![Prefect](https://img.shields.io/badge/Prefect-024DFD?style=flat&logo=prefect&logoColor=white)
![Power BI](https://img.shields.io/badge/PowerBI-F2C811?style=flat&logo=powerbi&logoColor=black)
![GCP](https://img.shields.io/badge/GCP-4285F4?style=flat&logo=google-cloud&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=flat&logo=pandas&logoColor=white)

### 🔧 DevOps & Tools
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=flat&logo=terraform&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat&logo=github&logoColor=white)

---

## 📂 Featured Projects

### 🛒 Olist E-Commerce Data Pipeline
> ETL Pipeline · BigQuery · Prefect · Power BI · DAX · SQL

End-to-end data pipeline ingesting 100K+ Brazilian e-commerce orders into BigQuery with a layered SQL data model and a Power BI dashboard covering September 2016 – August 2018.

**Pipeline layers:**
```
stg_orders / stg_order_items / stg_payments   ← raw staged data (minimal casting)
  → int_orders_enriched                        ← joined + delivery_lead_time_days + is_ontime flag
    → mart_daily_revenue                       ← aggregated daily mart (Power BI source)
      → KPI views: GMV · AOV · On-Time Rate   ← match dashboard metrics exactly
```

**What was built:**
- **Prefect ETL pipeline** — extract 3 CSVs → type cast → DQ checks (reject null order_id / negative price) → load to BigQuery
- **SQL data models** — staging → intermediate → mart + 3 KPI views
- **Power BI dashboard** — DAX measures, KPI cards, monthly trend charts, year_month slicer

**Dashboard KPIs:**
| Metric | Value |
|---|---|
| Total GMV | 14.21M BRL |
| Avg AOV | 130.72 BRL |
| Avg On-Time Rate | ~85–90% (monthly avg) |

**Data Quality & Null Strategy:**
- `product_category_name`: ~1,600 null rows retained as-is in staging; filtered downstream in intermediate layer to avoid losing order revenue
- `order_delivered_customer_date`: null for undelivered orders — excluded from on-time calculation via `WHERE order_status = 'delivered'`
- Null `order_id` and negative `price` rows rejected in DQ layer before loading

**Setup:**
```bash
# Install dependencies
py -3.11 -m pip install -r requirements.txt

# Authenticate with Google Cloud
gcloud auth application-default login

# Run pipeline (extract → cast → DQ check → load)
py -3.11 pipelines/flow.py
```

**BigQuery:** Project `project-839c799e-2b34-4fae-814` · Dataset `olist_staging`

**Data source:** [Brazilian E-Commerce Dataset — Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

👉 [View Repository](https://github.com/kittin-phm/olist-data-pipeline)

---

### ☁️ AWS Cloud Infrastructure Lab
> VPC · EC2 · ALB · Auto Scaling · RDS · S3 · Lambda · EventBridge · CloudWatch · SNS · Terraform

Production-grade AWS cloud infrastructure built from scratch across 7 phases — all running on a free-tier budget with an automated cost scheduler.

**Architecture:**
```
Internet
    │
    ▼
[Internet Gateway: cloud-lab-igw]
    │
    ▼
[ALB: cloud-lab-alb]  ← HTTP :80
    │
    ▼  (private subnets)
[Auto Scaling Group: cloud-lab-asg]
  EC2 t2.micro · Amazon Linux · Apache httpd
    │
    ▼
[RDS MySQL: cloud-lab-db]
  db.t3.micro · no public access

S3: cloud-lab-bucket-kittin
  Versioning ON · Lifecycle → Glacier after 90 days

Lambda scheduler (Terraform):
  🟢 09:00 BKK (weekdays) → ASG/RDS ON
  🔴 18:00 BKK (weekdays) → ASG/RDS OFF
```

**What was built:**

| Phase | What |
|---|---|
| 1 — VPC & Networking | CIDR `10.0.0.0/16` · 2 public + 2 private subnets across 2 AZs · IGW + route table |
| 2 — Security Groups | ALB-sg → EC2-sg → RDS-sg (least-privilege chain) |
| 3 — Database | RDS MySQL `db.t3.micro` in private subnets · automated backups |
| 4 — Compute | Launch template + ASG (min 1 / max 3) · CPU scaling at 50% · ALB |
| 5 — Storage | S3 with versioning + Glacier lifecycle after 90 days |
| 6 — Cost Scheduler | Terraform stack · Lambda + EventBridge · 16 resources · shuts down after hours |
| 7 — Monitoring | CloudWatch dashboard · CPU alarm >70% for 5 min → SNS email alert |

**Terraform scheduler** (`cloud-lab-scheduler/`):
```bash
terraform init    # ✅ Initialized
terraform plan    # ✅ 16 resources planned
terraform apply   # ✅ 16 resources created
```

**Live URL:** [http://cloud-lab-alb-521128514.ap-southeast-1.elb.amazonaws.com](http://cloud-lab-alb-521128514.ap-southeast-1.elb.amazonaws.com)

👉 [View Repository](https://github.com/kittin-phm/aws-cloud-infrastructure-lab)

---

## 📜 Certifications & Training
- 🎯 AWS SAA-C03 — Udemy · Stephane Maarek, 2026
- 📊 Data Engineering Bootcamp — Data TH School, 2025
- 🐍 Data Engineering & Python — Datacamp, 2024
- ☁️ Google Cloud / Data Engineering — Coursera, 2024
- 📈 Data Analyst Fundamentals — Data Rockie School, 2024

---

## 💼 Work Experience
- **Data Analyst Intern** — August Ten Digital (Oct – Nov 2025)
- **Service Engineer** — WIKA Instrumentation Thailand (2022–2025)
- **Calibration Engineer** — Ming Deng Metrology Thailand (2018–2022)

---

## 📬 Contact
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Kittin_Phummarawong-0077B5?style=flat&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/kittin-phummarawong-73b367291/)
[![Email](https://img.shields.io/badge/Email-kittin.phm@gmail.com-D14836?style=flat&logo=gmail&logoColor=white)](mailto:kittin.phm@gmail.com)
[![Phone](https://img.shields.io/badge/Phone-093--451--5693-25D366?style=flat&logo=whatsapp&logoColor=white)](tel:+66934515693)
