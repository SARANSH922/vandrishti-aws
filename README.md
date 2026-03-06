# 🌿 VanDrishti — Forest Land Claims Portal

A full-stack serverless web application built on AWS that helps tribal communities file forest land rights claims under the **Forest Rights Act (FRA) 2006** across 5 states in India.

> Originally built as a Hackathon project and later deployed on AWS with a complete serverless backend.

---

## 🌐 Live Demo

> Hosted on AWS with a serverless backend.



## AWS Architecture

```
User → AWS edge delivery (HTTPS)
           ↓
        S3 Bucket (Static Website)
           ↓
     API Gateway (HTTPS Endpoint)
           ↓
     Lambda Function (Python 3.14)
           ↓
       DynamoDB (NoSQL Database)
```

### Services Used

| Service | Purpose |
|---|---|
| **Amazon S3** | Hosts all static files (HTML, CSS, JS) |
| **AWS edge delivery** | Serves the website globally with HTTPS |
| **AWS Lambda** | Serverless Python function — processes claim submissions |
| **Amazon API Gateway** | Public HTTPS endpoint connecting frontend to Lambda |
| **Amazon DynamoDB** | NoSQL database storing all claim records |
| **AWS IAM** | Roles and permissions — least privilege access |

---

## 🚀 Features

- 📊 **Dashboard** — Live stats, recent claims, state coverage, activity feed
- 🗺️ **Interactive Map** — Leaflet.js powered India map with state/city drill-down
- 📋 **Submit Claims** — Full claim form with real-time validation, drag & drop file upload, live summary sidebar
- 📈 **Reports** — Searchable/filterable claims table, donut chart, bar chart analytics
- ⚙️ **Settings** — Profile, notifications, security, appearance settings
- 🔒 **HTTPS** — Secured with an SSL certificate
- ✅ **Serverless Backend** — Form submissions saved to DynamoDB in real-time

---

## 🛠️ Tech Stack

**Frontend**
- HTML5, CSS3, Vanilla JavaScript
- Leaflet.js (Interactive Maps)
- Font Awesome 6.5 (Icons)
- Google Fonts — Syne + DM Sans

**Backend (AWS Serverless)**
- AWS Lambda (Python 3.14)
- Amazon API Gateway (HTTP API)
- Amazon DynamoDB (NoSQL)
- boto3 (AWS Python SDK)

**Infrastructure**
- Amazon S3 (Static Hosting)
- AWS edge delivery (HTTPS)
- AWS IAM (Access Control)

---

## 📁 Project Structure

```
vandrishti/
├── dashboard.html       # Main dashboard with stats & activity
├── map2.html            # Interactive India map page
├── map2.js              # Leaflet map logic
├── claims.html          # Submit new claim form
├── reports.html         # Analytics & claims table
├── settings.html        # User settings page
├── shared.css           # Global design system & theme
└── README.md
```

---

## ⚙️ How It Works

### Claim Submission Flow
1. User fills the claim form on the VanDrishti website
2. Clicks **"Submit Claim"**
3. JavaScript sends a `POST` request to **API Gateway**
4. API Gateway triggers the **Lambda function**
5. Lambda generates a unique Claim ID (e.g. `VD-A3F9B2C1`)
6. Lambda saves the full claim record to **DynamoDB**
7. User sees a success toast with their Claim ID ✅

### Lambda Function (Python)
```python
import json, boto3, uuid
from datetime import datetime

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('VanDrishtiClaims')

def lambda_handler(event, context):
    body = json.loads(event['body'])
    claim = {
        'claimid': 'VD-' + str(uuid.uuid4())[:8].upper(),
        'applicantName': body.get('applicantName'),
        'state': body.get('state'),
        'village': body.get('village'),
        'area': body.get('area'),
        'status': 'Pending',
        'submittedAt': datetime.utcnow().isoformat()
    }
    table.put_item(Item=claim)
    return {
        'statusCode': 200,
        'headers': {'Access-Control-Allow-Origin': '*'},
        'body': json.dumps({'claimId': claim['claimid']})
    }
```

---

## 🧠 What I Learned

- Hosting static websites on **Amazon S3** and configuring bucket policies
- Setting up global HTTPS delivery for static files
- Writing **serverless functions** with AWS Lambda in Python
- Creating **REST APIs** with Amazon API Gateway
- Storing and querying data in **DynamoDB** (NoSQL)
- Configuring **IAM roles** and least privilege permissions
- Understanding and fixing **CORS** issues between frontend and API
- Managing cached static files after deployments
- End-to-end **serverless architecture** on AWS

---

## 💰 AWS Cost

Entire project runs on **AWS Free Tier — $0/month**

| Service | Free Tier |
|---|---|
| S3 | 5GB storage, 20K requests/month |
| Static delivery | 1TB transfer, 10M requests/month |
| Lambda | 1M requests/month |
| API Gateway | 1M requests/month |
| DynamoDB | 25GB storage forever |

---

## 🔮 What's Next

- [ ] **Amazon SNS** — Email notifications on claim submission
- [ ] **Amazon Cognito** — User authentication & login
- [ ] **S3 File Upload** — Store actual claim documents
- [ ] **Custom Domain** — Route 53 + custom domain name

---

## 📄 License

MIT License — feel free to use and modify
