FROM golang:1.12.9-stretch as built

ENV DEBIAN_FRONTEND noninteractive

WORKDIR /
RUN apt-get update &&\
    apt-get install -y apt-utils git software-properties-common libffi-dev \
    inetutils-tools wget ca-certificates curl build-essential libssl-dev make
# builds out geth
RUN git clone https://github.com/ethereum/go-ethereum.git

WORKDIR /go-ethereum

RUN make geth

FROM ubuntu:18.04 as final

COPY --from=built /go-ethereum/build/bin/geth /usr/local/bin/geth

RUN apt-get update && apt-get install --no-install-recommends -y iperf3 openssh-server iputils-ping tmux &&\
	apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /

ENTRYPOINT ["/bin/bash"]
