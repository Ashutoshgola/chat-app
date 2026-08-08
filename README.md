# 💬 Real-Time Chat Application

A high-performance, full-stack **real-time messaging application** built with **Node.js, Express.js, Socket.IO, React, and MongoDB**.

The application is designed with a focus on **real-time event-driven architecture, secure authentication, low-latency WebSocket communication, state management, and containerized deployment using Docker and Nginx**.

---

## 📋 Table of Contents

- [✨ Features](#-features)
- [🛠️ Tech Stack](#️-tech-stack)
- [🏗️ Architecture & Deployment](#️-architecture--deployment)
- [🔧 Prerequisites](#-prerequisites)
- [📝 Environment Configuration](#-environment-configuration)
- [🚀 Getting Started & Local Setup](#-getting-started--local-setup)
  - [Option A: Docker Compose Setup](#option-a-quick-docker-compose-setup-recommended)
  - [Option B: Manual Containerized Setup](#option-b-manual-containerized-build)
- [🗺️ Future Roadmap](#️-future-roadmap)
- [📄 License](#-license)

---

## ✨ Features

- ⚡ **Real-Time Bidirectional Messaging**  
  Event-driven communication powered by **Socket.IO** for instant, low-latency messaging.

- 🔐 **Secure Authentication & Authorization**  
  JWT-based authentication with protected routes and secure session handling.

- 👤 **Profile & Presence Management**  
  Profile image management with real-time online/offline user presence updates.

- 🎨 **Modern Responsive UI**  
  Responsive and interactive interface built with **React, TailwindCSS, and DaisyUI**.

- 🗃️ **State Management**  
  Lightweight and efficient client-side state management using **Zustand**.

- 🐳 **Containerized Architecture**  
  Frontend, backend, and database services are containerized using **Docker and Docker Compose**.

- 🌐 **Nginx Reverse Proxy**  
  Nginx is used as a web server and reverse proxy for streamlined production deployment.

---

## 🛠️ Tech Stack

### Frontend

- React.js
- TailwindCSS
- DaisyUI
- Zustand

### Backend

- Node.js
- Express.js
- Socket.IO
- Mongoose

### Database

- MongoDB

### Authentication

- JSON Web Tokens (JWT)

### DevOps & Infrastructure

- Docker
- Docker Compose
- Nginx

---

## 🏗️ Architecture & Deployment

The application follows a containerized multi-service architecture:

```text
                         ┌───────────────────────┐
                         │     Browser Client    │
                         └───────────┬───────────┘
                                     │
                              HTTP / WebSocket
                                     │
                              ┌──────▼──────┐
                              │    Nginx    │
                              │  Port: 80   │
                              └──────┬──────┘
                                     │
                    ┌────────────────┴────────────────┐
                    │                                 │
             ┌──────▼──────┐                  ┌───────▼───────┐
             │   Frontend  │                  │    Backend     │
             │ React / Vite│                  │ Node / Express │
             │ Port: 5173  │                  │   Port: 5001   │
             └─────────────┘                  └───────┬────────┘
                                                       │
                                                ┌──────▼──────┐
                                                │   MongoDB   │
                                                │ Port: 27017 │
                                                └─────────────┘
```

### Services

| Service | Technology | Port |
|---|---|---:|
| Frontend | React / Vite | `5173` |
| Backend | Node.js / Express | `5001` |
| Database | MongoDB | `27017` |
| Reverse Proxy | Nginx | `80` |

---

## 🔧 Prerequisites

Before running the application, make sure you have the following installed:

- **Node.js** v18 or higher
- **Docker**
- **Docker Desktop**
- **Git**

---

## 📝 Environment Configuration

Create a `.env` file in the root directory or inside the `backend` directory, depending on your project configuration.

```env
# Database Configuration
MONGODB_URI=mongodb://root:admin@mongo:27017/chatApp?authSource=admin&retryWrites=true&w=majority

# JWT Authentication
JWT_SECRET=your_strong_jwt_secret_key

# Server Configuration
PORT=5001
NODE_ENV=production
```

> **Note:** For standard local development without Docker, use:

```env
MONGODB_URI=mongodb://localhost:27017/chatApp
```

### ⚠️ Security Note

Never commit your `.env` file or expose your `JWT_SECRET` in a public repository.

Make sure `.env` is included in your `.gitignore`:

```gitignore
.env
node_modules/
dist/
```

---

# 🚀 Getting Started & Local Setup

## Clone the Repository

```bash
git clone https://github.com/Ashutoshgola/full-stack_chatApp.git
cd full-stack_chatApp
```

---

## Option A: Quick Docker Compose Setup (Recommended)

Docker Compose allows you to start the **frontend, backend, MongoDB, and supporting services** together.

### 1. Build and Start the Containers

```bash
docker-compose up -d --build
```

### 2. Check Running Containers

```bash
docker-compose ps
```

### 3. View Application Logs

```bash
docker-compose logs -f
```

### 4. Access the Application

Open your browser and visit:

```text
http://localhost
```

The application should now be accessible through the Nginx reverse proxy.

---

## Option B: Manual Containerized Build

If you prefer to build and run each container individually, follow these steps.

### 1. Create a Docker Network

```bash
docker network create full-stack
```

---

### 2. Start MongoDB Container

```bash
docker run -d \
  -p 27017:27017 \
  --name mongo \
  --network=full-stack \
  mongo:latest
```

---

### 3. Build & Run Backend Container

Navigate to the backend directory:

```bash
cd backend
```

Build the Docker image:

```bash
docker build -t full-stack_backend .
```

Run the backend container:

```bash
docker run -d \
  --network=full-stack \
  --add-host=host.docker.internal:host-gateway \
  -p 5001:5001 \
  --env-file .env \
  --name backend \
  full-stack_backend
```

Return to the project root:

```bash
cd ..
```

---

### 4. Build & Run Frontend Container

Navigate to the frontend directory:

```bash
cd frontend
```

Build the Docker image:

```bash
docker build -t full-stack_frontend .
```

Run the frontend container:

```bash
docker run -d \
  --network=full-stack \
  -p 5173:5173 \
  --name frontend \
  full-stack_frontend
```

Return to the project root:

```bash
cd ..
```

---

## 🌐 Access Points

After successfully starting the containers, the application can be accessed through the following endpoints:

| Service | URL |
|---|---|
| 🌐 Frontend | `http://localhost:5173` |
| 🔌 Backend API | `http://localhost:5001` |
| 🗄️ MongoDB | `mongodb://localhost:27017` |
| 🌍 Nginx | `http://localhost` |

---

## 🐳 Useful Docker Commands

### View Running Containers

```bash
docker ps
```

### View Container Logs

```bash
docker logs <container-name>
```

Example:

```bash
docker logs backend
```

### Follow Backend Logs

```bash
docker logs -f backend
```

### Stop Containers

```bash
docker stop frontend backend mongo
```

### Remove Containers

```bash
docker rm frontend backend mongo
```

### Remove Docker Images

```bash
docker rmi full-stack_frontend full-stack_backend
```

---

## 🗺️ Future Roadmap

Planned improvements include:

- 📱 Improved mobile responsiveness
- 🔔 Push and message notifications
- 📎 File and media sharing
- 🎙️ Voice messages
- 👥 Group conversations
- 🔎 Advanced user and message search
- 🟢 Enhanced online/offline presence tracking
- ☁️ Cloud deployment
- 🔄 CI/CD pipeline integration
- 📊 Application monitoring and logging
- 🔒 Additional security hardening

---

## 📄 License

This project is open-source and available under the **MIT License**.

---

## 👨‍💻 Author

**Ashutosh Gola**

GitHub:  
https://github.com/Ashutoshgola

---

⭐ If you found this project useful, consider giving it a **star** on GitHub!
