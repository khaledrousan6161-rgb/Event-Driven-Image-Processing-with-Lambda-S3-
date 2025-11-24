# Event-Driven-Image-Processing-with-Lambda-S3-
About Event-Driven Image Processing with Lambda &amp; S3 is a serverless solution that resizes and transforms images automatically. When a file is uploaded to Amazon S3, an AWS Lambda function processes it and stores the output in another S3 bucket—scalable, cost-efficient, and fully automated.
# Serverless Image Handler using S3 & Lambda

A fully managed, serverless pipeline that detects when images are uploaded to an S3 bucket, processes them (e.g. resizes), and saves the results into another S3 bucket.

---

## Table of Contents

- [Overview](#overview)
- [System Design](#system-design)
- [Architecture Diagram](#architecture-diagram)
- [Setup & Deployment](#setup--deployment)
- [CI/CD Workflow](#cicd-workflow)
-  [License](LICENSE)

---

## Overview

This repository implements a serverless image handler that automatically processes incoming image uploads. It's perfect for web or mobile backends that need dynamic image resizing without worrying about infrastructure.

Key advantages include:

- **Auto-scaling** – responds elastically to fluctuating upload volumes.  
- **Cost-effective usage** – only pay per execution.  
- **Minimal maintenance** – no server upkeep required.

---

## System Design

This solution leverages AWS’ serverless stack to handle images seamlessly:

- **Client interface** for uploading images.  
- **(Optional) API Gateway** to expose an HTTP endpoint for uploads.  
- **S3 “source” bucket** to catch raw uploads that trigger processing.  
- **AWS Lambda function** that resizes or transforms the image.  
- **(Optional) AWS Step Functions** orchestrate multi-stage image pipelines.  
- **(Optional) DynamoDB** for storing metadata.  
- **S3 “destination” bucket** for processed images.

---

## Architecture Diagram

Here is the high-level architecture of the system:

![Architecture Diagram](Blank%20diagram1.png)

---

## Setup & Deployment

### Prerequisites

- An active AWS account  
- Installed and configured **AWS CLI**  
- **AWS SAM CLI** (to build & deploy Lambda)  
- **Docker** (for local Lambda container builds)

### Installation Steps

1. From project root, run:
   ```bash
   sam build


   name: Deploy on Push
   on:
   push:
    branches:
      - main

    jobs:
     deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - name: Install SAM CLI
        run: |
          python -m pip install --upgrade pip
          pip install aws-sam-cli
      - name: Configure AWS
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: ${{ secrets.AWS_REGION }}
      - name: Build Lambda
        run: sam build --use-container
      - name: Deploy Stack
        run: sam deploy --no-confirm-changeset --no-fail-on-empty-changeset
***

## License

Copyright Amazon.com, Inc. or its affiliates. All Rights Reserved.
SPDX-License-Identifier: Apache-2.0
