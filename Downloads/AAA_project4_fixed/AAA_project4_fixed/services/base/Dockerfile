FROM python:3.11-slim

WORKDIR /app

# Install system dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    g++ \
    gcc \
    libatlas-base-dev \
    libffi-dev \
    liblapack-dev \
    libopenblas-dev \
    libssl-dev \
    wget \
    && rm -rf /var/lib/apt/lists/*

# Install TA-Lib (required by some trading indicators)
RUN wget https://github.com/ta-lib/ta-lib/releases/download/v0.6.4/ta-lib-0.6.4-src.tar.gz && \
    tar -xzf ta-lib-0.6.4-src.tar.gz && \
    cd ta-lib-0.6.4 && \
    ./configure && \
    make && \
    make install && \
    cd .. && \
    rm -rf ta-lib-0.6.4 ta-lib-0.6.4-src.tar.gz

ENV LD_LIBRARY_PATH=/usr/local/lib

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy your Streamlit app
COPY . .

# Set the port Streamlit should run on
EXPOSE 8501

# Default command to run Streamlit app
CMD ["streamlit", "run", "streamlit_app.py", "--server.port=8501", "--server.enableCORS=false", "--server.headless=true"]
