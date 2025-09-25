# AWS Node.js Application Monitoring

## Overview
This project demonstrates how to deploy a simple Node.js application on an AWS EC2 instance and monitor it using Amazon CloudWatch. The setup includes:

- EC2 instance creation
- Node.js application deployment
- Logging application output to a file
- Installing and configuring the CloudWatch Agent
- Monitoring VM metrics (CPU, Memory, Disk) and application logs

---

## Table of Contents
1. [Prerequisites](#prerequisites)  
2. [Steps](#steps)  
   - [1. Launch EC2 Instance](#1-launch-ec2-instance)  
   - [2. Deploy Node.js Application](#2-deploy-nodejs-application)  
   - [3. Configure Logging](#3-configure-logging)  
   - [4. Install CloudWatch Agent](#4-install-cloudwatch-agent)  
   - [5. Configure CloudWatch Agent](#5-configure-cloudwatch-agent)  
   - [6. Verify Metrics and Logs](#6-verify-metrics-and-logs)  
3. [Blockers and Solutions](#blockers-and-solutions)  
4. [GitHub Setup](#github-setup)  
5. [References](#references)

---

## Prerequisites
- AWS account with permissions to create EC2 instances and IAM roles  
- Node.js installed (version 20.x)  
- Git installed on EC2  

---

## Steps

### 1. Launch EC2 Instance
1. Go to AWS Console → EC2 → Launch Instance  
2. Choose **Ubuntu 24.04 LTS**  
3. Configure security group: open ports `22` (SSH), `80` (HTTP), `443` (HTTPS)  
4. Launch and note **Public IP**

---

### 2. Deploy Node.js Application
```bash
sudo apt update && sudo apt upgrade -y
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs git
mkdir myapp && cd myapp
nano app.js
---

### 3. Configure Logging

Create a log file and set proper permissions:

```bash
sudo touch /var/log/myapp.log
sudo chown ubuntu:ubuntu /var/log/myapp.log
sudo chmod 664 /var/log/myapp.log

---

## 4. Install CloudWatch Agent

Download and install the CloudWatch Agent:

```bash
wget https://s3.amazonaws.com/amazoncloudwatch-agent/ubuntu/amd64/latest/amazon-cloudwatch-agent.deb
sudo dpkg -i ./amazon-cloudwatch-agent.deb || sudo apt -f install -y

---

### 5. Configure CloudWatch Agent

This section explains how to configure the **AWS CloudWatch Agent** on your EC2 instance to collect metrics and logs.
1. Attach an IAM role with CloudWatchAgentServerPolicy to the EC2 instance.
2. Create CloudWatch agent JSON config:
3. Start agent:
Commands i used to restart the Agent: 
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl \
  -a fetch-config -m ec2 \
  -c file:config/amazon-cloudwatch-agent.json -s
sudo systemctl start amazon-cloudwatch-agent
sudo systemctl enable amazon-cloudwatch-agent

### 6. Verify Metrics and Logs

<img width="1920" height="912" alt="Metrics_monitoring" src="https://github.com/user-attachments/assets/5da70b4e-c2b8-49af-8be3-2a6d9f5fa843" />
<img width="1920" height="912" alt="Log_monitoring" src="https://github.com/user-attachments/assets/ed37e8c9-e2ae-4e09-bcdf-f217ac5fc3e6" />




