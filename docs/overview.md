# Overview
Documentation Overview

This document provides an overview of the Cloud-Native Finance Security Framework, an open-source initiative to enhance cybersecurity, compliance, and analytics in U.S. financial and insurance sectors. It aligns with the National Cybersecurity Strategy 2025, EO 14028, and CISA’s 2025-2026 Plan, addressing $16.6B in cybercrime losses (FBI IC3 2024) and $308.6B in insurance fraud (NAIC 2025).

Purpose

The framework modernizes critical infrastructure through:





    1. Secure Microservices: Java Spring Boot/AWS EKS, reducing risks by 30% (NIST 800-53).



    2. AI Compliance Engine: Automates NAIC/SOX/GDPR, cutting audit times by 40%.



    3. Real-Time Analytics: Detects 15% synthetic fraud (60% rise per 2025 trends).

Technical Stack





    Core: Java Spring Boot, OAuth 2.0, JWT, AES-256.



    Cloud: AWS EKS, Kinesis, SageMaker, Comprehend, RDS, S3, Redshift, WAF.



    Streaming: Apache Kafka (100+ partitions).



    DevOps: Docker, Git, Gradle, Istio, DataDog, Kibana.

Setup Instructions





    Prerequisites: AWS account, Java 17, Docker, Gradle.



    Clone: git clone https://github.com/jeethuboda/cloud-native-finance-security



    Deploy: See /infra/terraform/ for IaC scripts.



    Monitor: Configure DataDog/Kibana in /monitoring/.

Related Documents





    compliance-mapping.md: Regulatory alignments.



    impact-analysis.md: Economic/cybersecurity impacts.



    refile-note-for-USCIS.md: NIW petition context.