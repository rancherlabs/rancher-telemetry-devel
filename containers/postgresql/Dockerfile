FROM postgres
ENV POSTGRES_USER admin
ENV POSTGRES_PASSWORD admin
ENV POSTGRES_DB telemetry
ADD create_db.sql /docker-entrypoint-initdb.d/
