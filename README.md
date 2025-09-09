# Lecture No. 4  
## Tutorial: Docker & Docker Compose  

---

## 📌 Introduction  
So far, we deployed the Flask app directly to Azure.  

In real-world projects, developers often use **Docker** to package applications into containers.  
Containers ensure your app **runs the same way everywhere** – on your laptop, a teammate’s machine, or the cloud.  

**Docker Compose** helps manage multi-container applications (e.g., Flask + MongoDB + Redis).  

---

## 🚀 Example Scenario  
You have a Flask app that connects to MongoDB.  

- Without Docker → install Python, Flask, MongoDB, dependencies manually.  
- With Docker → everything is defined in `Dockerfile` & `docker-compose.yml`.  
- Anyone can run your app with a single command:  

```bash
docker-compose up
