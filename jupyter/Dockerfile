FROM ubuntu:16.04

FROM alpine:3.8

COPY --from=0 /etc/apt/sources.list /etc/apt/sources.list

RUN apk add --no-cache curl jq && \
    export COUNTRY=$(curl ipinfo.io | jq '.country' | tr -d '"') && \
    sed -i "s|http:\/\/security|http:\/\/$COUNTRY.archive|g" /etc/apt/sources.list && \
    sed -i "s|http:\/\/archive|http:\/\/$COUNTRY.archive|g" /etc/apt/sources.list
#"

FROM ubuntu:16.04

COPY --from=1 /etc/apt/sources.list /etc/apt/sources.list

# install package
RUN apt-get update && apt-get install -y --no-install-recommends \
    software-properties-common \
    build-essential \
    python \
    python-dev \
    python3 \
    python3-dev \
    pkg-config \
    libpng-dev \
    libzmq3-dev \
    libpq-dev \
    rsync \
    curl \
    wget \
    unzip \
    iputils-ping \
    net-tools \
    netcat \
    git \
    openssh-server \
    vim \
    pandoc \
    p7zip-full \
    tree \
    apache2 \
    nginx 

# set python env
ENV PYTHONPATH /usr/local/python:$PYTHONPATH

RUN wget --quiet https://github.com/krallin/tini/releases/download/v0.10.0/tini && \
    mv tini /usr/bin/tini && \
    chmod +x /usr/bin/tini

RUN wget --quiet -L https://bootstrap.pypa.io/get-pip.py && \
    python3 get-pip.py && \
    python2 get-pip.py && \
    rm get-pip.py

RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - \
    && \
    apt-get update && apt-get install -y --no-install-recommends \
    nodejs \
    && \
    apt-get clean \
    && \
    rm -rf /var/lib/apt/lists/*

# install python's package
RUN python3 -m pip --no-cache-dir install \
    git+git://github.com/powerline/powerline \
    selenium \
    psycopg2-binary \
    psycopg2 \
    Pillow \
    h5py \
    ipykernel \
    jupyter \
    notebook \
    matplotlib \
    numpy \
    pandas \
    scipy \
    sklearn \
    flask \
    pymongo \
    redis \
    requests \
    ipyparallel \
    bs4 \
    nbconvert \
    pandoc \
    django \
    selenium \
    tornado==5.1.1 \
    jupyterhub \
    && \
    python3 -m ipykernel.kernelspec

RUN npm install -g --unsafe-perm=true \
    @vue/cli \
    @vue/cli-service-global \
    ijavascript \
    && \
    ijsinstall \
    && \
    rm -rf ~/.npm

# link python3 and python exe
RUN ln -s -f /usr/bin/python3 /usr/bin/python

# copyt jupyter notebook config
COPY jupyter_notebook_config.py /root/.jupyter/

COPY start.sh /usr/local/bin/
COPY start-notebook.sh /usr/local/bin/
COPY start-singleuser.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/start.sh
RUN chmod +x /usr/local/bin/start-notebook.sh
RUN chmod +x /usr/local/bin/start-singleuser.sh

EXPOSE 6006 8888

ENTRYPOINT ["tini", "--"]

CMD ["start-notebook.sh"]
