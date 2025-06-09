FROM python:3.10-slim

WORKDIR /app

# Copy only requirements first to leverage Docker cache
COPY ./backend/app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY ./backend/app .

# Set environment variables
ENV PYTHONPATH=/app

CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
