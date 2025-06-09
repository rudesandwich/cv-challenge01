# Stage 1: Builder
FROM python:3.10-slim AS builder

# Install system dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install specific Poetry version
ENV POETRY_HOME=/opt/poetry \
    POETRY_VERSION=1.8.2
RUN curl -sSL https://install.python-poetry.org | python3 - && \
    cd /usr/local/bin && \
    ln -s /opt/poetry/bin/poetry && \
    poetry --version

# Configure Poetry
RUN poetry config virtualenvs.create false

# Copy dependency files first
WORKDIR /app
COPY pyproject.toml poetry.lock ./

# Install dependencies with retry
RUN poetry install --no-interaction --no-ansi --no-root || \
    (echo "First attempt failed, retrying..." && poetry install --no-interaction --no-ansi --no-root)

# Stage 2: Runtime
FROM python:3.10-slim
WORKDIR /app

# Copy installed packages
COPY --from=builder /usr/local/lib/python3.10/site-packages /usr/local/lib/python3.10/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Copy application code
COPY . .

# Verify critical files
RUN [ -f app/main.py ] || (echo "Error: main.py missing!" && exit 1)
RUN [ -f prestart.sh ] || (echo "Error: prestart.sh missing!" && exit 1)
RUN chmod +x prestart.sh

EXPOSE 8000
CMD ["./prestart.sh"]
