# Dockerfile (Render optimized)

FROM python:3.12-slim

WORKDIR /app

# Copy requirements and install packages
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Download the spaCy language model during the build
RUN python -m spacy download en_core_web_sm

COPY . .

# Render exposes the port via the $PORT environment variable
EXPOSE 8000 

# Command to run the application using Gunicorn (Bind to the injected $PORT variable)
CMD ["gunicorn", "-w", "4", "-k", "uvicorn.workers.UvicornWorker", "main:app", "--bind", "0.0.0.0:$PORT"]