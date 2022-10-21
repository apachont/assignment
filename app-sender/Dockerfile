FROM python:3 as production_image

ENV SCAN_INTERVAL="0"
ENV INPUT_DIR="/usr/src/app-sender/input"
ENV ENCRYPTION_KEY_DIR="/run/secrets/"
ENV ENCRYPTION_KEY="${ENCRYPTION_KEY_DIR}/encryption_key"
ENV STATUS_DB_PATH="/usr/src/app-sender/status-db"
ENV RECEIVER_ADDRESS="app-receiver"
ENV RECEIVER_PORT="8080"
ENV RECEIVER_URI="/upload"

WORKDIR /usr/src/app-sender

COPY ./sender ./sender/
COPY Pipfile ./
COPY Pipfile.lock ./

RUN mkdir -p ${STATUS_DB_PATH} \
    && mkdir -p ${INPUT_DIR}
RUN pip install --no-cache-dir pipenv
RUN pipenv install  --system --deploy --ignore-pipfile

ENTRYPOINT ["python3", "./sender/__main__.py"]
