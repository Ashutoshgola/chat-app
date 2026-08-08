💬 Real-Time Chat Application
A high-performance, full-stack real-time messaging application containerized with Docker and built with Node.js, Express, Socket.IO, React, and MongoDB. Designed with a focus on real-time event-driven architecture, secure access control, low-latency WebSocket communication, and streamlined containerized deployment.
📋 Table of Contents
⚬	Features
⚬	Tech Stack
⚬	Architecture & Deployment
⚬	Prerequisites
⚬	Environment Configuration
⚬	Getting Started & Local Setup
⚬	Option A: Quick Docker Compose Setup
⚬	Option B: Manual Containerized Build
⚬	Future Roadmap
⚬	License
✨ Features
⚬	⚡ Real-Time Bidirectional Messaging: Event-driven architecture powered by Socket.IO for instant communication.
⚬	🔐 Secure Authentication & Authorization: Session security and route protection built using JSON Web Tokens (JWT).
⚬	👤 Profile & Presence Management: Support for profile image management and real-time online/offline user status updates.
⚬	🎨 Modern Responsive UI: Designed with React, TailwindCSS, and DaisyUI for an intuitive, dynamic user interface.
⚬	🗃️ State Management: Clean, lightweight client-side state handling utilizing Zustand.
⚬	🐳 Containerized Architecture: Fully containerized setup with Docker and Nginx reverse proxy for production-grade deployments.
🛠️ Tech Stack
⚬	Backend: Node.js, Express.js, MongoDB (Mongoose), Socket.IO
⚬	Frontend: React, TailwindCSS, DaisyUI, Zustand
⚬	DevOps & Infrastructure: Docker, Docker Compose, Nginx (Web Server / Reverse Proxy)
⚬	Authentication: JWT (JSON Web Tokens)
🏗️ Architecture & Deployment
   +-------------------------------------------------------+
   |                    Browser Client                     |
   +-------------------------------------------------------+
                            |
                     Nginx (Port 80)
                            |
         +------------------+------------------+
         |                                     |
  Frontend Container                    Backend Container 
 (React / Port 5173)                   (Express / Port 5001)
                                               |
                                        MongoDB Container
                                           (Port 27017)

🔧 Prerequisites
Before starting, ensure you have the following installed:
⚬	Node.js (v18 or higher)
⚬	Docker & Docker Desktop
⚬	Git
📝 Environment Configuration
Create a .env file in the root directory (or backend directory) with the following variables:
# Database Configuration
MONGODB_URI=mongodb://root:admin@mongo:27017/chatApp?authSource=admin&retryWrites=true&w=majority

# JWT Authentication
JWT_SECRET=your_strong_jwt_secret_key

# Server Environment
PORT=5001
NODE_ENV=production

💡 Note: For standard local development without Docker, update MONGODB_URI to mongodb://localhost:27017/chatApp.
🚀 Getting Started & Local Setup
Clone the Repository
git clone https://github.com/Ashutoshgola/full-stack_chatApp.git
cd full-stack_chatApp

Option A: Quick Docker Compose Setup (Recommended)
Run the entire microservice stack (Frontend, Backend, Database) using Docker Compose:
# Build and run containers in detached mode
docker-compose up -d --build

Access the application in your browser at:
⚬	Frontend Application: http://localhost
⚬	Verify Backend & Logs: docker-compose logs -f
Option B: Manual Containerized Build
If you prefer building and running containers individually using custom Docker networks:
	1.	Create a shared Docker network:
docker network create full-stack

	2.	Start the MongoDB Container:
docker run -d -p 27017:27017 --name mongo --network=full-stack mongo:latest

	3.	Build & Run Backend Container:
cd backend
docker build -t full-stack_backend .
docker run -d --network=full-stack --add-host=host.docker.internal:host-gateway -p 5001:5001 --env-file .env --name backend full-stack_backend
cd ..

	4.	Build & Run Frontend Container:
cd frontend
docker build -t full-stack_frontend .
docker run -d --network=full-stack -p 5173:5173 --name frontend full-stack_frontend
cd ..

	5.	Access Points:
⚬	Frontend Interface: http://localhost:5173
⚬	Backend REST API: http://localhost:5001
