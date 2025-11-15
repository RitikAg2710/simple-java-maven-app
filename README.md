# simple-java-maven-app

Great — I’ll give you a **clean, professional, interview-ready README.md** for your repo **simple-java-maven-app**, in the same style as Vikas’ project but **better, clearer, and more structured**.

I’ll assume your project contains:

✔ Java–Maven app
✔ Jenkins CI/CD pipeline
✔ Automatic deployment to EC2
✔ Screenshots you showed

---

# ✅ **README.md (Copy–Paste Ready)**

Below is your complete README — **formatted, structured, and professional**.

---

```markdown
# 🚀 Automated CI/CD Pipeline for Java Maven Application using Jenkins & AWS EC2

This project demonstrates a complete **CI/CD pipeline** for a Java Maven application, built using **Jenkins Declarative Pipeline**.  
The pipeline automatically performs:

1. **Code Checkout** from GitHub  
2. **Build & Packaging** using Maven  
3. **Artifact Deployment** to AWS EC2  
4. **Remote Execution** of the Application on EC2

This project is ideal for **DevOps practice**, **interview preparation**, and **real-world Jenkins pipeline experience**.

---

## 🏗️ **Architecture Overview**

```

Developer → GitHub → Jenkins CI Pipeline → AWS EC2 Deployment → Application Running

```

---

## 🧩 **Tech Stack**

| Component | Technology Used |
|----------|-----------------|
| SCM | GitHub |
| CI Tool | Jenkins (Declarative Pipeline) |
| Build Tool | Maven |
| Runtime | Java 17 |
| Deployment Server | AWS EC2 (Ubuntu 24.04) |
| SSH Access | Jenkins Credentials |

---

## 📌 **Features Implemented**

### ✔ Continuous Integration  
- GitHub → Jenkins Webhook  
- Jenkins automatically triggers builds  
- Maven builds the Java app  
- Unit tests can be enabled/disabled

### ✔ Continuous Deployment  
- Jenkins securely connects to EC2 using SSH private key  
- Uploads `my-app-1.0-SNAPSHOT.jar`  
- Creates deployment directory on EC2  
- Optionally runs the app with `nohup`

### ✔ Secure Credential Handling  
- SSH key stored in Jenkins Credentials Manager  
- No hardcoding of secrets  
- Uses `sshUserPrivateKey` binding

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

## ⚙️ **Jenkinsfile (CI/CD Pipeline)**

```groovy
pipeline {
    agent any

    tools {
        maven 'Mvn'
    }

    environment {
        EC2_USER = 'ubuntu'
        EC2_HOST = '3.111.37.156'
        APP_NAME  = 'simple-java-maven-app'
        REMOTE_DIR = '/home/ubuntu/deploy'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                git url: 'https://github.com/RitikAg2710/simple-java-maven-app', branch: 'master'
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

## 🚀 **Running the Application on EC2**

SSH into EC2:

```bash
ssh -i April-lab.pem ubuntu@<EC2-IP>
```

Run JAR:

```bash
nohup java -jar /home/ubuntu/deploy/my-app-1.0-SNAPSHOT.jar > app.log 2>&1 &
```

Check logs:

```bash
tail -f app.log
```

Expected output:

```
Hello World!
```

---

## 📸 **Screenshots**

### ✔ Jenkins Pipeline Success

(Add your screenshot here)

### ✔ Application Running on EC2

(Add your screenshot here)

---

## 📝 **Conclusion**

This project successfully demonstrates:

* End-to-end CI/CD automation
* Secure deployment from Jenkins to EC2
* Real-world DevOps workflow
* Complete cloud-based Java app deployment

This mirrors real production pipelines and is ideal for **portfolio**, **resume**, and **interviews**.

---

## ⭐ If you like this project, give it a star on GitHub 😊
