# Docker pgRouting images
- You should be more interrested on [corpusops/pgrouting](https://hub.docker.com/r/corpusops/pgrouting) which is based on this image but built via this repo [corpusops/docker-postgresql](https://github.com/corpusops/docker-postgresql)
- This repository provides tags for the following docker images:
    - [corpusops/pgrouting-bare](https://hub.docker.com/r/corpusops/pgrouting-bare)
- Those images are based on
    [postgres](https://github.com/docker-library/postgres) and
    [starefossen/pgrouting](https://github.com/Starefossen/docker-pgrouting) Docker Images.

- And the reason for those to exist is to explicitly tag version tags and avoid non wanted production upgrades.

## Credits & documentation on image usage
- [For **PGROUTING** based / Inspired from this image](https://github.com/Starefossen/docker-pgrouting)

## Supported tags

### pgrouting
- The following `corpusops/pgrouting-bare:<tag>` Docker Image tags are supported. <br/>
  The form is ``corpusops/pgrouting-bare:<POSTGIS_VER>-<POSTGIS_VER>-<PGROUTING_VER>``
    - [see tags](https://hub.docker.com/r/corpusops/pgrouting-bare/tags?page=1&ordering=last_updated)
    - Also:
        - `corpusops/pgrouting-bare:13-3-3.1`   == `corpusops/pgrouting-bare:13`
        - `corpusops/pgrouting-bare:13-3-3.1`   == `corpusops/pgrouting-bare:13-3`
        - `corpusops/pgrouting-bare:12-3-3.1`   == `corpusops/pgrouting-bare:12-3`
        - `corpusops/pgrouting-bare:12-3-3.1`   == `corpusops/pgrouting-bare:12`
        - `corpusops/pgrouting-bare:11-3-3.1`   == `corpusops/pgrouting-bare:11-3`
        - `corpusops/pgrouting-bare:12-2.5-2.6` == `corpusops/pgrouting-bare:11-2.5`
        - `corpusops/pgrouting-bare:11-2.5-2.6` == `corpusops/pgrouting-bare:11-2.5`
        - `corpusops/pgrouting-bare:10-2.5-2.6` == `corpusops/pgrouting-bare:10-2.5`
        - `corpusops/pgrouting-bare:10-2.4-2.6` == `corpusops/pgrouting-bare:10-2.4`
        - `corpusops/pgrouting-bare:11-2.5-2.6` == `corpusops/pgrouting-bare:11`
        - `corpusops/pgrouting-bare:10-2.5-2.6` == `corpusops/pgrouting-bare:10`
