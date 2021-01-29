# What is Constellio

Constellio eim is an open source solution of enterprise information management offering the following key functionnalities:

- Document management
- Record Information Management
- Enterprise search

Learn more on [Constellio's homepage](https://constellio.com/) homepage and in the [Constellio documentation](https://documentation.constellio.com/).


# Getting started with the Docker image

This image is dependent on the [Constellio build for Solr](https://github.com/lracicot/docker-constellio-solr).

## Running Constellio

```bash
export ADMIN_SERVICE_KEY=adminkey
export ADMIN_PASSWORD=password
export SOLR_URL=http://solr-server-address:8983/solr
docker run -d -p 80:8080 --name constellio -e SOLR_URL=$SOLR_URL -e ADMIN_SERVICE_KEY=$ADMIN_SERVICE_KEY -e ADMIN_PASSWORD=$ADMIN_PASSWORD lracicot/constellio:9.1

```

# Using docker-compose

```yaml
version: '3'

services:
  constellio:
    image: lracicot/constellio:9.1
    links:
      - solr
    environment:
      - SOLR_URL=http://solr:8983/solr
      - ADMIN_SERVICE_KEY=adminkey
      - ADMIN_PASSWORD=password
    ports:
      - 80:8080
    volumes:
      - constellio-conf:/opt/constellio/conf
      - constellio-contents:/opt/constellio/contents
      - constellio-logs:/opt/constellio/logs
      - constellio-transaction_log:/opt/constellio/transaction_log
      - constellio-work:/opt/constellio/work

  solr:
    build:
      image: lracicot/constellio-solr:9.1-5.5
    volumes:
      - solr-records:/opt/solr/server/solr/configsets/records_configs/data
      - solr-notifications:/opt/solr/server/solr/configsets/notifications_configs/data
      - solr-events:/opt/solr/server/solr/configsets/events_configs/data
    ports:
      - 8983:8983

volumes:
  constellio-conf:
  constellio-contents:
  constellio-logs:
  constellio-transaction_log:
  constellio-work:
  solr-records:
  solr-notifications:
  solr-events:

```

# Contribute

This is not an official Constellio image. This image is provided as-is, is maintained by a volunteer and does not provide any warranty. Use it at your own risk. If you find any bugs or want to improve it, feel free to open an issue and/or submit a PR on GitHub.

For official support and hosting, visit the [Constellio website](https://constellio.com/).

# Included third party licenses

## Constellio (https://github.com/doculibre/constellio/blob/master/LICENSE.txt)

```
Constellio - License

Constellio is licensed under the terms of the GNU AFFERO GENERAL PUBLIC LICENSE v3, or
alternatively under the terms of the LiLiQ-R+ v1.1.
You may use Constellio according to either of these licenses as is most appropriate
for your project on a case-by-case basis.

The terms of each license can be found in the main directory of the Constellio source
repository:

GNU AFFERO GENERAL PUBLIC LICENSE v3: https://github.com/doculibre/constellio/blob/master/LICENSE.AGPL3
LiLiQ-R+ v1.1: https://github.com/doculibre/constellio/blob/master/LICENSE.LiLiQ-R+1-1
```

## LibreOffice (https://www.libreoffice.org/about-us/licenses)

```
Covered Software is provided under this License on an “as is” basis, without warranty of any kind, either expressed, implied, or statutory, including, without limitation, warranties that the Covered Software is free of defects, merchantable, fit for a particular purpose or non-infringing. The entire risk as to the quality and performance of the Covered Software is with You. Should any Covered Software prove defective in any respect, You (not any Contributor) assume the cost of any necessary servicing, repair, or correction. This disclaimer of warranty constitutes an essential part of this License. No use of any Covered Software is authorized under this License except under this disclaimer.
```

## Wrapper (https://wrapper.tanukisoftware.com/doc/english/licenseCommunity.html)

```
Disclaimer of Warranty.

THERE IS NO WARRANTY FOR THE PROGRAM, TO THE EXTENT PERMITTED BY
APPLICABLE LAW. EXCEPT WHEN OTHERWISE STATED IN WRITING THE
COPYRIGHT HOLDERS AND/OR OTHER PARTIES PROVIDE THE PROGRAM "AS IS"
WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED OR IMPLIED,
INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE. THE ENTIRE
RISK AS TO THE QUALITY AND PERFORMANCE OF THE PROGRAM IS WITH YOU.
SHOULD THE PROGRAM PROVE DEFECTIVE, YOU ASSUME THE COST OF ALL
NECESSARY SERVICING, REPAIR OR CORRECTION.
```
