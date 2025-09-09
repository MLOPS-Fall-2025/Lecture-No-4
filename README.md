# Lecture No. 4  
## Tutorial: Docker & Docker Compose  

---

##  Introduction  
So far, we deployed the Flask app directly to Azure.  

In real-world projects, developers often use **Docker** to package applications into containers.  
Containers ensure your app **runs the same way everywhere** – on your laptop, a teammate’s machine, or the cloud.  

**Docker Compose** helps manage multi-container applications (e.g., Flask + MongoDB + Redis).  

---

##  Example Scenario  
You have a Flask app that connects to MongoDB.  

- Without Docker → install Python, Flask, MongoDB, dependencies manually.  
- With Docker → everything is defined in `Dockerfile` & `docker-compose.yml`.  
- Anyone can run your app with a single command:  

```bash
docker-compose up
```

###  Step 1: Install Docker

Install Docker Desktop
 (Windows/Mac)

On Linux:

```
sudo apt update
sudo apt install docker.io docker-compose -y
```

Verify installation:

```
docker --version
docker-compose --version
```
---

### Step 2: Create a Dockerfile

In the root of your Flask project:

Dockerfile

```
# Base image
FROM python:3.10-slim

# Set working directory
WORKDIR /app

# Copy requirements and install
COPY requirements.txt requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Copy app source code
COPY . .

# Expose port
EXPOSE 5000

# Run the app
CMD ["python", "app.py"]
```
---
### Step 3: Build & Run the Container

Build image:
```
docker build -t flask-app .
```

Run container:
```
docker run -p 5000:5000 flask-app
```

Visit: http://localhost:5000

### Step 4: Add MongoDB with Docker Compose

Instead of running Flask and MongoDB separately, use docker-compose.yml.

docker-compose.yml
```
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    depends_on:
      - mongo
    environment:
      - MONGO_URI=mongodb://mongo:27017/flaskdb

  mongo:
    image: mongo:6.0
    container_name: mongo-db
    ports:
      - "27017:27017"
    volumes:
      - mongo-data:/data/db

volumes:
  mongo-data:
```
---
### Step 5: Run Multi-Container App
```
docker-compose up --build
```

- Flask app → http://localhost:5000

- MongoDB → mongodb://localhost:27017

Stop containers:
```
docker-compose down
```
---
### Step 6: Extend to Azure Deployment (Optional Preview)

Later, you’ll learn to push this Docker image to Azure Container Registry (ACR) and deploy it as a containerized Web App.

## Exercises
#### Exercise 1: Containerize Flask App

- Write a Dockerfile for your Flask app.

- Build and run it locally.

- Verify it runs on http://localhost:5000

#### Exercise 2: Introduce MongoDB

- Extend your Flask app to connect to MongoDB (e.g., store users).

- Use Docker Compose to run Flask + MongoDB.

- Verify data is stored in MongoDB.

#### Exercise 3: Breaking the Container

- Remove a line from requirements.txt.

- Rebuild the container and observe it fails.

- Fix requirements.txt → rebuild → confirm success.

#### Exercise 4: Add mongo-express

- Extend docker-compose.yml with mongo-express

- Access http://localhost:8081 to view MongoDB data.

## Key Takeaways

- Docker containers package your app with its dependencies.

- Docker Compose makes multi-service setups simple.

- You can run Flask + MongoDB + mongo-express with one command.

- Next, we’ll integrate Docker with GitHub Actions & Azure Deployment.