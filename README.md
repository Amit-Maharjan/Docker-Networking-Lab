# **Quote Server with Load Balancing using Docker Compose and HAProxy**

This project demonstrates how to create a **load-balanced multi-container application** using:
- **.NET Core C#** for the Quote Server
- **Python** for the Quote Client
- **HAProxy** as the load balancer
- **Docker Compose** to manage multi-container setup

---

## **Table of Contents**
1. [Project Structure](#project-structure)
2. [Prerequisites](#prerequisites)
3. [Setup Instructions](#setup-instructions)
4. [Build and Run the Server](#build-and-run-the-server)
5. [Build and Run the Client](#build-and-run-the-client)
6. [Testing Load Balancing](#testing-load-balancing)
7. [Sample Outputs](#sample-outputs)
8. [Troubleshooting](#troubleshooting)

---

## **Project Structure**
```
quote-server/
├── src/
│   └── QuoteServer.cs
├── Dockerfile
├── haproxy.cfg
├── docker-compose.yml

quote-client/
├── src/
│   └── quote_client.py
├── Dockerfile
```

---

## **Prerequisites**
- **Docker** and **Docker Compose** installed
- **.NET SDK** for local development ([Download](https://aka.ms/dotnet/download))
- **Python 3.x** for client development

---

## **Setup Instructions**

### 1. **Clone the repository**
```bash
git clone <repository-url>
cd quote-server
```

---

### 2. **Create Docker Network**
**Command:**
```bash
docker network create quote-network
```

**Sample Output:**
```
f4d5bce2f9e65ab7a340b8e84987d2b7692e9c9b273d5f8a93c3e0b1e6f9b914
```

---

## **Build and Run the Server**

### 1. **Build Docker Images using Docker Compose**
**Command:**
```bash
docker-compose build
```

**Sample Output:**
```
[+] Building 3/3
 ✔ quote-server-1 Built
 ✔ quote-server-2 Built
 ✔ quote-server-3 Built
```

---

### 2. **Start the Server Containers**
**Command:**
```bash
docker-compose up -d
```

**Sample Output:**
```
[+] Running 5/5
 ✔ Network quote-network Created
 ✔ Container quote-server-1 Started
 ✔ Container quote-server-2 Started
 ✔ Container quote-server-3 Started
 ✔ Container haproxy Started
```

---

## **Build and Run the Client**

### 1. **Build Client Docker Image**
**Command:**
```bash
docker build -t quote-client .
```

**Sample Output:**
```
[+] Building 0.6s (9/9) FINISHED
 ✔ quote-client Built
```

### 2. **Run the Client Container**
**Command:**
```bash
docker run -it --name quote-client --network quote-network quote-client
```

**Sample Output:**
```
Enter a quote number (1-10) or 0 to exit: 1

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: be6fe054-191f-43c1-a49c-ecb33f8c7359 ----- "Be the change you wish to see in the world." - Mahatma Gandhi

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: 1cf0c34d-eb4d-46e8-925f-a44b9f9947ad ----- "Be the change you wish to see in the world." - Mahatma Gandhi

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: c7315d8a-51a4-434f-95f7-1248e71a20aa ----- "Be the change you wish to see in the world." - Mahatma Gandhi

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: be6fe054-191f-43c1-a49c-ecb33f8c7359 ----- "Be the change you wish to see in the world." - Mahatma Gandhi

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: 1cf0c34d-eb4d-46e8-925f-a44b9f9947ad ----- "Be the change you wish to see in the world." - Mahatma Gandhi

Connecting to host.docker.internal port 13000

Received Quote: Quote Server with instance id: c7315d8a-51a4-434f-95f7-1248e71a20aa ----- "Be the change you wish to see in the world." - Mahatma Gandhi
```

---

## **Troubleshooting**

### 1. **Error: Network already exists**
**Solution:**
```bash
docker network rm quote-network
```

### 2. **Error: Connection Refused**
**Solution:**
- Ensure all containers are running:
```bash
docker ps
```
- Check HAProxy logs:
```bash
docker logs haproxy
```

---

## **Clean Up**
**Command:**
```bash
docker-compose down
docker network rm quote-network
```

**🚀 Happy Coding!**
