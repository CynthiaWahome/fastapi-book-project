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

## Project Structure

```
fastapi-book-project/
├── api/
│   ├── db/
│   │   ├── __init__.py
│   │   └── schemas.py      # Pydantic models and database schema
│   ├── routes/
│   │   ├── __init__.py
│   │   └── books.py        # API route handlers for books
│   └── router.py           # Centralized API router
├── core/
│   ├── __init__.py
│   └── config.py           # Configuration settings
├── tests/
│   ├── __init__.py
│   └── test_books.py       # Unit tests for book endpoints
├── main.py                 # Application entry point
├── requirements.txt        # Project dependencies
└── README.md               # Project documentation
```

## Technologies Used

- **Programming Language:** Python 3.12
- **Framework:** FastAPI
- **Validation:** Pydantic
- **Testing:** pytest
- **Web Server:** Uvicorn
- **Reverse Proxy:** Nginx (for deployment)

## Prerequisites

Ensure you have the following installed before running the application:

- Python 3.12+
- Git
- Virtual environment (`venv`)

## Installation & Setup

### 1. Clone the Repository

```bash
git clone https://github.com/CynthiaWahome/fastapi-book-project.git
cd fastapi-book-project
```

### 2. Create and Activate a Virtual Environment

```bash
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

## Running the Application

### Start the Development Server

```bash
uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

### API Documentation

Once the server is running, you can access the API documentation at:

- **Swagger UI:** [http://localhost:8000/docs](http://localhost:8000/docs)
- **ReDoc:** [http://localhost:8000/redoc](http://localhost:8000/redoc)

## API Endpoints

### Books

| Method | Endpoint                 | Description |
|--------|--------------------------|-------------|
| GET    | `/api/v1/books/`         | Get all books |
| GET    | `/api/v1/books/{book_id}` | Retrieve a specific book by ID |
| POST   | `/api/v1/books/`         | Add a new book |
| PUT    | `/api/v1/books/{book_id}` | Update an existing book |
| DELETE | `/api/v1/books/{book_id}` | Delete a book by ID |

## Deployment Guide (Amazon EC2 with Nginx)

### 1. Set Up an Amazon EC2 Instance

- Launch an **Amazon Linux 2023** EC2 instance
- Configure Security Group to allow HTTP (80), HTTPS (443), and custom API port (8000)
- Connect to your instance via SSH:

```bash
ssh -i ~/.pem ec2-user@<YOUR_EC2_PUBLIC_IP>
```

### 2. Install Required Packages

```bash
sudo dnf install -y python3 python3-pip nginx git
```

### 3. Clone the Repository and Set Up the Virtual Environment

```bash
git clone https://github.com/CynthiaWahome/fastapi-book-project.git
cd fastapi-book-project
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### 4. Configure FastAPI as a Systemd Service

Create a service file:

```bash
sudo nano /etc/systemd/system/fastapi.service
```

Add the following content:

```
[Unit]
Description=FastAPI Application
After=network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/home/ec2-user/fastapi-book-project
ExecStart=/home/ec2-user/fastapi-book-project/venv/bin/uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and close the file.

### 5. Start and Enable the FastAPI Service

```bash
sudo systemctl daemon-reload
sudo systemctl start fastapi.service
sudo systemctl enable fastapi.service
```

Verify the service is running:
```bash
sudo systemctl status fastapi.service
```

### 6. Configure Nginx as a Reverse Proxy

Create a new Nginx configuration file:

```bash
sudo nano /etc/nginx/conf.d/fastapi.conf
```

Add the following content:

```
server {
    listen 80;
    server_name <YOUR_EC2_PUBLIC_IP>;

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

Save and close the file, then restart Nginx:

```bash
sudo systemctl restart nginx
sudo systemctl enable nginx
```

### 7. Test the Deployment

Visit your EC2 instance’s public IP in a browser:

```
http://<YOUR_EC2_PUBLIC_IP>/docs
```

If everything is configured correctly, you should see the FastAPI Swagger documentation.

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

