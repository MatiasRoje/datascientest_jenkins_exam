FROM python:3.8-slim

WORKDIR /app

COPY ./requirements.txt /app/requirements.txt

RUN apt-get update \
    && apt-get install gcc -y \
    && apt-get clean

RUN pip install -r /app/requirements.txt \
    && rm -rf /root/.cache/pip

COPY . /app/

ENV DATABASE_URI=postgresql://movie_db_username:movie_db_password@movie_db/movie_db_dev
ENV CAST_SERVICE_HOST_URL=http://cast-service:8000/api/v1/casts/

EXPOSE 8000

CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]