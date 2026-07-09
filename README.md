# AWS Lightsail Flask Demo

A simple, lightweight Flask web application designed as a demonstration for deploying cloud-native applications to **AWS Lightsail**.

## Features
- **Flask Framework**: Simple, Python-based web server.
- **Production Ready**: Configured with `gunicorn` for WSGI production deployments.
- **Clean Layout**: A minimal HTML landing page.

## Project Structure
```text
aws-lightsail/
├── templates/
│   └── index.html      # Main HTML landing page
├── app.py              # Flask application entry point
├── requirements.txt    # Project dependencies (Flask, Gunicorn)
└── README.md           # Documentation
```

## Local Development Setup

Follow these steps to run the application locally on your machine.

### 1. Create and Activate a Virtual Environment
- **On macOS/Linux:**
  ```bash
  python3 -m venv venv
  source venv/bin/activate
  ```
- **On Windows:**
  ```bash
  python -m venv venv
  venv\Scripts\activate
  ```

### 2. Install Dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the Application
```bash
python app.py
```
By default, the server will start at `http://localhost:5000`.

---

## Production Execution
To run the server using `gunicorn` (production WSGI server):
```bash
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

---

## Deploying to AWS Lightsail
You can deploy this application using the following methods:

### Option A: Deployment via Lightsail Containers (Recommended)
1. **Create a `Dockerfile`** in the root directory:
   ```dockerfile
   FROM python:3.9-slim
   WORKDIR /app
   COPY requirements.txt .
   RUN pip install -r requirements.txt
   COPY . .
   EXPOSE 5000
   CMD ["gunicorn", "-w", "4", "-b", "0.0.0.0:5000", "app:app"]
   ```
2. **Build and Push**: Build the Docker image and push it to your AWS Lightsail container service registry.
3. **Deploy**: Deploy the container via the Lightsail Console, exposing port `5000`.

### Option B: Deployment on a Lightsail Virtual Private Server (VPS)
1. **Launch a Lightsail Instance**: Create a Linux instance (e.g., Ubuntu).
2. **Configure Security Group**: Open port `80` (HTTP) and port `22` (SSH) in the Lightsail firewall.
3. **Set Up the Host**:
   - Connect via SSH.
   - Install Python, pip, and Git:
     ```bash
     sudo apt update && sudo apt install python3-pip python3-venv git -y
     ```
4. **Deploy Application**:
   - Clone the repository.
   - Set up the virtual environment and install requirements.
5. **Configure Nginx as Reverse Proxy**:
   - Route traffic from port `80` to port `5000` (where Gunicorn is running).
   - Set up Gunicorn to run as a systemd background service.
