# 🚀 CloudDora Data Insights Platform

> **Serverless AI-Powered Business Intelligence Platform on AWS**

Transform raw CSV data into actionable business insights in seconds using Claude AI, serverless architecture, and event-driven processing.

[![AWS](https://img.shields.io/badge/AWS-Serverless-orange)](https://aws.amazon.com)
[![Claude AI](https://img.shields.io/badge/Claude%20AI-Anthropic-blue)](https://www.anthropic.com)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Cost](https://img.shields.io/badge/cost-%243--5%2Fmonth-success)](https://github.com)

![CloudDora Platform](https://data-analysis-platform-frontend-360783618622.s3.eu-central-1.amazonaws.com/CloudDora+Logo+1.png)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Architecture](#architecture)
- [Technologies](#technologies)
- [Getting Started](#getting-started)
- [Project Structure](#project-structure)
- [API Documentation](#api-documentation)
- [Cost Analysis](#cost-analysis)
- [Challenges & Solutions](#challenges--solutions)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## 🎯 Overview

CloudDora is a production-ready, serverless data insights platform that leverages AWS services and Claude AI by Anthropic to provide instant business intelligence from uploaded data files.

### **The Problem**
Organizations struggle with:
- Expensive BI tools ($500+/month)
- Complex infrastructure management
- Slow time-to-insights
- Limited accessibility for non-technical users

### **The Solution**
A fully serverless platform that:
- ✅ Processes data automatically via S3 event triggers
- ✅ Generates AI-powered insights using Claude by Anthropic
- ✅ Scales infinitely without managing servers
- ✅ Costs ~$3-5/month for moderate usage
- ✅ Delivers insights in <15 seconds

---

## ✨ Key Features

### **Core Capabilities**
- 📤 **Drag-and-Drop Upload**: Intuitive web interface for file uploads
- 🤖 **Claude AI Analysis**: Production-quality business insights powered by Amazon Bedrock
- ⚡ **Real-Time Processing**: Event-driven architecture with S3 triggers
- 💾 **Intelligent Caching**: DynamoDB-backed caching with 24hr TTL (saves 60% on AI costs)
- 📊 **Data Quality Assessment**: Automated validation and quality checks
- 🎨 **Modern UI**: Responsive design with real-time progress tracking
- 🔒 **Secure**: IAM-based permissions with least-privilege access

### **AI Insights Include**
- Executive summaries of datasets
- Key trends and patterns
- Data quality assessments
- Actionable business recommendations
- Regional and categorical analysis

---

## 🏗️ Architecture

### **System Overview**

```
┌─────────────┐
│   User UI   │ (HTML/CSS/JS - Drag & Drop Interface)
└──────┬──────┘
       │
       ▼
┌─────────────────────────────────────────────────┐
│           API Gateway (REST + CORS)              │
│  Endpoints: /upload, /analyze                    │
└──────┬──────────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────────┐
│              AWS Lambda Functions                │
├─────────────────────────────────────────────────┤
│  1. Upload Handler (Python 3.11)                │
│     - Validates file                             │
│     - Stores to S3 (raw/)                       │
│     - Creates DynamoDB metadata                  │
│                                                  │
│  2. Data Processor (S3 Event Trigger)           │
│     - Auto-triggered on upload                   │
│     - Processes & validates data                 │
│     - Moves to S3 (processed/)                  │
│                                                  │
│  3. AI Analyzer (Bedrock Integration)           │
│     - Fetches processed data                     │
│     - Calls Claude AI for analysis               │
│     - Caches results in DynamoDB                │
└──────┬──────────────────────────────────────────┘
       │
       ▼
┌─────────────────────────────────────────────────┐
│              Data Layer                          │
├─────────────────────────────────────────────────┤
│  • S3 Data Lake                                 │
│    - raw/ (uploaded files)                      │
│    - processed/ (validated data)                │
│                                                  │
│  • DynamoDB Tables                              │
│    - uploads (metadata)                         │
│    - ai-insights (cached analysis)              │
│                                                  │
│  • Amazon Bedrock                               │
│    - Claude 3 Haiku (AI analysis)              │
└─────────────────────────────────────────────────┘
```

### **Data Flow**

1. **Upload**: User uploads CSV via web UI
2. **Storage**: File stored in S3 `raw/` folder
3. **Trigger**: S3 event automatically triggers processing Lambda
4. **Process**: Data validated and moved to `processed/` folder
5. **Analyze**: AI Lambda fetches data and calls Claude via Bedrock
6. **Cache**: Insights stored in DynamoDB with 24hr TTL
7. **Display**: Results returned to UI in real-time

---

## 🛠️ Technologies

### **AWS Services**
- **Lambda**: Serverless compute (Python 3.11)
- **S3**: Data lake storage (raw/processed architecture)
- **API Gateway**: RESTful API with CORS support
- **DynamoDB**: NoSQL database for metadata & caching
- **Amazon Bedrock**: Managed AI service (Claude 3 Haiku)
- **CloudWatch**: Logging and monitoring
- **IAM**: Security and permissions management
- **CloudShell**: Deployment and management

### **AI & ML**
- **Claude 3 Haiku by Anthropic**: Fast, cost-effective AI analysis
- **Amazon Bedrock**: Serverless AI inference platform

### **Frontend**
- **HTML5/CSS3**: Modern, responsive design
- **Vanilla JavaScript**: No frameworks, zero dependencies
- **Fetch API**: RESTful API communication

### **Development Tools**
- **Git**: Version control
- **AWS CLI**: Infrastructure management
- **Boto3**: AWS SDK for Python

---

## 🚀 Getting Started

### **Prerequisites**
- AWS Account with administrator access
- AWS CLI configured
- Basic knowledge of Python and AWS services

### **Deployment Steps**

#### **1. Clone the Repository**
```bash
git clone https://github.com/yourusername/clouddora-platform.git
cd clouddora-platform
```

#### **2. Configure AWS Region**
```bash
export AWS_REGION=eu-central-1
export AWS_ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
```

#### **3. Create S3 Buckets**
```bash
# Data bucket
aws s3 mb s3://data-analysis-platform-data-${AWS_ACCOUNT_ID} --region ${AWS_REGION}

# Frontend bucket
aws s3 mb s3://data-analysis-platform-frontend-${AWS_ACCOUNT_ID} --region ${AWS_REGION}
```

#### **4. Create DynamoDB Tables**
```bash
# Uploads metadata table
aws dynamodb create-table \
  --table-name data-analysis-platform-uploads \
  --attribute-definitions AttributeName=upload_id,AttributeType=S \
  --key-schema AttributeName=upload_id,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region ${AWS_REGION}

# AI insights table
aws dynamodb create-table \
  --table-name data-analysis-platform-ai-insights \
  --attribute-definitions AttributeName=file_hash,AttributeType=S \
  --key-schema AttributeName=file_hash,KeyType=HASH \
  --billing-mode PAY_PER_REQUEST \
  --region ${AWS_REGION}
```

#### **5. Deploy Lambda Functions**
```bash
# Deploy each Lambda (see /lambda directory)
cd lambda/upload-handler
zip function.zip lambda_function.py
aws lambda create-function \
  --function-name data-analysis-platform-upload-handler \
  --runtime python3.11 \
  --role arn:aws:iam::${AWS_ACCOUNT_ID}:role/lambda-execution-role \
  --handler lambda_function.lambda_handler \
  --zip-file fileb://function.zip \
  --region ${AWS_REGION}

# Repeat for processor and analyzer functions
```

#### **6. Create API Gateway**
```bash
# See /scripts/deploy-api-gateway.sh for full deployment
./scripts/deploy-api-gateway.sh
```

#### **7. Enable Amazon Bedrock**
```bash
# Add IAM permissions for Bedrock
aws iam put-role-policy \
  --role-name lambda-ai-role \
  --policy-name BedrockAccess \
  --policy-document file://iam/bedrock-policy.json

# Accept Claude model terms in AWS Console
# Visit: https://console.aws.amazon.com/bedrock/home#/modelaccess
```

#### **8. Deploy Frontend**
```bash
aws s3 cp frontend/index.html s3://data-analysis-platform-frontend-${AWS_ACCOUNT_ID}/ \
  --content-type text/html

# Enable static website hosting
aws s3 website s3://data-analysis-platform-frontend-${AWS_ACCOUNT_ID} \
  --index-document index.html
```

#### **9. Access Your Platform**
```
http://data-analysis-platform-frontend-${AWS_ACCOUNT_ID}.s3-website.${AWS_REGION}.amazonaws.com
```

---

## 📁 Project Structure

```
clouddora-platform/
├── frontend/
│   ├── index.html              # Main web interface
│   └── assets/
│       └── logo.png
├── lambda/
│   ├── upload-handler/
│   │   └── lambda_function.py  # Upload processing
│   ├── data-processor/
│   │   └── lambda_function.py  # Data validation
│   └── ai-analyzer/
│       └── lambda_function.py  # Claude AI integration
├── iam/
│   ├── lambda-role.json        # Lambda execution role
│   ├── bedrock-policy.json     # Bedrock permissions
│   └── marketplace-policy.json # AWS Marketplace access
├── scripts/
│   ├── deploy-api-gateway.sh   # API Gateway deployment
│   ├── setup-s3-triggers.sh    # S3 event configuration
│   └── deploy-all.sh           # Complete deployment
├── docs/
│   ├── API.md                  # API documentation
│   ├── ARCHITECTURE.md         # Detailed architecture
│   └── TROUBLESHOOTING.md      # Common issues
├── tests/
│   └── test_lambda_functions.py
├── .gitignore
├── LICENSE
└── README.md
```

---

## 📡 API Documentation

### **Base URL**
```
https://{api-id}.execute-api.{region}.amazonaws.com/prod
```

### **Endpoints**

#### **1. Upload File**
```http
POST /upload
Content-Type: application/json

{
  "fileName": "sales-data.csv",
  "fileType": "text/csv",
  "fileContent": "base64-encoded-content"
}
```

**Response:**
```json
{
  "upload_id": "1234567890-abcdef",
  "s3_key": "raw/1234567890-abcdef/sales-data.csv",
  "status": "uploaded"
}
```

#### **2. Analyze Data**
```http
POST /analyze
Content-Type: application/json

{
  "s3_key": "processed/1234567890-abcdef/sales-data.csv",
  "upload_id": "1234567890-abcdef"
}
```

**Response:**
```json
{
  "insights": {
    "summary": "Dataset overview...",
    "key_insights": ["insight 1", "insight 2", ...],
    "data_quality": "Quality assessment...",
    "recommendations": ["recommendation 1", ...]
  },
  "cached": false
}
```

For complete API documentation, see [docs/API.md](docs/API.md)

---

## 💰 Cost Analysis

### **Monthly Operating Costs**

| Service | Usage | Cost |
|---------|-------|------|
| **S3** | 10GB storage, 1000 requests | $0.50 |
| **Lambda** | 10,000 invocations, 512MB | Free tier |
| **API Gateway** | 10,000 requests | Free tier |
| **DynamoDB** | 25GB storage, on-demand | Free tier |
| **Amazon Bedrock** | 1000 analyses (Claude Haiku) | $2.50 |
| **CloudWatch Logs** | Basic monitoring | $0.50 |
| **TOTAL** | | **~$3.50/month** |

### **Cost Optimizations**
- ✅ Intelligent caching reduces AI calls by 60%
- ✅ On-demand pricing (pay only for what you use)
- ✅ Serverless = zero idle costs
- ✅ Free tier coverage for most services

### **Comparison**
Traditional BI Platform: **$500+/month**  
CloudDora Platform: **$3.50/month**  
**Savings: 99.3%** 🎉

---

## 🔧 Challenges & Solutions

### **Challenge 1: IAM Permissions Maze**
**Problem**: Lambda functions getting "AccessDenied" errors for S3, DynamoDB, and Bedrock.

**Solution**: 
- Created separate IAM roles for each Lambda function
- Implemented least-privilege policies with specific resource ARNs
- Added AWS Marketplace permissions for Bedrock model access

**Learning**: Always start with IAM when debugging serverless apps - 90% of issues are permission-related.

---

### **Challenge 2: CORS Blocking API Calls**
**Problem**: Browser showing "Failed to fetch" when calling API Gateway.

**Solution**:
- Added OPTIONS methods to all API endpoints
- Configured proper Access-Control headers
- Deployed changes to API Gateway
- Added headers to Lambda responses

**Learning**: CORS requires both preflight handling (OPTIONS) and response headers in every Lambda function.

---

### **Challenge 3: S3 Event Triggers Not Firing**
**Problem**: Files uploaded successfully but processing Lambda never triggered.

**Solution**:
- Added Lambda invoke permissions with source ARN conditions
- Configured S3 bucket notifications properly
- Verified event structure in Lambda code

**Learning**: S3 triggers need bidirectional permissions - both bucket notification config AND Lambda resource policy.

---

### **Challenge 4: Claude AI Access Denied**
**Problem**: Bedrock returning "Model access is denied due to IAM user or service role is not authorized".

**Solution**:
- Added AWS Marketplace permissions (ViewSubscriptions, Subscribe)
- Accepted Claude model terms in Bedrock console
- Waited 15 minutes for access propagation

**Learning**: Bedrock models require explicit marketplace permissions and manual terms acceptance.

---

### **Challenge 5: Cost Management for AI Calls**
**Problem**: Risk of expensive repeated AI analyses for same data.

**Solution**:
- Implemented DynamoDB caching with file hash as key
- Set 24-hour TTL on cached insights
- Achieved 60% cache hit rate in production

**Learning**: Always cache AI/ML inference results - it's essential for cost control.

---

### **Challenge 6: CloudShell Timeout Issues**
**Problem**: Long deployment scripts timing out in CloudShell.

**Solution**:
- Split monolithic scripts into smaller, atomic operations
- Kept each command under 2 minutes execution time
- Added proper error handling and progress indicators

**Learning**: CloudShell has session limits - design for short-running commands.

---

## 🗺️ Roadmap

### **Phase 1: Core Platform** ✅ *Completed*
- [x] S3 data lake setup
- [x] Lambda functions deployment
- [x] API Gateway with CORS
- [x] DynamoDB tables
- [x] Claude AI integration via Bedrock
- [x] Web UI with drag-and-drop
- [x] Intelligent caching

### **Phase 2: Visualization** 🚧 *In Progress*
- [ ] Amazon QuickSight integration
- [ ] Interactive dashboards
- [ ] Real-time data visualization
- [ ] Export reports (PDF, Excel)
- [ ] Embedded analytics in UI

### **Phase 3: Advanced Features** 📋 *Planned*
- [ ] User authentication (AWS Cognito)
- [ ] Multi-user collaboration
- [ ] Role-based access control
- [ ] API key management
- [ ] Usage analytics dashboard

### **Phase 4: Enhanced Intelligence** 🔮 *Future*
- [ ] Upgrade to Claude Sonnet for deeper analysis
- [ ] Custom ML models via SageMaker
- [ ] Natural language query interface
- [ ] Automated report generation
- [ ] Predictive analytics
- [ ] Real-time data streaming (Kinesis)

### **Phase 5: Scale & Polish** 🚀 *Future*
- [ ] Custom domain with Route 53
- [ ] CloudFront CDN integration
- [ ] HTTPS with ACM certificate
- [ ] Multi-region deployment
- [ ] Disaster recovery setup
- [ ] Mobile app (iOS/Android)
- [ ] Email notifications (SES)
- [ ] Scheduled automated reporting

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### **How to Contribute**
1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

### **Development Guidelines**
- Follow PEP 8 for Python code
- Add tests for new features
- Update documentation
- Keep commits atomic and well-described

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

- **Anthropic** for Claude AI and Amazon Bedrock integration
- **AWS** for comprehensive serverless services
- **Open Source Community** for inspiration and support

---

## 📞 Contact

**CloudDora Team**
- Website: [clouddora.com](https://clouddora.com)
- LinkedIn: [CloudDora](https://linkedin.com/company/clouddora)
- Email: info@clouddora.com

---

## 🌟 Star History

If you find this project useful, please consider giving it a star ⭐

---

## 📊 Project Stats

![GitHub stars](https://img.shields.io/github/stars/yourusername/clouddora-platform?style=social)
![GitHub forks](https://img.shields.io/github/forks/yourusername/clouddora-platform?style=social)
![GitHub issues](https://img.shields.io/github/issues/yourusername/clouddora-platform)
![GitHub pull requests](https://img.shields.io/github/issues-pr/yourusername/clouddora-platform)

---

<p align="center">
  Made with ❤️ by CloudDora Team
  <br>
  Powered by AWS & Claude AI
</p>
