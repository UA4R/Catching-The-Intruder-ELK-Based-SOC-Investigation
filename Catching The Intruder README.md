# Catching-The-Intruder-ELK-Based-SOC-Investigation

## Objectives

To stand up a working **ELK** based **SIEM** pipeline on **AWS infrastructure**, generate a real **SSH** brute force attack against it, and then run an  **SOC** style investigation on the resulting logs. 

### Skills Learnt

- Launching and configuring an AWS EC2 instance (instance sizing, AMI selection, EBS storage)

- Configuring AWS security groups; least-privilege inbound rules, scoping to specific IPs vs open internet. 

- Understanding the boundary between external (security group controlled) and internal (localhost) traffic.

- Remote server management entirely via SSH, no GUI.

- Managing services with systemctl (start, status, restart) and respecting service dependency order.

- Distinguishing malicious patterns (volume, timing, repetition, source) from legitimate background activity in real, mixed log data.

- Understanding SSH authentication mechanics well enough to deliberately manipulate them.

### Tools Used

![image_alt](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazonaws&logoColor=white)
![image_alt](https://img.shields.io/badge/Elasticsearch-005571?style=for-the-badge&logo=elasticsearch&logoColor=white)
![image_alt](https://img.shields.io/badge/Logstash-005571?style=for-the-badge&logo=logstash&logoColor=white)
![image_alt](https://img.shields.io/badge/Kibana-005571?style=for-the-badge&logo=kibana&logoColor=white)
![image_alt](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![image_alt](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

### Framework Used

![image_alt](https://img.shields.io/badge/Framework-NIST%20SP%20800--61-blue?style=for-the-badge)

### Steps

### 1)Preparation

- Go to AWS account (IAM) user account and launch instance from AWS EC2. After Launching instance you name the instance, in this case; catching the intruder elk. For Amazon Machine Image **(AMI)** choose Ubuntu 22.04. For instance type, I went with t3.xlarge 4vCPU 16 GB of RAM. For key pair type, I went with RSA due to its universal compatibility and its been used multiple times. Set the appropriate network settings ( I applied the principle of least privilege and added inbound rules for port 22 and port 5601). For storage **EBS Volume**, I went with 40GB gp3. For file systems option I went with none because for this project I'll be doing with one instance only not multiple. 

- 
