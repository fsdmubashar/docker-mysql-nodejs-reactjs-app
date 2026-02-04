# 🐳 Docker MySQL Node.js React.js App

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![Express.js](https://img.shields.io/badge/Express.js-000000?style=for-the-badge&logo=express&logoColor=white)

**A Comprehensive Full-Stack Application Demonstrating Docker Containerization**

[Features](#-features) • [Architecture](#-architecture) • [Getting Started](#-getting-started) • [API Docs](#-api-documentation) • [Troubleshooting](#-troubleshooting)

</div>

---

## 🌟 Overview

This repository provides a **comprehensive demonstration** of Docker and Docker Compose capabilities through a full-stack application. The project showcases how to containerize and orchestrate a complete web application with:

- **React.js Frontend** - Modern UI where users can submit data
- **Node.js Backend** - RESTful API server processing requests
- **MySQL Database** - Persistent data storage

### 🎯 Learning Objectives

✅ Master Docker containerization for multi-tier applications  
✅ Understand Docker Compose orchestration  
✅ Practice full-stack development with modern technologies  
✅ Deploy isolated, reproducible development environments  
✅ Learn Docker networking and volume management  

---

## ✨ Features

### 🎨 Frontend
- Modern React.js user interface
- Responsive design
- Form validation
- Real-time data submission
- Error handling

### 🔧 Backend
- RESTful API with Express.js
- Secure data processing
- CORS enabled
- MySQL integration
- Error handling middleware

### 🐳 Docker
- Multi-container orchestration
- Isolated containers
- Volume persistence
- Network isolation
- One-command deployment

---

## 🏗️ Architecture

```
┌──────────────────────────────────────────────────┐
│          React.js Frontend (Port 3001)           │
│                                                  │
│  - User submits data through form                │
│  - Axios sends HTTP requests to backend          │
└──────────────────────────────────────────────────┘
                      │
                      ▼ HTTP POST/GET
┌──────────────────────────────────────────────────┐
│     Node.js + Express Backend (Port 5000)        │
│                                                  │
│  - Receives HTTP requests                        │
│  - Processes and validates data                  │
│  - Executes SQL queries                          │
└──────────────────────────────────────────────────┘
                      │
                      ▼ SQL Commands
┌──────────────────────────────────────────────────┐
│          MySQL Database (Port 3307)              │
│                                                  │
│  - Stores user data persistently                 │
│  - Manages data integrity                        │
└──────────────────────────────────────────────────┘
```

---

## 📦 Prerequisites

### Required Software

| Software | Version | Download Link |
|----------|---------|---------------|
| Docker | 24.0+ | [Get Docker](https://www.docker.com/products/docker-desktop/) |
| Docker Compose | 2.0+ | Included with Docker Desktop |
| Git | Latest | [Get Git](https://git-scm.com/downloads) |

### Optional Tools

- **MySQL Workbench** - Database management
- **Postman** - API testing
- **VS Code** - Code editor

### System Requirements

```
Minimum: 4GB RAM, 2 CPU cores, 10GB free space
Recommended: 8GB RAM, 4 CPU cores, 20GB free space
```

---

## 🚀 Getting Started

### 1️⃣ Clone the Repository

```bash
git clone https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app.git
cd docker-mysql-nodejs-reactjs-app
```

### 2️⃣ Ensure script.sql Exists

Make sure `script.sql` file is in the project root directory.

### 3️⃣ Build and Start Docker Containers

```bash
# Build and start all containers
docker-compose up --build

# Or run in detached mode (background)
docker-compose up --build -d
```

**Expected Output:**
```
✔ Container database  Created
✔ Container backend   Created
✔ Container frontend  Created
✔ Container database  Started
✔ Container backend   Started
✔ Container frontend  Started
```

### 4️⃣ Initialize MySQL Database

#### Option A: Using MySQL Workbench

1. Open MySQL Workbench
2. Create new connection with:
   - **Hostname:** `localhost`
   - **Port:** `3307`
   - **Username:** `root`
   - **Password:** `pass123`
3. Connect and execute `script.sql`

#### Option B: Using Command Line

```bash
# Copy script to container
docker cp script.sql database:/script.sql

# Execute script
docker exec -i database mysql -uroot -ppass123 < script.sql

# Verify
docker exec -it database mysql -uroot -ppass123 -e "SHOW DATABASES; USE myapp; SHOW TABLES;"
```

### 5️⃣ Access the Application

Open your browser and navigate to:

```
http://localhost:3001
```

🎉 **Success!** The React application should now be running.

---

## 📁 Project Structure

```
docker-mysql-nodejs-reactjs-app/
│
├── frontend/                   # React.js Application
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── App.js
│   │   ├── App.css
│   │   └── index.js
│   ├── Dockerfile
│   └── package.json
│
├── backend/                    # Node.js API Server
│   ├── server.js
│   ├── Dockerfile
│   └── package.json
│
├── docker-compose.yml         # Container orchestration
├── script.sql                 # Database initialization
├── .gitignore
└── README.md
```

---

## 📡 API Documentation

### Base URL
```
http://localhost:5000/api
```

### Endpoints

#### Submit Data
```http
POST /api/submit
Content-Type: application/json

{
  "name": "Muhammad Mubashar",
  "email": "city.mubashar@gmail.com",
  "message": "Hello from Docker!"
}
```

**Response:**
```json
{
  "success": true,
  "message": "Data saved successfully",
  "id": 1
}
```

#### Get All Data
```http
GET /api/data
```

**Response:**
```json
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Muhammad Mubashar",
      "email": "city.mubashar@gmail.com",
      "message": "Hello from Docker!",
      "created_at": "2025-02-04T10:30:00.000Z"
    }
  ]
}
```

---

## 🐳 Docker Configuration

### Services Overview

| Service | Port Mapping | Description |
|---------|--------------|-------------|
| frontend | 3001:3000 | React.js UI |
| backend | 5000:5000 | Express.js API |
| database | 3307:3306 | MySQL 8.0 |

### Useful Docker Commands

```bash
# View running containers
docker-compose ps

# View logs
docker-compose logs -f

# Stop containers
docker-compose down

# Restart a specific service
docker-compose restart backend

# Execute command in container
docker exec -it backend sh

# Remove containers and volumes
docker-compose down -v
```

---

## 💾 Database Schema

```sql
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    message TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Database Operations

```bash
# Connect to MySQL
docker exec -it database mysql -uroot -ppass123

# View all data
docker exec -it database mysql -uroot -ppass123 -e "SELECT * FROM myapp.users;"

# Backup database
docker exec database mysqldump -uroot -ppass123 myapp > backup.sql

# Restore database
docker exec -i database mysql -uroot -ppass123 myapp < backup.sql
```

---

## 🔍 Troubleshooting

### Issue: Port Already in Use

```bash
# Find process using port 3307
lsof -i :3307

# Kill the process
kill -9 <PID>

# Or change port in docker-compose.yml
```

### Issue: Database Connection Failed

```bash
# Check database container logs
docker-compose logs database

# Restart database
docker-compose restart database

# Verify connection
docker exec -it database mysql -uroot -ppass123 -e "SELECT 1"
```

### Issue: Frontend Can't Connect to Backend

```bash
# Check backend logs
docker-compose logs backend

# Test backend
curl http://localhost:5000/api/health

# Verify all containers are running
docker-compose ps
```

### Issue: Permission Denied

```bash
# Fix file permissions
sudo chown -R $USER:$USER .
```

---

## 💡 Usage Guide

### Development Workflow

1. **Make code changes** in frontend or backend directories
2. **Rebuild containers** if needed:
   ```bash
   docker-compose up --build
   ```
3. **View logs** for debugging:
   ```bash
   docker-compose logs -f backend
   ```

### Testing the Application

1. Open `http://localhost:3001`
2. Fill in the form with:
   - Name
   - Email
   - Message
3. Click Submit
4. Verify data is saved in MySQL

### Verify Database Entry

```bash
docker exec -it database mysql -uroot -ppass123 -e "SELECT * FROM myapp.users;"
```

---

## 🎓 Learning Resources

This project demonstrates:

- **Docker Basics** - Containerization fundamentals
- **Docker Compose** - Multi-container orchestration
- **Full-Stack Development** - React + Node.js + MySQL
- **RESTful APIs** - Building and consuming APIs
- **Database Management** - MySQL operations in containers

### Next Steps

- Add authentication
- Implement pagination
- Add input validation
- Create update/delete endpoints
- Add environment variables
- Implement logging
- Add health checks
- Setup CI/CD pipeline

---

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Push to the branch
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License.

```
MIT License

Copyright (c) 2025 Muhammad Mubashar Karamat Ali

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 📞 Contact

**Muhammad Mubashar Karamat Ali**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/mubashar-karamat-833457245/)
[![Email](https://img.shields.io/badge/Email-Contact-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:city.mubashar@gmail.com)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/fsdmubashar)

**Project Link:** [https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app](https://github.com/fsdmubashar/docker-mysql-nodejs-reactjs-app)

---

<div align="center">

**⭐ If you find this project helpful, please give it a star!**

**Made with ❤️ by [Muhammad Mubashar Karamat Ali](https://github.com/fsdmubashar)**

![Visitors](https://komarev.com/ghpvc/?username=fsdmubashar-docker&color=brightgreen&style=for-the-badge)

</div>
