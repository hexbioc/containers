FROM python:3.11-alpine3.17

WORKDIR /app

ADD requirements.txt .

RUN pip install -r requirements.txt

ADD . .

ENTRYPOINT [ "python", "main.py" ]
