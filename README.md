# Docker pgRouting images
- This repository provides tags for the following docker images:
    - [corpusops/pgrouting-bare](https://hub.docker.com/r/corpusops/pgrouting-bare)
- The additional tags from [corpusops/docker-images](https://github.com/corpusops/docker-images) are based on those images:
    - [corpusops/pgrouting](https://hub.docker.com/r/corpusops/pgrouting)


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
    - 9 (copy of latest 9.x tag)
        - 9.4-2.4 (copy of latest 9.4-2.4 tag)
            - 9.4-2.4-2.4
            - 9.4-2.4-2.5
            - 9.4-2.4-2.6
        - 9.5-2.4 (copy of latest 9.5-2.4 tag)
            - 9.5-2.4-2.4
            - 9.5-2.4-2.5
            - 9.5-2.4-2.6
        - 9.6-2.4 (copy of latest 9.6-2.4 tag)
            - 9.6-2.4-2.4
            - 9.6-2.4-2.5
            - 9.6-2.4-2.6
        - 9.4-2.5 (copy of latest 9.4-2.5 tag)
            - 9.4-2.5-2.4
            - 9.4-2.5-2.5
            - 9.4-2.5-2.6
        - 9.5-2.5 (copy of latest 9.5-2.5 tag)
            - 9.5-2.5-2.4
            - 9.5-2.5-2.5
            - 9.5-2.5-2.6
        - 9.6-2.5 (copy of latest 9.6-2.5 tag)
            - 9.6-2.5-2.4
            - 9.6-2.5-2.5
            - 9.6-2.5-2.6
    - 10 (copy of latest 10 tag)
        - 10-2.4 (copy of latest 10-2.4 tag)
            - 10-2.4-2.4
            - 10-2.4-2.5
            - 10-2.4-2.6
        - 10-2.5 (copy of latest 10-2.5 tag)
            - 10-2.5-2.4
            - 10-2.5-2.5
            - 10-2.5-2.6
    - 11 (copy of latest 11 tag)
        - 11-2.5 (copy of latest 11.x-2.5 tag)
            - 11-2.5-2.4
            - 11-2.5-2.5
            - 11-2.5-2.6
  - Notes:
      - ``pgrouting > 2.6 : postgis >= 2.2``
      - ``pgrouting < 2.5 : postgis >= 2.0``

## Support development
- [paypal](https://paypal.me/kiorky)
