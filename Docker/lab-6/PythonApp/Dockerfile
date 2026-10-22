FROM python:3.12-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .

CMD ["python" , "app.py"]

EXPOSE 5000


ENV APP_NAME=iVolveApp3 APP_ENV=dev

