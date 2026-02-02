FROM public.ecr.aws/docker/library/python:3.10-slim-bookworm

ENV FLASK_RUN_HOST=0.0.0.0

RUN useradd -m -u 1000 app && \
    mkdir -p /var/www/app && \
    chown -R app:app /var/www/app

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential \
    libpcre3 \
    libpcre3-dev \
    gcc \
    && rm -rf /var/lib/apt/lists/* && \
    pip install pipenv


WORKDIR /app

COPY --chown=app:app Pipfile* ./

RUN pipenv install --system --deploy

COPY --chown=app:app . .

USER app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app