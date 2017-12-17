FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

MAINTAINER Reck Mi <fengbiaoshan@gmail.com>

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update --fix-missing && apt-get install -y wget vim bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial

RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh && \
    wget --quiet https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /opt/conda && \
    rm ~/anaconda.sh

RUN apt-get install -y curl grep sed dpkg && \
    TINI_VERSION=`curl https://github.com/krallin/tini/releases/latest | grep -o "/v.*\"" | sed 's:^..\(.*\).$:\1:'` && \
    curl -L "https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini_${TINI_VERSION}.deb" > tini.deb && \
    dpkg -i tini.deb && \
    rm tini.deb && \
    apt-get clean

RUN /opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks

RUN /opt/conda/bin/pip install tensorflow-gpu

RUN rm -rf /opt/notebooks
VOLUME /opt/notebooks
WORKDIR /opt/notebooks

COPY run_jupyter.sh /

RUN chmod +x /run_jupyter.sh

ENV PATH /opt/conda/bin:$PATH

ENV LD_LIBRARY_PATH /usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH

ENTRYPOINT [ "/usr/bin/tini", "--" ]

CMD ["/run_jupyter.sh"]
