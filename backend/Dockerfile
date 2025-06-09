FROM python:3.10-slim

WORKDIR /app

# Copy only requirements first to leverage Docker cache
COPY ./app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY ./app .

# Set environment variables
ENV PYTHONPATH=/app

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
