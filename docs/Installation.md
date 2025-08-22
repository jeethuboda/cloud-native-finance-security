# Installation Guide

This guide provides step-by-step instructions to set up the **Cloud-Native Finance Security Framework**, an open-source initiative to enhance cybersecurity, regulatory compliance, and real-time analytics for U.S. financial and insurance sectors. The framework leverages Java Spring Boot, AWS services, Apache Kafka, Docker, and Istio, aligning with national priorities (National Cybersecurity Strategy 2025, EO 14028, CISA 2025-2026 Plan). It supports community adoption to mitigate $16.6B in cybercrime losses (FBI IC3 2024) and $308.6B in insurance fraud (NAIC 2025).

## Prerequisites
Before installing, ensure the following are available:
- **System**: Linux (Ubuntu 20.04+), macOS (12+), or Windows (10+ with WSL2).
- **Tools**:
  - Java 17 (OpenJDK recommended).
  - Docker 20.10+ and Docker Compose.
  - Gradle 7.5+ for builds.
  - AWS CLI 2.13+ with an active AWS account.
  - Git 2.30+ for repository cloning.
  - Kubernetes CLI (kubectl) 1.25+ for EKS.
- **AWS Services**:
  - Configured IAM role with permissions for EKS, Kinesis, SageMaker, Comprehend, RDS, S3, Redshift, and WAF.
  - VPC with public/private subnets (see `/infra/terraform/` for setup).
- **Optional**: DataDog API key, Kibana 8.8+ for monitoring.

## Installation Steps

1. **Clone the Repository**
   ```bash
   git clone https://github.com/jeethuboda/cloud-native-finance-security
   cd cloud-native-finance-security
   ```

2. **Set Up Environment Variables**
   Create a `.env` file in the root directory:
   ```bash
   touch .env
   ```
   Add:
   ```env
   AWS_ACCESS_KEY_ID=your_access_key
   AWS_SECRET_ACCESS_KEY=your_secret_key
   AWS_REGION=us-east-1
   DATADOG_API_KEY=your_datadog_key  # Optional
   ```
   Source the file:
   ```bash
   source .env
   ```

3. **Install Dependencies**
   Run Gradle to fetch Java dependencies:
   ```bash
   ./gradlew build
   ```
   Verify Docker images:
   ```bash
   docker-compose -f docker-compose.yml pull
   ```

4. **Deploy Infrastructure**
   Initialize AWS infrastructure using Terraform:
   ```bash
   cd infra/terraform
   terraform init
   terraform apply
   ```
   This sets up EKS, RDS, S3, Redshift, Kinesis, and WAF. See `/infra/terraform/main.tf` for details.

5. **Deploy Microservices**
   Build and deploy services to EKS:
   ```bash
   cd ../../services
   ./gradlew build
   kubectl apply -f k8s/
   ```
   Configure Istio service mesh:
   ```bash
   cd ../infra/istio
   istio apply -f istio-config.yaml
   ```

6. **Set Up Monitoring**
   Configure DataDog and Kibana:
   ```bash
   cd ../monitoring
   kubectl apply -f datadog-agent.yaml
   ./setup-kibana.sh
   ```
   Access dashboards at `http://<kibana-host>:5601`.

7. **Run Demo Applications**
   Test banking/insurance demos:
   ```bash
   cd ../examples
   docker-compose up
   ```
   Access at `http://localhost:8080/demo`.

8. **Verify Setup**
   Check microservices:
   ```bash
   kubectl get pods -n finance-security
   ```
   Test APIs:
   ```bash
   curl -H "Authorization: Bearer <jwt-token>" http://<api-gateway-url>/transaction/health
   ```
   View logs in DataDog or Kibana.

## Troubleshooting
- **AWS Errors**: Verify IAM permissions and region settings.
- **Kafka Lag**: Adjust partitions in `/services/kafka-config.yaml` (default: 100).
- **Build Failures**: Ensure Java 17 and Gradle compatibility.
- See `/docs/troubleshooting.md` for detailed solutions.

## Next Steps
- Explore `/docs/compliance-mapping.md` for regulatory setups.
- Run tests in `/tests/` for load/stress validation.
- Contribute via `/docs/CONTRIBUTING.md`.

## License
MIT License. See `LICENSE` for details.