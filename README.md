# Spectre Analytica - Crime Risk Assessment Platform

![Spectre Analytica Logo](frontend/logo.png)

**Engineering Tomorrow's Intelligence Today**

A comprehensive crime risk assessment and route planning platform for South Africa, featuring real-time crime data analysis, 3D mapping, and intelligent route optimization.

## 🌟 Features

### 🎯 Core Functionality
- **Real-time Crime Data Analysis** - Live crime incident tracking and analysis
- **3D Crime Visualization** - Interactive Cesium-powered 3D mapping
- **Safe Route Planning** - AI-powered route optimization based on crime risk
- **Risk Assessment Reports** - Professional PDF reports with executive summaries
- **Real-time Alerts** - Push notifications for route warnings and area alerts
- **Multi-layered Security** - Comprehensive user authentication and role management

### 📊 Dashboard Features
- Real-time crime statistics and KPIs
- Interactive charts and data visualizations
- Provincial crime distribution analysis
- Community reporting integration
- Full-screen professional interface

### 🗺️ Crime Map
- 3D/2D view toggle functionality
- Crime category filtering (Murder, Assault, Robbery, Carjacking, etc.)
- Time range selection (7 days to 6 months)
- Interactive crime markers with severity indicators
- High-risk area visualization

### 🛣️ Route Planning
- Travel origin and destination input
- Threat radius configuration
- Profession-based risk assessment
- Vehicle type optimization
- Travel time preferences
- PDF report generation

## 🏗️ Architecture

### Frontend
- **Technology**: HTML5, CSS3, JavaScript (ES6+)
- **Mapping**: Cesium Ion 3D mapping engine
- **Charts**: Chart.js for data visualization
- **Design**: Responsive dark theme with vibrant data displays

### Backend
- **Framework**: Flask (Python 3.11+)
- **Database**: MySQL 8.0 with comprehensive schema
- **API**: RESTful API with JSON responses
- **Security**: CORS enabled, SQL injection protection

### Database Schema
- **Users**: Authentication and profile management
- **Crime Incidents**: Geolocation-based crime data
- **Routes**: Calculated safe routes with risk scoring
- **Reports**: PDF report metadata and content
- **Areas**: Risk zones and safe area definitions
- **Alerts**: Real-time notification system
- **Data Sources**: Web scraping source management
- **System Logs**: Comprehensive audit trail

## 🚀 Quick Start

### Prerequisites
- Python 3.11+
- MySQL 8.0+
- Node.js 18+ (for development)
- Git

### Local Development Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/YOUR_USERNAME/spectre-analytica.git
   cd spectre-analytica
   ```

2. **Setup Database**
   ```bash
   mysql -u root -p
   CREATE DATABASE spectre_analytica;
   CREATE USER 'spectre_user'@'localhost' IDENTIFIED BY 'your_password';
   GRANT ALL PRIVILEGES ON spectre_analytica.* TO 'spectre_user'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   
   mysql -u spectre_user -p spectre_analytica < database/schema.sql
   ```

3. **Setup Backend**
   ```bash
   cd backend
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   pip install -r requirements.txt
   
   # Configure environment variables
   cp .env.example .env
   # Edit .env with your database credentials
   
   python main.py
   ```

4. **Setup Frontend**
   ```bash
   cd frontend
   # Serve with any web server, e.g.:
   python -m http.server 8080
   # Or use nginx, Apache, etc.
   ```

5. **Access the application**
   - Frontend: http://localhost:8080
   - Backend API: http://localhost:5000
   - Health Check: http://localhost:5000/api/health

## ☁️ AWS Deployment

### Option 1: CloudFormation (Recommended)

1. **Deploy Infrastructure**
   ```bash
   aws cloudformation create-stack \
     --stack-name spectre-analytica-prod \
     --template-body file://aws-infrastructure/cloudformation-template.yaml \
     --parameters ParameterKey=KeyPairName,ParameterValue=your-key-pair \
                  ParameterKey=DomainName,ParameterValue=yourdomain.com \
     --capabilities CAPABILITY_IAM
   ```

2. **Set Database Password**
   ```bash
   aws ssm put-parameter \
     --name "/spectre-analytica/database/password" \
     --value "your-secure-password" \
     --type "SecureString"
   ```

### Option 2: Terraform

1. **Initialize Terraform**
   ```bash
   cd aws-infrastructure/terraform
   terraform init
   ```

2. **Configure Variables**
   ```bash
   cp terraform.tfvars.example terraform.tfvars
   # Edit terraform.tfvars with your values
   ```

3. **Deploy**
   ```bash
   terraform plan
   terraform apply
   ```

### Option 3: Docker + ECS

1. **Build and Push Images**
   ```bash
   # Build images
   docker-compose -f aws-infrastructure/docker-compose.aws.yml build
   
   # Tag and push to ECR
   aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin YOUR_ACCOUNT.dkr.ecr.us-east-1.amazonaws.com
   docker tag spectre-analytica_frontend:latest YOUR_ACCOUNT.dkr.ecr.us-east-1.amazonaws.com/spectre-frontend:latest
   docker tag spectre-analytica_backend:latest YOUR_ACCOUNT.dkr.ecr.us-east-1.amazonaws.com/spectre-backend:latest
   docker push YOUR_ACCOUNT.dkr.ecr.us-east-1.amazonaws.com/spectre-frontend:latest
   docker push YOUR_ACCOUNT.dkr.ecr.us-east-1.amazonaws.com/spectre-backend:latest
   ```

## 🔧 Configuration

### Environment Variables

#### Backend (.env)
```env
FLASK_ENV=production
FLASK_DEBUG=False
PORT=5000
DB_HOST=localhost
DB_PORT=3306
DB_NAME=spectre_analytica
DB_USER=spectre_user
DB_PASSWORD=your_password
CESIUM_ION_TOKEN=your_cesium_token
AWS_REGION=us-east-1
```

#### AWS Deployment
- Set database password in AWS Systems Manager Parameter Store
- Configure Cesium Ion token for 3D mapping
- Set up CloudWatch for monitoring and logging

## 📊 API Documentation

### Health Check
```
GET /api/health
Response: {"status": "operational", "database": "connected", "timestamp": "..."}
```

### Crime Data
```
GET /api/crime-data
Response: {"success": true, "data": [...], "count": 5}
```

### Route Planning
```
POST /api/route-planning
Body: {
  "origin_address": "Johannesburg, SA",
  "destination_address": "Cape Town, SA",
  "origin_lat": -26.2041,
  "origin_lng": 28.0473,
  "destination_lat": -33.9249,
  "destination_lng": 18.4241,
  "threat_radius_km": 10
}
Response: {"success": true, "route_id": 1, "data": {...}}
```

### Dashboard Statistics
```
GET /api/dashboard-stats
Response: {"success": true, "data": {"total_incidents": 5, "high_risk_areas": 2, ...}}
```

## 🔒 Security Features

- **Database Security**: Encrypted connections, parameterized queries
- **API Security**: CORS configuration, input validation
- **Infrastructure Security**: VPC isolation, security groups, encrypted storage
- **Authentication**: Role-based access control, secure password hashing
- **Monitoring**: CloudWatch integration, audit logging

## 📈 Monitoring & Maintenance

### CloudWatch Metrics
- Application performance monitoring
- Database connection health
- API response times
- Error rate tracking

### Automated Backups
- Daily database backups to S3
- 7-day local backup retention
- Point-in-time recovery capability

### Log Management
- Application logs via CloudWatch
- Nginx access and error logs
- Database query logging

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines
- Follow PEP 8 for Python code
- Use semantic commit messages
- Add tests for new features
- Update documentation as needed

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🆘 Support

For support and questions:
- Create an issue in this repository
- Contact: admin@spectreanalytica.com
- Documentation: [docs/](docs/)

## 🙏 Acknowledgments

- **Cesium Ion** for 3D mapping capabilities
- **South African Police Service (SAPS)** for crime data sources
- **Institute for Security Studies (ISS)** for crime research data
- **Community Safety Forums** for community-reported incidents

---

**Spectre Analytica** - Engineering Tomorrow's Intelligence Today

