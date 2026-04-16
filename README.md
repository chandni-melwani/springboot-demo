**1. Create Spring Boot Application**

**👉 Using Spring Initializr**

- Project: Maven
- Language: Java
- Spring Boot: Stable version (e.g., 3.5.x)
- **Group**`com.example`
- **Artifact**`demo-app`
- **Name**`demo-app`
- **Package Name**`com.example.demo_app`
- **Packaging :** Jar
- Dependencies: Spring Web

**📁 Project Structure**

`src/main/java/com/example/demo_app/
    DemoAppApplication.java`

### Option A: Using `application.properties`

Navigate to:

```
src/main/resources/application.properties
```

Add:

```
server.port=8081
```

Rebuild and run:

```
mvn clean package
java-jar target/demo-app-0.0.1-SNAPSHOT.jar
```

### Option B: Run via Command Line

```
java-jar target/demo-app-0.0.1-SNAPSHOT.jar--server.port=8081
```

**4. Docker Setup**

## **📄 Create Dockerfile (Project Root)**

FROM eclipse-temurin:17-jdk-jammy
COPY target/demo-app-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]

**Build Docker Image**

```
docker build -t demo-app .
```

---

## **▶️ Run Container**

docker run -p 9095:8080 demo-app

### Run the Container

```
docker run -p 9095:9095 demo-app
```

---

## 🌐 Verify the Application

Open your browser and navigate to:

```
http://localhost:9095/hello
```

**Expected Output:**

```
Hello DevOps!
```

## 🚀 Step-by-Step: Push Docker Image to Docker Hub

### 🔐 1. Log in to Docker Hub

```
docker login
```

Enter your Docker Hub username and password when prompted.

---

### 🏷️ 2. Tag the Docker Image

Tag your local image (`demo-app`) with your Docker Hub repository name.

```
docker tag demo-app chandni2308/demo-app
```

To specify a version, use:

```
docker tag demo-app chandni2308/demo-app:1.0
```

---

### 📤 3. Push the Image

Push the tagged image to Docker Hub.

```
docker push chandni2308/demo-app
```

Or, if using a version tag:

```
docker push chandni2308/demo-app:1.0
```

---

### 📦 4. Verify the Upload

Visit your Docker Hub repository:

```
https://hub.docker.com/r/chandni2308/demo-app
```

You should see your uploaded image listed there.

---

## 🧪 5. Test the Image from Anywhere

To confirm that the image works globally, pull and run it:

```
docker pull chandni2308/demo-app
docker run-p9095:8080 chandni2308/demo-app
```

Then open:

```
http://localhost:9095/hello
```

Expected output:

```
Hello DevOps!
```

---

### 

### ✅ 1. Verify the Image Exists Locally

Check whether the Docker image is available on your system:

```
docker images
```

## 🌐 Verify on Docker Hub

Open your repository in a browser:

**https://hub.docker.com/r/chandni2308/demo-app**

You should see the following tags:

- `latest`
- `1.0`

Ensure the repository visibility is set to **Public** if you want anyone to pull the image.

---

## 📥 Test by Pulling the Image

To confirm everything works globally, pull the image:

```
docker pull chandni2308/demo-app:latest
docker pull chandni2308/demo-app:1.0
```

---

## ▶️ Run the Container

Based on your previous configuration, use the appropriate port mapping.

If your Spring Boot application runs on **port 8080** inside the container (recommended):

```
docker run-p9095:9095 chandni2308/demo-app:latest
```

Then open:

```
http://localhost:9095/hello
```

## ✅ Step 1: Update the Deployment Configuration

Ensure your `deployment.yaml` specifies the correct container port and Docker image.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: demo-app
        image: chandni2308/demo-app:1.0
        ports:
        - containerPort: 9095
```

Apply the updated deployment:

```
kubectl apply-f deployment.yaml
```

If it was previously deployed, restart it to apply changes:

```
kubectl rolloutrestart deployment demo-app
```

---

## 🌐 Step 2: Expose the Deployment as a NodePort Service

Since you are not using a `service.yaml`, expose it directly:

```
kubectl expose deployment demo-app \
--type=NodePort \
--port=9095 \
--target-port=9095
```

This creates a service that routes external traffic to your application.

---

## 🔍 Step 3: Verify the Deployment and Service

Check the pods:

```
kubectlget pods
```

Check the services:

```
kubectlget services
```

Example output:

```
NAME         TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
demo-app     NodePort   10.96.0.12     <none>        9095:30007/TCP   20s
```

---

## 🌍 Step 4: Access the Application

Open your browser using the assigned NodePort:

```
http://localhost:<NodePort>/hello
```

For example:

```
http://localhost:30007/hello
```

---

## 🔄 Alternative: Use Port Forwarding

If NodePort access does not work:

```
kubectl port-forwardservice/demo-app9095:9095
```

Then open:

```
http://localhost:9095/hello
```

---

# 🚀 CI/CD Pipeline: Spring Boot → Docker → Kubernetes → Jenkins

## 📌 Step 1: Access the Application via NodePort

After deploying the application to Kubernetes, verify the services:

```
kubectlget services
```

Locate the `demo-app` service. Example output:

```
NAME       TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
demo-app   NodePort   10.96.180.45    <none>        9095:31534/TCP   23m
```

Open the application in your browser:

```
http://localhost:31534/hello
```

---

## 🔄 Step 2: Alternative – Use Port Forwarding

If NodePort access does not work, use port forwarding.

### ▶️ Command

### ▶️ Command

```
kubectl port-forwardservice/demo-app9095:9095
```

### 🌐 Access the Application

```
http://localhost:9095/hello
```

---

## 📦 Step 3: Verify Kubernetes Resources

### Check Pods

```
kubectlget pods
```

### Check Deployments

```
kubectlget deployments
```

### Check Services

```

```

```
kubectlget services
```

---

## 🐳 Step 4: Verify Docker Image on Docker Hub

```
docker pull chandni2308/demo-app:1.0
```

Run the container locally (optional verification):

```
docker run-p9095:9095 chandni2308/demo-app:1.0
```

Access:

```
http://localhost:9095/hello
```

---

## 🔧 Step 5: Push Code to GitHub

```

```

```
git init
git add .
git commit-m"Spring Boot CI/CD with Docker, Kubernetes, and Jenkins"
git branch-M main
git remote add origin https://github.com/chandni-melwani/springboot-demo.git
git push-u origin main
```

---

## 🤖 Step 6: Jenkins Pipeline Execution

Trigger the pipeline manually from Jenkins:

1. Open **Jenkins Dashboard**.
2. Select **springboot-demo-pipeline**.
3. Click **Build Now**.
4. Monitor progress in **Console Output**.

### Expected Pipeline Stages

- ✔ Checkout Code
- ✔ Build with Maven
- ✔ Build Docker Image
- ✔ Tag Docker Image
- ✔ Push Docker Image
- ✔ Deploy to Kubernetes

---

## 🔐 Step 7: Configure Docker Hub Credentials in Jenkins

| Field | Value |
| --- | --- |
| Username | `chandni2308` |
| Password | Docker Hub password or access token |
| Credential ID | `dockerhub-creds` |

Navigate to:

**Manage Jenkins → Credentials → Global → Add Credentials**

---

## 🔗 Step 8: Configure GitHub Webhook

Add the following webhook to your GitHub repository:

```
http://localhost:8080/github-webhook/
```

If Jenkins is not publicly accessible, use ngrok:

```
ngrok http8080
```

Use the generated URL:

```
https://<ngrok-id>.ngrok.io/github-webhook/
```

Set:

- **Content Type:** `application/json`
- **Event:** Just the **Push** event

---

## 📄 Step 9: Deployment Configuration (`deployment.yaml`)

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: demo-app
        image: chandni2308/demo-app:1.0
        ports:
        - containerPort: 9095
```

Apply the deployment:

```
kubectl apply-f deployment.yaml
```

Expose the service:

```
kubectl expose deployment demo-app--type=NodePort--port=9095--target-port=9095
```

---

## 🧹 Step 10: Clean Up Failed or Unused Resources (Optional)

```
kubectl delete deployment java-app
kubectl deleteservice java-service
```

---

## 📊 Final Verification Commands

```
kubectlget pods
kubectlget services
kubectlget deployments
docker images
```

---

## 🌐 Final Application URLs

| Access Method | URL |
| --- | --- |
| NodePort | `http://localhost:31534/hello` |
| Port Forwarding | `http://localhost:9095/hello` |
| Docker (Local) | `http://localhost:9095/hello` |

---

## ✅ Project Summary

| Component | Status |
| --- | --- |
| Spring Boot Application | ✅ Running |
| Docker Image | ✅ Built and Pushed |
| Docker Hub Repository | ✅ chandni2308/demo-app |
| Kubernetes Deployment | ✅ Successful |
| Kubernetes Service | ✅ NodePort Exposed |
| Jenkins CI/CD Pipeline | ✅ Configured |
| GitHub Integration | ✅ Enabled |
| GitHub Webhook | ✅ Automated Builds |

---

### 🎉 Conclusion

You have successfully implemented a complete **CI/CD pipeline** using:

- **Spring Boot**
- **Docker**
- **Kubernetes**
- **Jenkins**
- **GitHub**
- **Docker Hub**

**Confidence Level: 100%** – All steps align with your executed workflow and environment.

---

**🚀 DevOps Pipeline: Spring Boot → Maven → Docker → Kubernetes → Jenkins**

**📌 Overview**

This project demonstrates a complete **CI/CD pipeline** using:

- Spring Boot (Application)
- Maven (Build Tool)
- Docker (Containerization)
- Kubernetes (Deployment)
- Jenkins (Automation)

---

**🧩 1. Create Spring Boot Application**

**👉 Using Spring Initializr**

- Project: Maven
- Language: Java
- Spring Boot: Stable version (e.g., 3.5.x)
- Dependencies: Spring Web

**📁 Project Structure**

```
src/main/java/com/example/demo_app/
    DemoAppApplication.java
```

---

**🧑‍💻 2. Add Controller**

Create `HelloController.java` inside:

```
src/main/java/com/example/demo_app/
```

```
@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello DevOps!";
    }
}
```

---

**🔨 3. Build Project using Maven**

```
mvn clean install
```

**📦 Output**

```
target/demo-app-0.0.1-SNAPSHOT.jar
```

**▶️ Run Locally**

```
java -jar target/demo-app-0.0.1-SNAPSHOT.jar
```

Open:

```
http://localhost:8080/hello
```

---

**🐳 4. Docker Setup**

**📄 Create Dockerfile (Project Root)**

```
FROM openjdk:17
COPY target/demo-app-0.0.1-SNAPSHOT.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
```

---

**🏗️ Build Docker Image**

```
docker build -t demo-app .
```

---

**▶️ Run Container**

```
docker run -p 8080:8080 demo-app
```

---

**🚀 5. Push Docker Image**

**🔐 Login**

```
docker login
```

**🏷️ Tag**

```
docker tag demo-app keerthiswaran/demo-app
```

**📤 Push**

```
docker push keerthiswaran/demo-app
```

---

**☸️ 6. Kubernetes Deployment**

**📄 deployment.yaml**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: demo-app
spec:
  replicas: 2
  selector:
    matchLabels:
      app: demo-app
  template:
    metadata:
      labels:
        app: demo-app
    spec:
      containers:
      - name: demo-app
        image: keerthiswaran/demo-app
        ports:
        - containerPort: 8080
```

---

**▶️ Apply Deployment**

```
kubectl apply -f deployment.yaml
```

---

**🌐 Expose Service**

```
kubectl expose deployment demo-app --type=NodePort --port=8080
```

---

**🔍 Check Status**

```
kubectl get pods
kubectl get services
```

---

**🤖 7. Jenkins Pipeline**

**📄 Jenkinsfile**

pipeline { agent any

```
environment {
    IMAGE_NAME = "keerthiswaran/demo-app"
}

stages {

    stage('Checkout Code') {
        steps {
            git 'https://github.com/YOUR-USERNAME/YOUR-REPO.git'
        }
    }

    stage('Build with Maven') {
        steps {
            bat 'mvn clean install'
        }
    }

    stage('Build Docker Image') {
        steps {
            bat 'docker build -t demo-app .'
        }
    }

    stage('Tag Docker Image') {
        steps {
            bat 'docker tag demo-app %IMAGE_NAME%'
        }
    }

    stage('Push Docker Image') {
        steps {
            bat 'docker push %IMAGE_NAME%'
        }
    }

    stage('Deploy to Kubernetes') {
        steps {
            bat 'kubectl apply -f deployment.yaml'
        }
    }
}
```

}

---

**🔁 CI/CD Flow**

```
Code → GitHub → Jenkins → Maven → Docker → Docker Hub → Kubernetes
```

---

**⚙️ Prerequisites**

- Java 17
- Maven
- Docker
- Kubernetes (Minikube / Docker Desktop)
- Jenkins

---

**🧠 Key Concepts**

- **Maven** → Builds project (.jar)
- **Docker** → Packages application
- **Kubernetes** → Runs & manages containers
- **Jenkins** → Automates pipeline

---

**💥 Important Notes**

- Dockerfile & Kubernetes YAML → Created once
- Pipeline → Runs for every code change
- Use correct Docker image name everywhere

---

**🎯 Conclusion**

This pipeline automates:

- Build → Test → Package → Deploy

Making application deployment:

- Faster ⚡
- Scalable 📈
- Reliable 🔁

---

**🧠 Quick Revision**

👉 Spring Boot → Maven → Docker → Kubernetes → Jenkins

---

🔥 **You are now ready to build any DevOps pipeline!**

## **About**

*No description, website, or topics provided.*

### **Resources**

[Readme](https://github.com/Keerthiswaran27/springboot-demo#readme-ov-file)
