FROM mcr.microsoft.com/devcontainers/python:3.11

ENV PYTHONUNBUFFERED=1

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get --yes install --no-install-recommends \
    # https://docs.brew.sh/Homebrew-on-Linux#requirements
    build-essential \
    curl \
    file \
    git \
    procps \

    # network tools
    iputils-ping \
    traceroute \

    # database tools
    postgresql-client \
    redis-tools

# Microsoft's Python image comes with a nonroot user named vscode. This is super convenient
# because Codespaces will automatically switch to this user, and also because pips and homebrew
# should ideally be installed as nonroot.

USER vscode
RUN NONINTERACTIVE=1 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)" \
    && (echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/vscode/.profile \
    && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" \
    && brew install hello \
    && brew autoremove \
    && brew cleanup

USER root
COPY requirements.txt /tmp/pip-tmp/

USER vscode
RUN pip --no-cache-dir install --no-warn-script-location --upgrade pip setuptools wheel \
    && pip --no-cache-dir install --no-warn-script-location --requirement /tmp/pip-tmp/requirements.txt \
    && sudo rm -rf /tmp/pip-tmp
