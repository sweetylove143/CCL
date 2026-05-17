# Practical 6: Dockerfile for Flask app

## Goal

Containerize a simple Python Flask application using Docker.

## Prerequisites

- Docker Desktop installed on Windows
- Python Flask app files

## Step 1: Create a sample Flask app

Create app.py:

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello, Docker Flask App!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

Create requirements.txt:

```
flask
```

## Step 2: Create Dockerfile

Create Dockerfile:

```
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
EXPOSE 5000
CMD ["python", "app.py"]
```

## Step 3: Build the image

```
docker build -t flask-app .
```

## Step 4: Run the container

```
docker run -p 5000:5000 flask-app
```

## Step 5: Test

Open:

```
http://localhost:5000
```
