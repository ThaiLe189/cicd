FROM python:3.10-slim as builder

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y \
    # curl \
    # libpcap-dev  \
    # libpq-dev \
    # libgl1-mesa-dev \
    # build-essential \
    # libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

RUN python3 -m venv /opt/venv 
ENV PATH="/opt/venv/bin:$PATH"

RUN --mount=type=cache,target=/root/.cache/pip pip3 install --no-cache-dir --upgrade pip setuptools wheel

COPY ./app/requirements.txt ./

RUN --mount=type=cache,target=/root/.cache/pip \
    pip3 install -r requirements.txt

FROM python:3.10-slim

RUN apt-get update && apt-get install --no-install-recommends --no-install-suggests -y \
    # curl \
    # libpcap-dev  \
    # libpq-dev \
    # libgl1-mesa-dev \
    # build-essential \
    # libglib2.0-0 \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /app
WORKDIR /app

COPY --from=builder /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"
COPY . .

ENTRYPOINT [ "" ]

CMD ["gunicorn", "-c", "gunicorn.conf.py", "--log-config", "gunicorn.conf", "main:app"]
