#Using an official lightweight Python image
FROM python:3.9-slim

#Set the working directory
WORKDIR /app

#Copy the requirements file
COPY requirements.txt

#Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy the application code
COPY . .

#EXPOSE the port Fastapi runs on
EXPOSE 8000

#RUN the application 
CMD ["uvicron", "main:app", "--host", "0.0.0.0", "--port", "8000"]
