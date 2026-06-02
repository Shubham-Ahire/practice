
---


Flask Dockerized App

This is a small Python Flask application that has been containerized using Docker and Docker Compose. The project demonstrates how to set up a basic Flask app, dockerize it, manage it with Docker Compose, and automate deployment with a Jenkins CI/CD pipeline.

---

📦 Project Overview

This project contains:
- A Flask web application running on port 5000.
- A Dockerfile to create a Docker image for the Flask app.
- A docker-compose.yml file to simplify the building and running of containers.
- A Jenkinsfile to automate CI/CD processes.

By using Docker, this project ensures that the Flask app runs in an isolated environment with all dependencies required for production.

---

✅ Prerequisites

Before you begin, ensure you have the following installed:

- Docker: [Install Docker](https://www.docker.com/get-started)
- Docker Compose: Comes with Docker Desktop. On Linux, install it separately.

Verify installations:

```bash
docker --version
docker-compose --version
````

---

## 🧱 Project Structure

```bash
/your-project-directory
├── Dockerfile           # Defines the Docker image for the Flask app
├── app.py               # Flask application code
├── requirements.txt     # Python dependencies
├── docker-compose.yml   # Docker Compose configuration
├── Jenkinsfile          # Jenkins pipeline configuration (CI/CD)
└── README.md            # Project documentation
```

---

## 🚀 Setup and Installation

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/flask-docker-app.git
cd flask-docker-app
```

### 2. Build the Docker Image

```bash
docker-compose build
```

### 3. Run the Application with Docker Compose

```bash
docker-compose up -d
```

Once running, access the app at:
👉 `http://localhost:5000`

### 4. Check Container Status

```bash
docker-compose ps
```

### 5. Stop the Containers

```bash
docker-compose down
```

---

## 🌐 Application Output

Once the application is running, navigate to:

```
http://localhost:5000
```

You should see:

```
Hello, Dockerized World!
```

---

## ⚙️ Configuration Notes

* **Development Mode**: Currently runs in Flask’s development mode. For production, consider using `gunicorn` or another WSGI server.
* **Port Conflicts**: If port 5000 is busy, edit `docker-compose.yml`:

```yaml
ports:
  - "5001:5000"
```

---

## 🔁 CI/CD with Jenkins

This project includes a Jenkins-based Continuous Integration and Deployment (CI/CD) pipeline.

### 🧪 Jenkins Pipeline Overview

The pipeline defined in [`Jenkinsfile`](./Jenkinsfile) performs the following:

1. **Code Clone** – Clones the repo from GitHub.
2. **Code Build** – Builds the Docker image using Docker Compose.
3. **Push to DockerHub** – Authenticates with DockerHub and pushes the image.
4. **Code Deploy** – Starts the container using `docker-compose up -d`.

### 🔐 Jenkins Credentials Required

* A Jenkins agent labeled `worker` must be available.
* Docker and Docker Compose should be installed on that agent.
* DockerHub credentials must be configured in Jenkins:

  * **Credentials ID**: `secret`
  * **Type**: Username with Password

These credentials are injected using `withCredentials` in the pipeline.

### 🛠 Jenkinsfile Overview

```groovy
pipeline {
    agent { label 'worker' }

    stages {
        stage('Code clone') {
            steps {
                git url: "https://github.com/Shubham-Ahire/flask-dockerized-app.git", branch: "main"
            }
        }
        stage('Code build') {
            steps {
                sh 'docker compose build'
            }
        }
        stage('Push to Dockerhub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: "secret", 
                    usernameVariable: "dockerHubUser", 
                    passwordVariable: "dockerHubPass")]) {
                    
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag flask-app-web:latest ${env.dockerHubUser}/flask-app-web:latest"
                sh "docker push ${env.dockerHubUser}/flask-app-web:latest"
                }
            }
        }
        stage('Code Deploy') {
            steps {
                sh 'docker compose up -d'
            }
        }
    }
}
```

---

## 🚧 Additional Features (Optional)

You can extend this project by:

* ✅ Adding a database service (PostgreSQL, MySQL)
* ✅ Scaling using Docker Compose with replicas
* ✅ Integrating GitHub Webhooks to trigger Jenkins automatically
* ✅ Adding testing stages in the Jenkins pipeline

---

## 🛠 Troubleshooting

* **App not loading on port 5000**: Check if the port is in use, or modify the host port in `docker-compose.yml`.
* **Error with `docker-compose up`**: Check logs with:

```bash
docker-compose logs web
```

---

## 📤 Push to GitHub

### 1. Create a New Repository

Create a new repo on GitHub, e.g., `flask-docker-app`.

### 2. Initialize and Push

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/flask-docker-app.git
git push -u origin master
```
