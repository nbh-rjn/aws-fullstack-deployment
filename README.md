# Overview
This project demonstrates the deployment of a cloud-native full-stack web application using AWS infrastructure. It features:

1. Frontend: HTML, JavaScript (static)

2. Backend: Flask (Python)

3. Database: PostgreSQL (Amazon RDS)

4. File Storage: AWS S3

5. Deployment: EC2 for backend, Elastic Beanstalk for frontend

6. Security: IAM roles, Security Groups, Key Pairs

# Deployment Guide

Database Setup

  - Launch an Amazon RDS PostgreSQL instance.

  - Allow EC2 access via RDS Security Group (open port 5432).

  - Connect using SQLAlchemy and run the schema creation scripts.

S3 Bucket Setup

  - Create a new S3 bucket via AWS Console.

  - Add IAM policy allowing read/write access for your EC2 instance.

  - Configure upload routes in Flask backend to store images to S3.

Security & IAM

  - Create IAM roles with least-privilege permissions for EC2, RDS, and S3.

  - Configure Security Groups:

    - EC2: allow ports 22 (SSH), 5000 (API), 80 (HTTP)

    - RDS: allow 5432 from EC2's SG

  - Generate and securely store Access Keys for programmatic access.

Backend Deployment

  - Create an EC2 instance inside a VPC.

  - SSH into the instance using the generated key pair.

  - Install Docker and Docker Compose:
    sudo yum install docker
    sudo service docker start

  - Clone the repo and set the environment variables in .env
    SQLALCHEMY_DATABASE_URI, AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION, S3_BUCKET_NAME, S3_ENDPOINT_URL, SECRET_KEY

  - Build and run your Docker container, map the port 5000 which Flask will be running on

  - Assign an Elastic IP to ensure consistent backend access.
    
Frontend Deployment

  - Install EB CLI and configure with aws configure using your IAM credentials.

  - Navigate to the frontend directory:
    cd frontend

  - Set the backend API URL in index.html

  - Initialize the Elastic Beanstalk project:
    eb init

  - Select the region, platform (Python), and application name.

  - Create and deploy the environment:
    eb create project-env --single

  - Once deployed, access the frontend
