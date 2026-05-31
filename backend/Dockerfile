FROM python:3.10-slim

# Install system dependencies for OpenCV and machine learning
# Note: libgl1-mesa-glx was removed in Debian Trixie; use libgl1 instead
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libgl1 \
    libglib2.0-0 \
    libgomp1 \
    libsm6 \
    libxext6 \
    ffmpeg \
    curl \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Download the YOLOv8 nano weights at build time (6MB, fits under 512MB RAM limit)
# Using nano model instead of small to stay within Render free tier memory constraints
RUN curl -L -o /app/yolov8n.pt https://github.com/ultralytics/assets/releases/download/v8.3.0/yolov8n.pt

# Copy requirements and install dependencies
COPY backend/requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Copy backend application and pipeline tracking engine code
COPY backend/app/ /app/app/
COPY pipeline/ /app/pipeline/

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
