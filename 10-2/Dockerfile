FROM postgres:10

MAINTAINER Chia-liang Kao <clkao@clkao.org>

ENV PLV8_VERSION=v2.1.0 \
    PLV8_SHASUM="207d712e919ab666936f42b29ff3eae413736b70745f5bfeb2d0910f0c017a5c  v2.1.0.tar.gz"

ENV buildDependencies="build-essential \
    ca-certificates \
    curl \
    git-core \
    python \
    postgresql-server-dev-$PG_MAJOR" 
RUN apt-get update 
RUN apt-get install -y --no-install-recommends ${buildDependencies} 
RUN mkdir -p /tmp/build 
RUN curl -o /tmp/build/${PLV8_VERSION}.tar.gz -SL "https://github.com/plv8/plv8/archive/$PLV8_VERSION.tar.gz" 
WORKDIR /tmp/build 
RUN echo ${PLV8_SHASUM} | sha256sum -c 
RUN tar -xzf /tmp/build/${PLV8_VERSION}.tar.gz -C /tmp/build/ 
RUN cd plv8-${PLV8_VERSION#?} && make -j8 static
RUN cd plv8-${PLV8_VERSION#?} && make install 
RUN strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8.so 
WORKDIR / 
RUN apt-get clean 
RUN apt-get remove -y ${buildDependencies} 
RUN apt-get autoremove -y 
RUN rm -rf /tmp/build /var/lib/apt/lists/*
