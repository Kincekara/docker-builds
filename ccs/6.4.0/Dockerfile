FROM ubuntu:jammy as app

ARG CCS_VER="6.4.0"

LABEL base.image="ubuntu:jammy"
LABEL dockerfile.version="1"
LABEL software="CSS"
LABEL software.version="${CCS_VER}"
LABEL description="ccs combines multiple subreads of the same SMRTbell molecule using a statistical model to produce HiFi read."
LABEL website="https://ccs.how/"
LABEL license="https://github.com/PacificBiosciences/ccs/blob/develop/LICENSE"
LABEL maintainer="Kutluhan Incekara"
LABEL maintainer.email="kutluhan.incekara@ct.gov"

RUN apt-get update && apt-get install --no-install-recommends -y \
    wget &&\
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN wget --no-check-certificate https://github.com/PacificBiosciences/ccs/releases/download/v${CCS_VER}/ccs.tar.gz &&\
    tar --no-same-owner -C /usr/local/bin/ -xvf ccs.tar.gz &&\
    rm ccs.tar.gz &&\
    chmod +x /usr/local/bin/ccs-alt

ENV LC_ALL=C

CMD [ "ccs", "-h" ]

WORKDIR /data

## Test ##
FROM app as test

RUN ccs --version && ccs --help