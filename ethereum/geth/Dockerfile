FROM ubuntu:18.04 as built

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update &&\
    apt-get install -y apt-utils expect git git-extras software-properties-common \
    inetutils-tools wget ca-certificates curl build-essential libssl-dev golang-go make
# builds out geth
RUN git clone https://github.com/spapinistarkware/go-ethereum.git
WORKDIR /go-ethereum
RUN git checkout spapini/increase-tx-size-limit &&\
        cd /go-ethereum &&\
        make all


FROM gcr.io/whiteblock/base:ubuntu18.04 as final

COPY --from=built /go-ethereum/build/bin /usr/local/bin

RUN apt-get update &&\
    apt-get install -y software-properties-common &&\
    add-apt-repository ppa:ethereum/ethereum && \
    apt-get update && \
    apt-get install -y solc

RUN ln -s /go-ethereum/build/bin/geth /usr/bin/geth

WORKDIR /
#ENV PATH /go-ethereum/build/bin:${PATH}

ENTRYPOINT ["/bin/bash"]
