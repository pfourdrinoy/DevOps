# Utilise l'image PostgreSQL version 14.1 avec Alpine Linux
FROM postgres:14.1-alpine

# Copie les fichiers SQL dans le répertoire d'initialisation de la base de données
COPY ./CreateScheme.sql /docker-entrypoint-initdb.d/CreateScheme.sql
COPY ./InsertData.sql /docker-entrypoint-initdb.d/InsertData.sql

# Définit les variables d'environnement pour la base de données PostgreSQL
ENV POSTGRES_DB=db \
    POSTGRES_USER=usr \
    POSTGRES_PASSWORD=pwd
