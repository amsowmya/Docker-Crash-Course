FROM python:alpine

WORKDIR /app

RUN pip install pymongo

# COPY . .  (copy from local dir to inside of the image, now work dir is /app)
COPY . /app

CMD [ "python", "main.py" ]