####################################################
FROM python:3 as base_image

ENV SERVER_IP="0.0.0.0"
ENV SERVER_PORT="8080"
ENV OUTPUT_DIR="/usr/src/app-receiver/output"
ENV DECRYPTION_KEY_DIR="/run/secrets"
ENV DECRYPTION_KEY="${DECRYPTION_KEY_DIR}/decryption_key"

RUN mkdir -p ${OUTPUT_DIR}

COPY receiver ./
COPY Pipfile ./
COPY Pipfile.lock ./

RUN pip install --no-cache-dir pipenv
RUN pipenv install --system --deploy --ignore-pipfile

FROM base_image as development_image
COPY receiver ./receiver/
COPY tests ./tests/
RUN pip install pytest
CMD ["pytest", "-v", "tests"]

FROM base_image as production_image
EXPOSE 8080
ENTRYPOINT ["gunicorn", "server:app"]

