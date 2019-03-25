# Docker postgis & pgRouting images
- This repository provides tags for the following docker images:
    - [corpusops/postgis-bare](https://hub.docker.com/r/corpusops/postgis-bare)
    - [corpusops/pgrouting-bare](https://hub.docker.com/r/corpusops/pgrouting-bare)
- The additional tags from [corpusops/docker-images](https://github.com/corpusops/docker-images) are based on those images:
    - [corpusops/postgis](https://hub.docker.com/r/corpusops/postgis)
    - [corpusops/pgrouting](https://hub.docker.com/r/corpusops/pgrouting)


- Those images are based on
    [postgres](https://github.com/docker-library/postgres) and
    [mdillon/postgis](https://github.com/md5/docker-postgis) and
    [starefossen/pgrouting](https://github.com/Starefossen/docker-pgrouting) Docker Images.

- And the reason for those to exist is to explicitly tag version tags and avoid non wanted production upgrades.

## Credits & documentation on image usage
- [For **PGROUTING** based / Inspired from this image](https://github.com/Starefossen/docker-pgrouting)
- [And For **POSTGIS** based / inspired from this image](https://github.com/appropriate/docker-postgis)

## Supported tags
- The following `corpusops/postgis-bare:<tag>` Docker Image tags are supported. <br/>
  The form is ``corpusops/postgis-bare:<POSTGIS_VER>-<POSTGIS_VER>``
  - 9 (copy of latest 9.x tag)
    - 9-2.5
    - 9.4-2.4
    - 9.6-2.4
    - 9.5-2.4
    - 9.4-2.5
    - 9.5-2.5
    - 9.6-2.5
  - 10 (copy of latest 10.x tag)
    - 10-2.4
    - 10-2.5
  - 11 (copy of latest 11.x tag)
    - 11-2.5

## Support development
- [paypal](https://paypal.me/kiorky)