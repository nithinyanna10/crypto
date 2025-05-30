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

# Install TA-Lib
RUN wget https://github.com/ta-lib/ta-lib/releases/download/v0.6.4/ta-lib-0.6.4-src.tar.gz && \
    tar -xzf ta-lib-0.6.4-src.tar.gz && \
    cd ta-lib-0.6.4 && \
    ./configure && make && make install && \
    cd .. && rm -rf ta-lib-0.6.4 ta-lib-0.6.4-src.tar.gz

ENV LD_LIBRARY_PATH=/usr/local/lib

# Install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy Streamlit app and DuckDB file
COPY streamlit_app.py .
COPY crypto.duckdb .

# Run Streamlit on Render-assigned port
CMD bash -c "streamlit run streamlit_app.py --server.port=${PORT:-8501} --server.enableXsrfProtection=false --server.headless=true"
