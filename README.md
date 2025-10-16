# Fraud Detection System 🛡️

[![CI/CD](https://github.com/your-org/fraud-detection-system/actions/workflows/ci-cd.yml/badge.svg)](https://github.com/your-org/fraud-detection-system/actions/workflows/ci-cd.yml)
[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Docker](https://img.shields.io/badge/Docker-Ready-blue.svg)](docker-compose.yml)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Ready-green.svg)](k8s/)

A comprehensive, production-ready microservices-based fraud detection system built with Spring Boot, Python ML, and modern DevOps practices.

## 🏗️ Architecture Overview

```
┌─────────────────┐    Kafka     ┌─────────────────────┐    HTTP    ┌─────────────────┐
│  Transaction    │─────────────→│  Fraud Detection    │───────────→│   ML Model      │
│  API Service    │              │  Service            │            │   (Python)      │
│  (Spring Boot)  │              │  (Spring Boot)      │            │   (Flask/API)   │
└─────────────────┘              └─────────────────────┘            └─────────────────┘
         │                                  │                                │
         │                                  │                                │
         ▼                                  ▼                                ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                              PostgreSQL Database                                    │
│  ┌─────────────────┐                           ┌─────────────────┐                │
│  │   transactions  │                           │  fraud_results  │                │
│  └─────────────────┘                           └─────────────────┘                │
└─────────────────────────────────────────────────────────────────────────────────────┘
         ▲
         │ Metrics & Monitoring
         ▼
┌─────────────────────────────────────────────────────────────────────────────────────┐
│                            Monitoring Stack                                         │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │   Prometheus    │  │    Grafana      │  │   Kafka UI      │  │  AlertManager   ││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘  └─────────────────┘│
└─────────────────────────────────────────────────────────────────────────────────────┘
```

## 📂 Project Structure

```
fraud-detection-system/
├── transaction-api/                 # Transaction API microservice (Spring Boot)
│   ├── src/main/java/                # Java source code
│   ├── Dockerfile                    # Container configuration
│   └── pom.xml                       # Maven dependencies
├── fraud-detection-service/         # Fraud detection microservice (Spring Boot)
│   ├── src/main/java/                # Java source code with Kafka consumer
│   ├── Dockerfile                    # Container configuration
│   └── pom.xml                       # Maven dependencies
├── ml-model/                        # Python ML service (Flask)
│   ├── app.py                        # Main Flask application
│   ├── train.py                      # Model training script
│   ├── requirements.txt              # Python dependencies
│   └── Dockerfile                    # Container configuration
├── k8s/                             # Kubernetes deployment manifests
│   ├── 00-namespace.yaml             # Namespace and ConfigMaps
│   ├── 01-postgres.yaml              # Database deployment
│   ├── 02-kafka.yaml                 # Kafka and Zookeeper
│   ├── 03-ml-model.yaml              # ML service deployment
│   ├── 04-transaction-api.yaml       # Transaction API deployment
│   ├── 05-fraud-detection.yaml       # Fraud detection service
│   └── deploy.sh                     # Deployment automation script
├── monitoring/                      # Monitoring and observability
│   ├── prometheus.yml                # Prometheus configuration
│   ├── alert_rules.yml              # Alerting rules
│   └── grafana/                      # Grafana dashboards
├── scripts/                         # Automation scripts
│   └── run-integration-tests.sh      # Integration testing
├── .github/workflows/               # GitHub Actions CI/CD
│   └── ci-cd.yml                     # Main pipeline
├── Jenkinsfile                      # Jenkins pipeline
├── docker-compose.yml               # Local development environment
├── schema.sql                       # Database schema
├── start.sh                         # Quick start script
├── stop.sh                          # Shutdown script
└── README.md                        # This comprehensive guide
```

## 🚀 Quick Start

### Prerequisites
- Java 17+
- Python 3.8+
- Docker & Docker Compose
- PostgreSQL
- Apache Kafka

### Local Development

1. **Start infrastructure services:**
   ```bash
   docker-compose up -d postgres kafka zookeeper
   ```

2. **Setup database:**
   ```bash
   psql -h localhost -U postgres -d frauddb -f schema.sql
   ```

3. **Start services:**
   ```bash
   # Terminal 1: Transaction API
   cd transaction-api
   ./mvnw spring-boot:run

   # Terminal 2: Fraud Detection Service  
   cd fraud-detection-service
   ./mvnw spring-boot:run

   # Terminal 3: ML Model Service
   cd ml-model
   python app.py
   ```

## 🔧 Service Details

### Transaction API (Port: 8080)
- **Purpose**: Accept and validate transactions
- **Endpoints**: 
  - `POST /api/transactions` - Submit new transaction
  - `GET /api/transactions/{id}` - Get transaction details

### Fraud Detection Service (Port: 8081) 
- **Purpose**: Process transactions for fraud detection
- **Features**: Kafka consumer, ML model integration, result storage

### ML Model Service (Port: 5000)
- **Purpose**: Fraud risk scoring
- **Endpoints**:
  - `POST /predict` - Get fraud risk score
  - `GET /model/info` - Model information

## 📊 Monitoring

- **Prometheus**: Metrics collection (Port: 9090)
- **Grafana**: Dashboards and visualization (Port: 3000)
- **Health Checks**: Available at `/actuator/health` for each service

## 🚢 Deployment

### Docker
```bash
docker-compose up -d
```

### Kubernetes
```bash
kubectl apply -f k8s/
```

## 🧪 Testing

```bash
# Test transaction submission
curl -X POST http://localhost:8080/api/transactions \
  -H "Content-Type: application/json" \
  -d '{
    "accountId": "ACC-12345",
    "amount": 100.00,
    "currency": "USD",
    "merchant": "Test Store"
  }'
```

## 📈 Performance & Scaling

- **Transaction API**: Stateless, horizontally scalable
- **Fraud Detection**: Event-driven, auto-scaling based on Kafka lag
- **ML Model**: Lightweight, can handle high throughput

## 🔒 Security Features

- Input validation and sanitization
- Rate limiting
- Audit logging
- Secure configurations

## 🛠️ Development Guidelines

1. Follow Spring Boot best practices
2. Implement proper error handling
3. Add comprehensive logging
4. Write unit and integration tests
5. Use feature flags for new functionality
#   F r a u d - D e t e c t i o n - S y s t e m  
 