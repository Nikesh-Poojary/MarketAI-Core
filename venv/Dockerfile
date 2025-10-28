# Use an official Python runtime as a parent image
FROM python:3.12-slim

# Set the working directory in the container
WORKDIR /app

# Install system dependencies if any (spaCy might sometimes need them, though less common with wheels)
# RUN apt-get update && apt-get install -y --no-install-recommends build-essential && rm -rf /var/lib/apt/lists/*

# Copy the requirements file into the container
COPY requirements.txt requirements.txt

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Download the spaCy model during the build
# This avoids downloading it every time the container starts
RUN python -m spacy download en_core_web_sm

# Copy the rest of your application code into the container
COPY . .

# Expose the port the app runs on (matching the Gunicorn command)
EXPOSE 8080

# Command to run the application using Gunicorn with Uvicorn workers
# -w 2: Use 2 worker processes (adjust based on Cloud Run instance size/expected load)
# -k uvicorn.workers.UvicornWorker: Use Uvicorn for async handling
# main:app: Tells Gunicorn to run the 'app' object found in 'main.py'
# --bind 0.0.0.0:8080: Listen on all network interfaces on port 8080
CMD ["gunicorn", "-w", "2", "-k", "uvicorn.workers.UvicornWorker", "main:app", "--bind", "0.0.0.0:8080"]