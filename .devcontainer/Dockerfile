# Adapted from https://github.com/qdm12/basedevcontainer
ARG LOGOLS_VERSION=v1.3.7
FROM qmcgaw/binpot:logo-ls-${LOGOLS_VERSION} AS logo-ls

FROM mcr.microsoft.com/devcontainers/base:jammy

ARG DEBIAN_FRONTEND=noninteractive
ARG USER=vscode

RUN DEBIAN_FRONTEND=noninteractive \
    && apt-get update \ 
    && apt-get install -y build-essential --no-install-recommends make \
        ca-certificates \
        git \
        libssl-dev \
        zlib1g-dev \
        libbz2-dev \
        libreadline-dev \
        libsqlite3-dev \
        wget \
        curl \
        llvm \
        libncurses5-dev \
        xz-utils \
        tk-dev \
        libxml2-dev \
        libxmlsec1-dev \
        libffi-dev \
        liblzma-dev

# Python and poetry installation
USER $USER
ARG HOME="/home/$USER"
ARG PYTHON_VERSION=3.11
# ARG PYTHON_VERSION=3.10

ENV PYENV_ROOT="${HOME}/.pyenv"
ENV PATH="${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${HOME}/.local/bin:$PATH"

RUN echo "done 0" \
    && curl https://pyenv.run | bash \
    && echo "done 1" \
    && pyenv install ${PYTHON_VERSION} \
    && echo "done 2" \
    && pyenv global ${PYTHON_VERSION} \
    && echo "done 3" \
    && curl -sSL https://install.python-poetry.org | python3 - \
    && poetry config virtualenvs.in-project true

# Logo ls for pretty terminal
COPY --from=logo-ls /bin /usr/local/bin/logo-ls
RUN echo "alias ls='logo-ls'" >> ${HOME}/.zshrc && \
    sudo chown ${USER} /usr/local/bin/logo-ls
