FROM postgres:11.6-alpine

ENV POSTGRES_DB=db \
    POSTGRES_USER=usr

COPY sql/01-create.sql /docker-entrypoint-initdb.d
COPY sql/02-init.sql /docker-entrypoint-initdb.d