FROM python:3.9-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install Flask==2.0.3 Werkzeug==2.0.3
COPY app.py .
EXPOSE 5000
CMD ["python", "app.py"]
