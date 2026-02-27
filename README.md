# 📌 Automated Deployment of Static Website on AWS S3 with CI/CD using Jenkins & GitHub

## 📌 Project Description
In this project I built a CI/CD pipeline that automatically deploys a static website to AWS S3 whenever I push code to GitHub. I set up Jenkins on an AWS EC2 instance which acts as the automation server. When I make any change to the website and push it to GitHub, Jenkins automatically picks up the change and deploys the updated website to S3. The website is hosted on S3 static website hosting and is publicly accessible via a URL. This project helped me understand how real world deployment automation works using AWS and Jenkins.

---

## 🏗️ Architecture Flow

1.Developer pushes code to GitHub

2.Jenkins pulls the latest code

3.Jenkins uses AWS CLI to upload files to S3

4.S3 hosts the website publicly

5.Website becomes live immediately

<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/358219da-4da6-4a2a-bc60-51a326b9425a" />


---

## 🛠️ Tech Stack
| Tool | Purpose |
|---|---|
| GitHub | Storing the website source code |
| Jenkins | Automating the deployment process |
| AWS EC2 | Server where Jenkins is running |
| AWS S3 | Hosting the static website |
| AWS IAM | Managing secure access to S3 |
| AWS CLI | Running S3 commands from EC2 |
| Linux | Setting up and configuring the server |

---

## ⚙️ Prerequisites
- AWS Account
- GitHub Account
- EC2 Instance (Amazon Linux 2023, t2.medium)
- Jenkins installed on EC2
- AWS CLI configured on EC2
- S3 Bucket with static website hosting enabled

---

## 📁 Project Structure
```
static-website-cicd/
├── index.html       # Main website file
├── style.css        # Website styling
├── Jenkinsfile      # Jenkins pipeline script
└── README.md        # Project documentation
```

---

## 🚀 Step by Step Setup

### Step 1: Launch EC2 Instance
- AMI: Amazon Linux 2023
- Instance Type: t2.medium
- Security Group Ports: 22, 80, 8080

### Step 2: Install Jenkins on EC2
```bash
sudo dnf update -y
sudo dnf install java-17-amazon-corretto -y
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo dnf install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
```
<img width="1920" height="1080" alt="EC2 server" src="https://github.com/user-attachments/assets/cbe8b82e-16d5-4f84-9bc3-b355e26d1619" />

<img width="1920" height="1080" alt="server" src="https://github.com/user-attachments/assets/2b7ff150-989f-4a06-960c-a8856e9a5cd6"

### Step 3:🪣 Creating the S3 Bucket

Open AWS Console → S3

Create bucket → name it: new-hepsi-bucket

Disable “Block Public Access”

Enable Static Website Hosting

Upload HTML files to test manually

<img width="1920" height="1080" alt="s3 bucket running" src="https://github.com/user-attachments/assets/ba185830-c92c-41a6-8386-41efb60d3a1d" />

<img width="1920" height="1080" alt="s3" src="https://github.com/user-attachments/assets/3e5d1480-f31c-493f-ad9d-ecfe08c794e0" />

<img width="1920" height="1080" alt="s3 bucket" src="https://github.com/user-attachments/assets/85b44135-1e9d-4668-a66a-b02751bdaa1f" />

#### website URL:

👉 http://new-hepsi-bucket.s3-website.ap-south-1.amazonaws.com


### Step 4: Configure AWS Access
- Created IAM role with AmazonS3FullAccess permission
- Attached the role to EC2 instance
- Configured AWS CLI on EC2 with credentials
- Tested connection using aws s3 ls command

  <img width="1920" height="1080" alt="server iam ttached" src="https://github.com/user-attachments/assets/a5cdb2f2-3d5c-47db-8f06-df3e7bb9663f" />



### Step 5: Create Jenkins Pipeline
- Installed GitHub Integration plugin in Jenkins
- Created a new Pipeline job
- Connected GitHub repository to Jenkins
- Added Jenkinsfile to the repo with deploy script

### Step 7: Configure GitHub Webhook
- Went to GitHub repo Settings → Webhooks
- Added Jenkins webhook URL
- Set trigger to push event
- Jenkins now triggers automatically on every push

  <img width="1920" height="1080" alt="webhook url" src="https://github.com/user-attachments/assets/e9a659f0-70ab-452f-ad07-b60b1677e32d" />


---

## 📝 Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ArlagaddaHepseeba-git/static-website-cicd.git'
            }
        }
        stage('Deploy to S3') {
            steps {
                sh 'aws s3 sync . s3://new-hepsi-bucket --delete --exclude ".git/*" --exclude "Jenkinsfile"'
            }
        }
    }
    post {
        success {
            echo 'Website deployed successfully to S3!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
```

<img width="1920" height="1080" alt="build sucess" src="https://github.com/user-attachments/assets/406883ab-b49c-4e74-a6b6-6ba0e064cdce" />

<img width="1920" height="1080" alt="console build sucess" src="https://github.com/user-attachments/assets/507b280c-e51e-48f2-9009-723f4e55c2b4" />

<img width="1920" height="1080" alt="deployment sucess" src="https://github.com/user-attachments/assets/39f8d319-382e-42a6-964b-f4512e2bfd31" />


---

## 🌐 Live Website
- Website is hosted on AWS S3 Static Website Hosting
- URL: `http://new-hepsi-bucket.s3-website.ap-south-1.amazonaws.com`

  <img width="1920" height="1080" alt="final website output" src="https://github.com/user-attachments/assets/b25e30e5-4161-408c-99e4-45c7362edf47" />

---


## ✅ What I Learned
- How to set up Jenkins on AWS EC2 from scratch
- How to write a Jenkins pipeline using Jenkinsfile
- How to host a static website on AWS S3
- How to configure IAM roles and AWS CLI for secure access
- How CI/CD automation works in real world projects

---

## 👩‍💻 Author
- **Name:** Arlagadda Hepseeba
- **GitHub:** https://github.com/ArlagaddaHepseeba-git
- **Skills:** Linux, Git, GitHub, Jenkins, Docker, AWS EC2, IAM, S3
