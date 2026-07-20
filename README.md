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
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/1.png)

- For the next step, I connected my instance to my terminal.


- For the sake of this exercise I enabled password authentication so that the brute force can be seen because without it it wouldn't. On real world mechanics, this would be highly discouraged.
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/2.png)

- I installed Elasticsearch first due to its indepent state as I couldn't start with logstash. After I installed elasticsearch, I started the application.
<p align="center">
  <img src="https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/3.png" width="48%" />
  <img src="https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/4.png" width="48%" />
</p>

- Next step I took was installing logstash and it came in successfully.
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/5.png)

- Next step was install kiabana and it installed successfully.
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/6.png)

### 2)Detection & Analysis

- For this I initiated an ssh brute force after making sure everything was running smoothly. The brute force was a looping python script trying to connect. 
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/7.png)

- I went ahead to kibana to confirm log flow. I initiated a KQL query to see how many "failed" timestamps we got and from that it would be determined the brute force was initiated successfully and its logs we queried properly. The logs showed were slightly bit inflated as the original outcome expected was 6 and it gave out 12, which was likely due to those same 6-7 failed login lines likely got indexed into Elasticsearch multiple times hence 12 instead of 6. 
<p align="center">
  <img src="https://raw.githubusercontent.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/main/Catching%20the%20Intruder%20screenshots/8.png" width="48%" />
  <img src="https://raw.githubusercontent.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/main/Catching%20the%20Intruder%20screenshots/9.png" width="48%" />
</p>

- Discovered one little hiccup, duplicate ingestion caused by sincedb_path, resolved via clean re-index. Resolved it by editing the grok filter so the output should be coming in cleanly, 6, exactly as foresaw. 
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/10.png)

### 3)Containment, Eradication & Recovery

- The SSH brute force attack happened due to earlier password enablement so I accessed my terminal and switched the password authentication from a yes to a no.
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/11.png)

### 4)Post Incident Activity

### Lessons Learnt

- SSH brute force is the most common attack on cloud infrastructure exposed to the internet.

- SIEM detected 100% of attack attempts through real time auth.log monitoring

- Password authentication should never been enabled on production cloud servers . Key based authorization only.

- Principle of least privilege is critical for SOC analyst be it cloud infrastructure or anything else.

### Conclusion

The incident demonstrates the critical importance of continuous monitoring and SIEM deployment in cloud environments. Following NIST SP 800-61 (Incident & Response Framework), the cloud SOC lab successfully detected, analyzed, contained and documented a real world SSH brute force attack pattern using ELK ingesting live logs from an AWS EC2 instance. Finally as a project wrap up, the instance was immediately terminated successfully for cost efficiency in terms of the cloud environment. 
![image_alt](https://github.com/UA4R/Catching-The-Intruder-ELK-Based-SOC-Investigation/blob/main/Catching%20the%20Intruder%20screenshots/12.1.png?raw=true)
