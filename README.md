# FastAPI Book Management API

## Overview

The **FastAPI Book Management API** is a RESTful web service for managing a book collection. It provides full CRUD (Create, Read, Update, Delete) operations, ensuring efficient handling of book data with robust input validation, proper error handling, and automatically generated API documentation. This project can be deployed on an AWS EC2 t2.micro instance, which is eligible for the AWS Free Tier.

## Features

- 📚 **Book Management** – Add, retrieve, update, and delete books
- ✅ **Data Validation** – Uses Pydantic models for request validation
- 🔍 **Genre Classification** – Enum-based genre selection
- 🧪 **Automated Testing** – Comprehensive test coverage using pytest
- 📝 **Auto-Generated API Docs** – Interactive Swagger UI & ReDoc
- 🔒 **CORS Middleware** – Secure cross-origin requests handling
- 🚀 **Optimized Performance** – Uvicorn ASGI server integration

## Technologies Used

- **Backend:** Python 3.12, FastAPI
- **Containerization:** Docker & Docker Compose
- **Reverse Proxy:** Nginx
- **Cloud Platform:** AWS EC2
- **Documentation:** Swagger UI, ReDoc

## Prerequisites

On your EC2 instance:
- Docker
- Docker Compose
- Git

## EC2 Setup and Docker Deployment

### 1. EC2 Instance Setup

```bash
# Connect to your EC2 instance
ssh -i your-key.pem ec2-user@your-ec2-ip

# Install Docker and Docker Compose
sudo yum update -y
sudo yum install -y docker
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

### 2. Deploy Application

```bash
# Clone repository on EC2
git clone https://github.com/CynthiaWahome/fastapi-book-project.git
cd fastapi-book-project

# Start Docker containers
docker-compose up -d

# Verify deployment
docker ps
```

## Docker Configuration on EC2


## ⚠️ Port Binding Issues on EC2

When deploying on EC2, you might encounter port binding errors. Here's how to resolve them:

1. **Check EC2 Security Group**
   - Ensure port 80 is open in your EC2 security group
   - Allow inbound HTTP traffic

2. **Port 80 Already in Use on EC2**
```bash
# Check what's using port 80
sudo netstat -tulpn | grep :80

# Stop system-level Nginx if running
sudo systemctl stop nginx
sudo systemctl disable nginx
```

3. **Clean Docker Environment on EC2**
```bash
# Stop all containers
docker-compose down

# Prune containers, networks, and volumes
docker system prune -f

# Start fresh
docker-compose up -d
```

## Accessing the API

Once deployed on EC2:
- API Endpoints: `http://your-ec2-ip/api/v1/`
- Swagger UI: `http://your-ec2-ip/docs`
- ReDoc: `http://your-ec2-ip/redoc`

## Troubleshooting on EC2

1. **Check Container Logs**
```bash
# View logs
docker-compose logs -f

# Check specific container
docker logs fastapi-app
docker logs nginx
```

2. **Verify Network Configuration**
```bash
# Check EC2 network interfaces
ip addr show

# Check Docker networks
docker network ls
docker network inspect app-network
```

3. **Test API Access**
```bash
# Test from EC2 instance
curl -v http://localhost/api/v1/books

# Test from your local machine
curl -v http://your-ec2-ip/api/v1/books
```

## Maintenance Commands

```bash
# Update application on EC2
git pull origin main
docker-compose up -d --build

# Monitor containers
docker stats

# Check container health
docker ps -a
```
=

### 7. Test the Deployment

Visit your EC2 instance’s public IP in a browser:

```
http://<YOUR_EC2_PUBLIC_IP>/docs
```

If everything is configured correctly, you should see the FastAPI Swagger documentation.

## API Endpoints

### Books

| Method | Endpoint                 | Description |
|--------|--------------------------|-------------|
| GET    | `/api/v1/books/`         | Get all books |
| GET    | `/api/v1/books/{book_id}` | Retrieve a specific book by ID |
| POST   | `/api/v1/books/`         | Add a new book |
| PUT    | `/api/v1/books/{book_id}` | Update an existing book |
| DELETE | `/api/v1/books/{book_id}` | Delete a book by ID |

## Running Tests

To run unit tests, execute:

```bash
pytest tests/
```

## Contributing

Feel free to submit issues, create pull requests, or suggest enhancements.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

