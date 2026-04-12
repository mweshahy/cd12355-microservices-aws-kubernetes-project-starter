FROM public.ecr.aws/docker/library/python:3.11-slim

# Set environment variables
ENV DB_USERNAME=mostafa \
    POSTGRES_PASSWORD=mostafaspassword \
    DB_PASSWORD=mostafaspassword \
    DB_HOST=127.0.0.1 \
    DB_PORT=5433 \
    DB_NAME=mostafasdatabase

# Set working directory
WORKDIR /app

# Copy requirements file first (for better layer caching)
COPY requirements.txt .

# Update apt and install required system packages
RUN apt-get update -y && \
    apt-get install -y build-essential libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Upgrade pip tools and install Python dependencies
RUN pip install --upgrade pip setuptools wheel && \
    pip install -r requirements.txt --force-reinstall

# Copy the rest of the application code
COPY . .

# Default command (adjust as needed)
CMD ["python", "app.py"]