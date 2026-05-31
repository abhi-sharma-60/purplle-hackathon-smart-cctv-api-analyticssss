FROM python:3.10-slim

# Install system dependencies for OpenCV and machine learning
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    libgl1-mesa-glx \
    libglib2.0-0 \
    libgomp1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

# Copy requirements and install dependencies
COPY backend/requirements.txt .
RUN pip install --no-cache-dir --upgrade pip && \
    pip install --no-cache-dir -r requirements.txt

# Download the YOLO weights file during build time to guarantee instant container startup
RUN apt-get update && apt-get install -y --no-install-recommends curl && \
    curl -L -o /app/yolov8s.pt https://github.com/ultralytics/assets/releases/download/v8.3.0/yolov8s.pt && \
    apt-get purge -y curl && apt-get autoremove -y && rm -rf /var/lib/apt/lists/*

# Copy backend application and pipeline tracking engine code
COPY backend/app/ /app/app/
COPY pipeline/ /app/pipeline/

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
