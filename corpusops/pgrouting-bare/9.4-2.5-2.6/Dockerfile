FROM corpusops/postgis-bare:9.4-2.5 as builder
ARG NONINTERACTIVE=y
ADD helpers/ /tmp/copshelpers/
RUN cd /tmp/copshelpers && ./up.sh \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*
MAINTAINER kiorky <kiorky@cryptelium.net>
RUN bash -euxc ': \
    && apt-get update -qq \
    && apt-get install -y --force-yes build-essential cmake \
               devscripts equivs lsb-release \
               dbconfig-common python-twisted-web daemon'
RUN bash -euc ': \
    && apt-get install -y --force-yes \
        cmake                \
        libboost-thread-dev  \
        libboost-graph-dev   \
        libcgal-dev          \
        python-sphinx        \
        graphviz             \
        texlive              \
        texlive-font-utils   \
        texlive-latex-extra  \
        ghostscript          \
        postgresql-server-dev-all \
    '
ENV PGROUTING_REPO=https://salsa.debian.org/debian-gis-team/pgrouting.git
ENV PGROUTING_UPSTREAM_REPO=https://github.com/pgRouting/pgrouting.git
RUN bash -euxc ': \
    && apt-get install -y --force-yes \
        git                  \
        ca-certificates      \
    && git clone $PGROUTING_REPO /usr/src/pgrouting \
    && cd /usr/src/pgrouting \
    && git remote add pgrouting $PGROUTING_UPSTREAM_REPO \
    && git fetch pgrouting \
    '
ENV PG_MAJOR=9.4
ENV POSTGRES_MAJOR=9.4
ENV POSTGIS_MAJOR=2.5
ENV PGROUTING_MAJOR=2.6
ENV PGROUTING_VERSION=2.6.2-1
ENV PGROUTING_DEBIAN_VERSION=debian/2.6.2-1
ENV PGROUTING_UPSTREAM_VERSION=v2.6.2
ARG MAKEOPTS=-j4
WORKDIR /usr/src/pgrouting
ADD *.patch.* ../
RUN bash -euxc ': \
    && git reset --hard $PGROUTING_DEBIAN_VERSION \
    && : cp -rf debian debian.bak \
    && : git reset --hard $PGROUTING_UPSTREAM_VERSION \
    && : cp -rf debian.bak debian \
    && verlte() { [  "$1" = "`echo -e "$1\n$2" | sort -V | head -n1`" ]; } \
    && : fix https://github.com/pgRouting/pgrouting/issues/963 \
    && if ( verlte $PG_MAJOR 10 ) && ( verlte $PGROUTING_MAJOR 2.4 );then \
        bzip2 -ckd ../10.patch.bz2|patch -Np1; \
    fi'
RUN bash -euxc ': \
    && sed -i -r \
         -e "s/postgresql-[0-9]+-postgis,/postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR,/g" \
         -e "s/postgresql-[0-9]+-/postgresql-$PG_MAJOR/g" \
         -e "s/postgis-[0-9]+\.[0-9]+/postgis-$POSTGIS_MAJOR/g" \
         -e "s/postgresql-[0-9]+-(pgrouting-scripts)/postgresql-$PG_MAJOR-\1/g" \
         -e "s/postgresql-PGVERSION-postgis,/postgresql-PGVERSION-postgis-$POSTGIS_MAJOR, postgresql-PGVERSION-postgis-$POSTGIS_MAJOR-scripts,/g" \
        debian/control* \
    && echo $PG_MAJOR > debian/pgversions \
    && pg_buildext updatecontrol \
    && cat debian/control \
    && mk-build-deps -t "apt-get --no-install-recommends -y --force-yes" -ir debian/control'
RUN bash -euxc ': \
    && debuild -i -us -uc -b'

FROM corpusops/postgis-bare:9.4-2.5
MAINTAINER kiorky <kiorky@cryptelium.net>
ENV PGROUTING_MAJOR 2.6
ENV PGROUTING_VERSION 2.6.2-1
COPY --from=builder /usr/src/*deb /tmp/
COPY ./initdb-pgrouting.sh /docker-entrypoint-initdb.d/routing.sh
RUN bash -euxc ': \
    && ls /tmp/*deb \
    && apt-get update \
    && apt install -y --no-install-recommends \
        $( ls /tmp/postgresql-$PG_MAJOR-pgrouting*deb -1 | grep -v dbg ) \
    && mkdir -p /docker-entrypoint-initdb.d \
    && rm -rvf /var/lib/apt/lists/* /tmp/*deb'
LABEL com.github.corpusops.docker-images-commit="$DOCKER_IMAGES_COMMIT"
ARG DOCKER_IMAGES_COMMIT=master
