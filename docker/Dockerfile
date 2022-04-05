FROM python:3

USER root

ENV PYTHONUNBUFFERED=rununbuffered \
  PYTHONIOENCODING=utf-8 \
  SHELL=/bin/bash \
  LC_ALL=en_US.UTF-8 \
  LANG=en_US.UTF-8 \
  LANGUAGE=en_US.UTF-8 \
  PYTHONFAULTHANDLER=1 \
  PYTHONHASHSEED=random \
  PIP_NO_CACHE_DIR=off \
  PIP_DISABLE_PIP_VERSION_CHECK=on \
  PIP_DEFAULT_TIMEOUT=100 \
  MAX_POETRY_VERSION=2 \
  DOCKERIZE_VERSION=v0.6.1

ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
  && apt-get install -y \
  wget \
  locales \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  && echo "en_US.UTF-8 UTF-8" > /etc/locale.gen \
  && locale-gen

RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
  && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
  && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
  # Enable prompt color in the skeleton .bashrc before creating the default USERNAME
  && sed -i 's/^#force_color_prompt=yes/force_color_prompt=yes/' /etc/skel/.bashrc

RUN pip install -U "poetry<$MAX_POETRY_VERSION" \
  && poetry config virtualenvs.create false

ARG PROJECT_NAME='changesetmd'
ARG PROJECT_DIR='/code'
ARG USER_ID=999

ENV WORKDIR=${PROJECT_DIR}/${PROJECT_NAME}
WORKDIR ${PROJECT_DIR}

COPY ./poetry.lock ./pyproject.toml ${PROJECT_DIR}/
RUN poetry install --no-interaction --no-ansi

WORKDIR ${WORKDIR}

COPY . ${WORKDIR}

ENV USER_ID=${USER_ID}
USER ${USER_ID}