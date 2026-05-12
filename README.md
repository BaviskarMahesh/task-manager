# MERN Task Management Application Deployment

# Project Overview

This project is a MERN Stack based Task Management Application deployed on AWS EC2 using MongoDB Atlas.

The application allows users to:
- Create Tasks
- Update Tasks
- Delete Tasks
- Mark Tasks as Completed
- Search Tasks
- Filter Tasks

---

# Technology Stack

| Technology | Purpose |
|------------|----------|
| React.js | Frontend UI |
| Vite | Frontend Build Tool |
| Node.js | JavaScript Runtime |
| Express.js | Backend Framework |
| MongoDB Atlas | Cloud Database |
| AWS EC2 | Cloud Hosting |
| PM2 | Process Manager |
| GitHub | Source Code Hosting |
| NVM | Node Version Manager |

---

# GitHub Repository

```text
https://github.com/BaviskarMahesh/task-manager
```

---

# Step 1: Launch AWS EC2 Instance

## Steps

1. Open AWS Console
2. Go to EC2 Dashboard
3. Click Launch Instance
4. Select Ubuntu Server
5. Choose t2.micro instance
6. Create or select Key Pair
7. Configure Security Groups
8. Launch Instance

---

# Step 2: Configure Security Group

## Inbound Rules

| Type | Port | Purpose |
|------|------|----------|
| SSH | 22 | Remote Server Access |
| HTTP | 80 | Web Access |
| HTTPS | 443 | Secure Web Access |
| Custom TCP | 5001 | Backend API |

---

# Why Security Groups are Necessary

| Port | Reason |
|------|---------|
| 22 | Required for SSH login |
| 80 | Allows HTTP website access |
| 443 | Allows secure HTTPS communication |
| 5001 | Exposes backend API |

---

# Step 3: Connect to EC2 Instance

## Give Permission to PEM Key

```bash
chmod 400 task-key.pem
```

## SSH Connection

```bash
ssh -i task-key.pem ubuntu@YOUR_PUBLIC_IP
```

Example:

```bash
ssh -i task-key.pem ubuntu@3.95.213.143
```

---

# Step 4: Install NVM

## Install NVM

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
```

## Load NVM

```bash
source ~/.bashrc
```

---

# Why NVM is Necessary

- Manages multiple Node.js versions
- Avoids permission issues
- Easy Node.js installation
- Better dependency handling

---

# Step 5: Install Node.js

## Install Node Version 22

```bash
nvm install 22
```

## Set Default Version

```bash
nvm alias default 22
```

## Verify Installation

```bash
node -v
npm -v
```

---

# Step 6: Clone GitHub Repository

```bash
git clone https://github.com/BaviskarMahesh/task-manager.git
```

## Move into Project Folder

```bash
cd task-manager
```

---

# Why GitHub is Necessary

- Stores project source code
- Version control
- Easy deployment
- Backup of project
- Collaboration support

---

# Step 7: Backend Setup

## Move into Backend

```bash
cd backend
```

## Install Backend Dependencies

```bash
npm install
```

---

# Step 8: Configure Environment Variables

## Create .env File

```bash
nano .env
```

## Add the Following

```env
PORT=5001
MONGO_URI=your_mongodb_connection_string
NODE_ENV=production
```

Example:

```env
PORT=5001
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/taskmanager
NODE_ENV=production
```

---

# Why Environment Variables are Necessary

- Protect sensitive credentials
- Avoid hardcoding passwords
- Easy configuration management
- Better security practices

---

# Step 9: MongoDB Atlas Setup

## Create MongoDB Atlas Account

Visit:

```text
https://www.mongodb.com/atlas
```

---

# MongoDB Atlas Configuration Steps

1. Create Free Cluster
2. Create Database User
3. Set Username and Password
4. Configure Network Access
5. Add Current IP or:

```text
0.0.0.0/0
```

6. Copy Connection String

---

# Why MongoDB Atlas is Necessary

- Cloud database hosting
- Accessible from anywhere
- Automatic backups
- Secure authentication
- Scalable infrastructure

---

# MongoDB Connection String Example

```env
mongodb+srv://username:password@cluster.mongodb.net/taskmanager
```

---

# Important MongoDB Note

If password contains special characters like:

```text
@
#
$
%
```

Encode them properly.

Example:

```text
@ becomes %40
```

---

# Step 10: Frontend Setup

## Move into Frontend

```bash
cd ../frontend
```

## Install Frontend Dependencies

```bash
npm install
```

## Build Frontend

```bash
npm run build
```

---

# Why Build is Necessary

The build process:
- Optimizes React application
- Creates production-ready files
- Generates dist folder
- Improves performance

---

# Step 11: Install PM2

## Install PM2 Globally

```bash
npm install -g pm2
```

---

# Why PM2 is Necessary

- Keeps backend running continuously
- Auto restarts on crashes
- Background execution
- Process monitoring
- Log management

---

# Step 12: Start Backend Server

## Move into Backend

```bash
cd ../backend
```

## Start Server

```bash
pm2 start server.js --name backend
```

---

# PM2 Commands

## Check Status

```bash
pm2 status
```

## View Logs

```bash
pm2 logs backend
```

## Restart Backend

```bash
pm2 restart backend
```

## Stop Backend

```bash
pm2 stop backend
```

## Delete Backend

```bash
pm2 delete backend
```

## Save PM2 Process

```bash
pm2 save
```

---

# Backend Server Code

```javascript
import express from 'express';
import mongoose from 'mongoose';
import cors from 'cors';
import dotenv from 'dotenv';
import path from 'path';
import { fileURLToPath } from 'url';
import tasksRouter from './routes/tasks.js';

dotenv.config();

const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);

const app = express();
const PORT = process.env.PORT || 5000;

const MONGO_URI = process.env.MONGO_URI;

app.use(cors());
app.use(express.json());

app.use('/api/tasks', tasksRouter);

if (process.env.NODE_ENV === 'production') {
  app.use(express.static(path.join(__dirname, '../frontend/dist')));

  app.get('*', (req, res) => {
    res.sendFile(path.resolve(__dirname, '../frontend/dist', 'index.html'));
  });
}

mongoose.connect(MONGO_URI)
  .then(() => {
    console.log('Connected to MongoDB');

    app.listen(PORT, '0.0.0.0', () => {
      console.log(`Server running on port ${PORT}`);
    });

  })
  .catch((error) => {
    console.error('Error connecting to MongoDB:', error.message);
  });
```

---

# Why Each Backend Component is Necessary

| Component | Purpose |
|-----------|----------|
| Express | Backend server framework |
| Mongoose | MongoDB connection |
| CORS | Frontend-backend communication |
| dotenv | Loads environment variables |
| path | File path handling |
| PM2 | Process management |

---

# Frontend API Configuration

```javascript
const API_URL = import.meta.env.VITE_API_URL || '/api/tasks';
```

---

# Application Features

- Add Tasks
- Edit Tasks
- Delete Tasks
- Search Tasks
- Filter Tasks
- Toggle Completion Status

---

# Access Application

## Frontend

```text
http://YOUR_PUBLIC_IP:5001
```

Example:

```text
http://3.95.213.143:5001
```

---

## Backend API

```text
http://YOUR_PUBLIC_IP:5001/api/tasks
```

Example:

```text
http://3.95.213.143:5001/api/tasks
```

---

# Common Errors and Solutions

---

## Error: MongoDB Atlas Not Connecting

### Error

```text
Could not connect to MongoDB Atlas cluster
```

### Solution

Go to MongoDB Atlas:

```text
Network Access -> Add IP Address
```

Add:

```text
0.0.0.0/0
```

---

## Error: PM2 Script Already Running

### Error

```text
Script already launched
```

### Solution

```bash
pm2 stop backend
pm2 delete backend
pm2 start server.js --name backend
```

---

## Error: Frontend Dist Folder Missing

### Error

```text
ENOENT: no such file or directory
```

### Solution

```bash
cd frontend
npm run build
```

---

## Error: Port Not Accessible

### Solution

Open Port 5001 in EC2 Security Group.

---

# Useful Commands

## Check Running Processes

```bash
pm2 status
```

## Check Server Logs

```bash
pm2 logs backend
```

## Restart Server

```bash
pm2 restart backend
```

## Stop Server

```bash
pm2 stop backend
```

---
 
