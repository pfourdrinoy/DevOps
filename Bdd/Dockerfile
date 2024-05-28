FROM postgres:14.1-alpine

COPY ./CreateScheme.sql /docker-entrypoint-initdb.d/CreateScheme.sql
COPY ./InsertData.sql /docker-entrypoint-initdb.d/InsertData.sql

ENV POSTGRES_DB=db \
   POSTGRES_USER=usr \
   POSTGRES_PASSWORD=pwd
