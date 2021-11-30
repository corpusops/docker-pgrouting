FROM corpusops/postgis-bare:10-2.5 as builder
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
        graphviz             \
        texlive              \
        texlive-font-utils   \
        texlive-latex-extra  \
        ghostscript          \
        postgresql-server-dev-all \
#       python3-pip python-pip \
#       doxygen                \
#       python3-sphinx         \
'
# RUN bash -euc ': \
#     && rm /usr/bin/sphinx-build \
#     && ln -sfv /usr/local/bin/sphinx-build /usr/bin \
#     && python  -m pip install "sphinx>1.8" \
#     && python3 -m pip install "sphinx>1.8" \
#     '
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
ENV PG_MAJOR=10
ENV POSTGRES_MAJOR=10
ENV POSTGIS_MAJOR=2.5
ENV PGROUTING_MAJOR=2.4
ENV PGROUTING_VERSION=2.4.2-1
ENV PGROUTING_DEBIAN_VERSION=debian/2.4.2-1
ENV PGROUTING_UPSTREAM_VERSION=v2.4.2
ARG MAKEOPTS=-j4
WORKDIR /usr/src/pgrouting
ADD *.patch.* ../
RUN bash -euxc ': \
    && git reset --hard $PGROUTING_DEBIAN_VERSION \
    && use_upstream="" \
    && if [ "$POSTGRES_MAJOR-$POSTGIS_MAJOR-$PGROUTING_MAJOR" = "11-2.5-2.5" ];then use_upstream="1";fi \
    && if [ "x$use_upstream" = "x1" ];then \
        cp -rf debian debian.bak \
        && git reset --hard $(echo $PGROUTING_UPSTREAM_VERSION|sed -re "s/v2.5../v2.5.3/g") \
        && cp -rf debian.bak debian; \
    fi \
    && verlte() { [  "$1" = "`echo -e "$1\n$2" | sort -V | head -n1`" ]; } \
    && : fix https://github.com/pgRouting/pgrouting/issues/963 \
    && if ( verlte $PG_MAJOR 10 ) && ( verlte $PGROUTING_MAJOR 2.4 );then \
        bzip2 -ckd ../10.patch.bz2|patch -Np1; \
    fi'
RUN bash -euxc ': \
    && echo $PG_MAJOR > ~/.pg_supported_versions \
    && cat ~/.pg_supported_versions \
    && cp debian/control debian/control.bak\
    && cp debian/rules   debian/rules.bak\
    && sed -i -r \
    -e "s/postgresql-[0-9]+-postgis,/postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR-scripts postgresql-$PG_MAJOR-postgis-$POSTGIS_MAJOR,/g" \
    -e "/(python3-sphinx \(>= ).*\)/ d" \
    -e "s/postgresql-[0-9]+-/postgresql-$PG_MAJOR/g" \
    -e "s/postgis-[0-9]+\.[0-9]+/postgis-$POSTGIS_MAJOR/g" \
    -e "s/postgresql-[0-9]+-(pgrouting-scripts)/postgresql-$PG_MAJOR-\1/g" \
    -e "s/postgresql-PGVERSION-postgis,/postgresql-PGVERSION-postgis-$POSTGIS_MAJOR, postgresql-PGVERSION-postgis-$POSTGIS_MAJOR-scripts,/g" \
    -e  "/^Package:.*doc/,$ d" \
    debian/control* \
    && : "doc relies on sphinx, which do not built on old debs, in docker we wont need it, disabling." \
    && rm -f debian/postgresql-pgrouting-doc.doc-base.in \
    && sed -i -re "s/ --with sphinxdoc| -DWITH_DOC=ON//g" -e "/sphinx|doc-base|doc\/html|pgrouting-doc/ d" debian/rules \
    && : "rebuild glue" \
    && echo $PG_MAJOR > debian/pgversions \
    && pg_buildext updatecontrol \
    && ( diff -u debian/rules.bak   debian/rules  ||true )\
    && ( diff -u debian/control.bak debian/control||true )\
    && cat debian/control\
    '
RUN mk-build-deps -t "apt-get --no-install-recommends -y --force-yes -o Debug::pkgProblemResolver=yes" -ir debian/control
RUN debuild -i -us -uc -b

FROM corpusops/postgis-bare:10-2.5
MAINTAINER kiorky <kiorky@cryptelium.net>
ENV PGROUTING_MAJOR 2.4
ENV PGROUTING_VERSION 2.4.2-1
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
