FROM postgres:10

MAINTAINER Chia-liang Kao <clkao@clkao.org>

ENV PLV8_VERSION=v2.3.8 \
    PLV8_SHASUM="b3ffb95daeb21b17bc5670f372403879dfad7eee86e0152e4da763ffd16fbf07  v2.3.8.tar.gz"

RUN buildDependencies="build-essential \
    ca-certificates \
    curl \
    git-core \
    python \
    postgresql-server-dev-$PG_MAJOR" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies} \
  && mkdir -p /tmp/build \
  && curl -o /tmp/build/${PLV8_VERSION}.tar.gz -SL "https://github.com/plv8/plv8/archive/$PLV8_VERSION.tar.gz" \
  && cd /tmp/build \
  && echo ${PLV8_SHASUM} | sha256sum -c \
  && tar -xzf /tmp/build/${PLV8_VERSION}.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-${PLV8_VERSION#?} \
  && sed -i 's/\(depot_tools.git\)/\1; cd depot_tools; git checkout 46541b4996f25b706146148331b9613c8a787e7e; rm -rf .git;/' Makefile.v8 \
  && make static \
  && make install \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8.so \
  && cd / \
  && apt-get clean \
  && apt-get remove -y ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmp/build /var/lib/apt/lists/*
