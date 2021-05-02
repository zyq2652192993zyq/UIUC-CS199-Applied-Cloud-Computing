FROM ubuntu:18.04

LABEL "NETID"="123456"
LABEL "CLASS"="CS199"

RUN mkdir data
RUN cd data/ && touch info.txt && echo 'Karl the fog is the best cloud computing platform' > info.txt

RUN useradd --create-home --no-log-init --shell /bin/bash cs199
RUN adduser cs199 sudo
RUN echo 'cs199:password' | chpasswd

USER cs199
ENV NAME yuqi
