FROM python:3.10

WORKDIR /app

# Install system dependencies
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

# Install Poetry
RUN curl -sSL https://install.python-poetry.org | python3 -
ENV PATH="/root/.local/bin:$PATH"

# Copy only necessary files for dependency installation
COPY pyproject.toml poetry.lock* ./

# Install Python dependencies
RUN poetry install --no-root

# Copy the rest of the code
COPY . .

# Set PYTHONPATH to include /app
ENV PYTHONPATH="/app:${PYTHONPATH}"

# Make prestart script executable
RUN chmod +x ./prestart.sh

# Run tests with PYTHONPATH configured
RUN poetry run pytest

EXPOSE 8000

CMD ["sh", "-c", "poetry run ./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload"]
