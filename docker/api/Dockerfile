FROM python:3.11-slim-buster

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWIRTEBYTECODE=1 \
    PYSETUP_PATH=/opt/pysetup \
    APPLICATION_PATH=/usr/app
ENV VENV_PATH=$PYSETUP_PATH/.venv
ENV PATH=$VENV_PATH/bin:$PATH

RUN apt-get update && apt-get install -y --no-install-recommends libpq5

ENV PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    POETRY_HOME=/opt/poetry/ \
    POETRY_VIRTUALENVS_IN_PROJECT=true \
    POETRY_NO_INTERACTION=1 \
    POETRY_VERSION=1.5.0
ENV PATH=$POETRY_HOME/bin:$PATH

WORKDIR $PYSETUP_PATH

RUN apt-get update -y  \
    && apt-get install -y build-essential libpq-dev curl  \
    && curl -sSL https://install.python-poetry.org | python

COPY poetry.lock pyproject.toml ./

RUN poetry install -n --no-root --no-dev

COPY . /home
WORKDIR /home

RUN chmod +x /home/docker/api/startup.sh
ENTRYPOINT /home/docker/api/startup.sh