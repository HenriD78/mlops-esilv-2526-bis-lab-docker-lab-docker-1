# 1. Use a lightweight Python base image
FROM python:3.11-slim

# 2. Set environment variables 
# PYTHONDONTWRITEBYTECODE: Prevents Python from writing .pyc files to disk
# PYTHONUNBUFFERED: Ensures console output is sent straight to terminal (useful for logs)
ENV PYTHONDONTWRITEBYTECODE=1
ENV PYTHONUNBUFFERED=1

# 3. Set the working directory inside the container
WORKDIR /app

# 4. Install system dependencies (if your code needs tools like gcc or libpq)
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# 5. Copy only the requirements file first to leverage Docker's cache
COPY requirements.txt .

# 6. Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# 7. Copy the rest of your application code
COPY . .

# 8. Create a non-root user for security (don't run as root in production!)
RUN adduser --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

# 9. Expose the port your app runs on
EXPOSE 8000

# 10. The command to run your app (using Gunicorn or Uvicorn is standard)
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "app.main:app"]
