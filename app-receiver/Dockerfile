# syntax = docker/dockerfile:1.2
####################################################
FROM python:3 as production_image

ENV SERVER_IP="0.0.0.0"
ENV SERVER_PORT="8080"
ENV OUTPUT_DIR="/usr/src/app-receiver/output"
ENV DECRYPTION_KEY="/run/secrets/decryption_key"

EXPOSE 8080

COPY receiver ./
COPY tests ./
COPY prepare-env.sh ./
RUN mkdir -p /run/secrets
COPY key ${DECRYPTION_KEY}
RUN --mount=type=secret,id=decryption_key cat /run/secrets/decryption_key

COPY Pipfile ./
COPY Pipfile.lock ./

RUN mkdir -p ${OUTPUT_DIR}
RUN pip install --no-cache-dir pipenv
RUN pipenv install --system --deploy --ignore-pipfile

ENTRYPOINT ["gunicorn", "server:app"]

# MULTI-STAGE BUILDS
# CAN BE ENABLED
# FOR THIS DOCKER IMAGE