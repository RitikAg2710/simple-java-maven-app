
# ✅ **README.md **

```markdown
# 🚀 CI/CD Pipeline for Java Maven Application using Jenkins & AWS EC2

This project demonstrates a complete **CI/CD pipeline** for a Java Maven application using Jenkins Declarative Pipeline.  
It showcases real-world DevOps practices including automated build, test, and deployment to AWS EC2.

---

## 🏗️ **Architecture Overview**

```

Developer → GitHub → Jenkins → Build (Maven) → Deployment → AWS EC2 → Application Running

```

---

## 🧩 **Tech Stack**

| Component | Technology |
|----------|------------|
| Source Control | GitHub |
| CI/CD Tool | Jenkins |
| Build Tool | Maven |
| Runtime | Java 17 |
| Deployment Server | AWS EC2 |
| Secure Access | SSH key stored in Jenkins Credentials |

---

## 📌 **Pipeline Features**

### ✔ Continuous Integration  
- Pull code from GitHub  
- Build with Maven  
- Optional unit tests  
- Clean, repeatable packaging process  

### ✔ Continuous Deployment  
- Secure SSH-based deployment to EC2  
- Upload JAR file to remote server  
- Auto-create deployment directory  
- Ready for app start automation  

### ✔ Security  
- SSH private key stored in Jenkins Credentials as `ec2-ssh-key`  
- No secrets written inside repo  
- No personal IPs, usernames, or key paths exposed  

---

## 📁 **Project Structure**

```

simple-java-maven-app/
│
├── src/main/java/com/mycompany/app/App.java
├── pom.xml
├── Jenkinsfile
└── README.md

````

---

## ⚙️ **Jenkins Pipeline**

```groovy
pipeline {
    agent any

    tools {
        maven 'Mvn'
    }

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '<your-ec2-public-ip>'
        APP_NAME  = 'simple-java-maven-app'
        REMOTE_DIR = '/home/ubuntu/deploy'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/<your-username>/simple-java-maven-app', branch: 'master'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to EC2') {
            steps {
                echo "Deploying $APP_NAME to EC2..."
                withCredentials([sshUserPrivateKey(credentialsId: 'ec2-ssh-key', keyFileVariable: 'EC2_KEY')]) {

                    sh 'ssh -o StrictHostKeyChecking=no -i "$EC2_KEY" $EC2_USER@$EC2_HOST "mkdir -p $REMOTE_DIR"'
                    sh 'scp -o StrictHostKeyChecking=no -i "$EC2_KEY" target/*.jar $EC2_USER@$EC2_HOST:$REMOTE_DIR/'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
````

---

## 🚀 **Run Application on EC2**

SSH into EC2:

```bash
ssh -i <your-key.pem> ubuntu@<your-ec2-public-ip>
```

Run JAR:

```bash
nohup java -jar /home/ubuntu/deploy/my-app-1.0-SNAPSHOT.jar > app.log 2>&1 &
```

View output:

```bash
tail -f app.log
```

---

## 📸 **Screenshots (Remove Sensitive Info Before Uploading)**

✔ Jenkins Pipeline Success
✔ Application Running (only show terminal output, never show AWS account info)

---

## 📝 **Conclusion**

This project demonstrates a real-world CI/CD pipeline using:

* Jenkins
* GitHub
* Maven
* AWS EC2
* Secure Credential Management

Perfect for learning DevOps, building your portfolio, and interview preparation.

---

## ⭐ Star this repo if you found it helpful!

```


**"Add diagrams"** or **"Add setup guide"**.
```
