# project
Project: CI/CD Implementation for Java App using Jenkins, Docker, and Kubernetes
 Tools and Services Used
1.	GitHub – Source code management (repository hosting)
2.	Jenkins – CI/CD pipeline automation
3.	Maven – Java build tool
4.	Docker – Containerization platform
5.	Kubernetes (Minikube or local cluster) – Application deployment and orchestration

Step 1: Clone the GitHub Repository
1.	Go to GitHub and fork or create a new repository for your project.
2.	Clone the sample Java "Hello World" app (or use the provided one).
Command:
git clone https://github.com/YOUR_USERNAME/hello-world-java.git
3.	Navigate to the project directory.
Command:
cd hello-world-java
________________________________________
Step 2: Create Maven Build Setup
1.	Ensure the pom.xml file exists and is correct.
2.	To verify the build, use Maven:
Command:
mvn clean package
3.	The generated .jar file should be in the target/ folder.
________________________________________
Step 3: Set Up Jenkins
1.	Install Jenkins and configure it.
2.	Create a new Freestyle or Declarative Pipeline project.
Jenkins Job Configuration:
•	Source Code Management: Git
o	Repository URL: https://github.com/YOUR_USERNAME/hello-world-java.git
•	Build Step:
o	Execute Shell Command: mvn clean package
•	Post-build Action:
o	Archive Artifacts: target/*.jar
________________________________________
Step 4: Dockerize the Application
1.	Create a file named Dockerfile in the project root directory.
2.	Use the following Dockerfile contents:

FROM openjdk:17
COPY target/*.jar app.jar
ENTRYPOINT ["java", "-jar", "app.jar"]
3.	Build the Docker image.
Command:
docker build -t hello-world-app .
4.	Run the Docker container locally to test.
Command:
docker run -p 8080:8080 hello-world-app
5.	Test by accessing: http://localhost:8080 (or check logs to verify output).
________________________________________
Step 5: Push Code and Dockerfile to GitHub
1.	Add all changes:
Commands:

git add .
git commit -m "Added Dockerfile and Maven setup"
git push origin main
________________________________________
Step 6: Kubernetes Deployment (Minikube or Local Cluster)
1.	Start Minikube:
Command:
minikube start
2.	Create a deployment.yaml file:

apiVersion: apps/v1
kind: Deployment
metadata:
  name: hello-world-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: hello-world
  template:
    metadata:
      labels:
        app: hello-world
    spec:
      containers:
      - name: hello-world
        image: hello-world-app:latest
        imagePullPolicy: Never
        ports:
        - containerPort: 8080
3.	Create a service.yaml file:

apiVersion: v1
kind: Service
metadata:
  name: hello-world-service
spec:
  type: NodePort
  selector:
    app: hello-world
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
    nodePort: 30036
4.	Apply the deployment and service:
Commands:
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
5.	Verify the pods and services:
Commands:
kubectl get pods
kubectl get svc
6.	To access the app:
Command:
minikube service hello-world-service
