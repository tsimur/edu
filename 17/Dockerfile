FROM ubuntu:20.04
RUN apt-get update && \
  apt-get install -y make gcc wget libgdbm-dev libssl-dev zlib1g-dev libreadline-dev && \
  wget https://cache.ruby-lang.org/pub/ruby/2.7/ruby-2.7.2.tar.gz && \
  tar -xzf ruby-2.7.2.tar.gz && \
  cd ruby-2.7.2 && \
  ./configure && \
  make && \
  make install
CMD [ "/bin/bash" ]