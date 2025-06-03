FROM python:3.11-slim

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_NO_WARN_SCRIPT_LOCATION=0 \
    PIP_ROOT_USER_ACTION=ignore \
    AIOHTTP_NO_EXTENSIONS=1 \
    MULTIDICT_NO_EXTENSIONS=1 \
    YARL_NO_EXTENSIONS=1
ENV TGTG_TOKEN_PATH=/tokens
ENV LOGS_PATH=/logs
ENV DOCKER=true
ENV UID=1000
ENV GID=1000

RUN addgroup --gid ${GID} tgtg && \
    adduser --shell /bin/false \
            --disabled-password \
            --uid ${UID} \
            --gid ${GID} \
            tgtg
RUN mkdir -p ${LOGS_PATH}
RUN mkdir -p ${TGTG_TOKEN_PATH}
RUN chown tgtg:tgtg ${LOGS_PATH}
RUN chown tgtg:tgtg ${TGTG_TOKEN_PATH}
VOLUME ${TGTG_TOKEN_PATH}

WORKDIR /app  # Set the working directory

# Copy the requirements.txt file into the image
COPY requirements.txt /app/requirements.txt

# Install dependencies
RUN pip install -r /app/requirements.txt

RUN ls -l

# Copy the tgtg_scanner directory into the image
COPY tgtg_scanner /app/tgtg_scanner

# Install the tgtg_scanner
RUN pip install /app/tgtg_scanner

COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "python", "-m", "tgtg_scanner" ]
