###
### Base stage
###
FROM python:3.13.0a6-alpine3.19 AS base
ENV CONFIG=./config/gunicorn.conf.py
WORKDIR /app
RUN adduser --system voteuser \
    && chown voteuser /app \
    && apk update \
    && apk --no-cache add  curl \
    && rm -fr /var/cache/apk/*
COPY ./app/requirements.txt ./requirements.txt
RUN pip install --no-cache-dir -r requirements.txt
COPY ./app/ .

###
### Dev stage
###
FROM base AS dev
RUN pip install watchdog
COPY ./config-dev/gunicorn.conf.py ./config/gunicorn.conf.py
USER voteuser
EXPOSE 80
CMD ["sh", "-c", "gunicorn app:app -c $CONFIG"]

###
### Final stage
###
FROM base AS final
COPY ./config-prod/gunicorn.conf.py ./config/gunicorn.conf.py
USER voteuser
EXPOSE 80
HEALTHCHECK --interval=5s --timeout=3s \
  CMD curl -fs http://localhost/ || exit 1
CMD ["sh", "-c", "gunicorn app:app -c $CONFIG"]
