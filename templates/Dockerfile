FROM python:3-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --upgrade pip && pip install --no-cache-dir -r requirements.txt

COPY . .

ENV FLASK_HOST_RUN=0.0.0.0

EXPOSE 5000

CMD ["python", "app.py"]
