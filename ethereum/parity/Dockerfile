FROM ubuntu:18.04 as built

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y --no-install-recommends git wget ca-certificates

RUN wget https://releases.parity.io/ethereum/v2.6.5/x86_64-unknown-linux-gnu/parity
RUN mv ./parity /usr/local/bin/ && chmod +x /usr/local/bin/parity

FROM ubuntu:18.04 as final

RUN apt-get update && apt-get install --no-install-recommends -y iperf3 openssh-server iputils-ping tmux &&\
	apt-get clean && rm -rf /var/lib/apt/lists/*

COPY --from=built /usr/local/bin/parity /usr/local/bin/parity
WORKDIR /

EXPOSE 8080 8545 8180
ENTRYPOINT ["/bin/bash"]
