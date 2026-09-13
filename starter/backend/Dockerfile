# FROM public.ecr.aws/docker/library/python:3.10-slim
# ENV FLASK_RUN_HOST=0.0.0.0

# WORKDIR /
# RUN adduser -u 1000 -D app
# RUN apk update && \
#   apk add \
#   pcre \
#   pcre-dev \
#   build-base \
#   gcc \
#   linux-headers \
#   openssl \
#   libffi-dev \
#   && pip install pipenv

# RUN mkdir -p /var/www/app && mkdir /app

# COPY Pipfile* /app
# RUN cd /app && pipenv install --system --deploy

# COPY . /app

# CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app


FROM public.ecr.aws/docker/library/python:3.10-slim

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libpcre3 \
    libpcre3-dev \
    build-essential \
    gcc \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pip install pipenv

RUN useradd --uid 1000 --create-home app

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app

RUN cd /app && pipenv install --system --deploy

COPY . /app

RUN chown -R app:app /app /var/www/app

USER app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]