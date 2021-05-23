# Auto-deployment for matrix-org/synapse
[![Build Status](https://travis-ci.com/buluma/matrix-synapse-auto-deploy.svg?branch=master)](https://travis-ci.com/buluma/matrix-synapse-auto-deploy)
[![HitCount](http://hits.dwyl.com/buluma/matrix-synapse-auto-deploy.svg)](http://hits.dwyl.com/buluma/matrix-synapse-auto-deploy)

[![Maintenance](https://img.shields.io/maintenance/yes/2020.svg)](https://github.com/Madic-/matrix-synapse-auto-deploy) [![Build Status](https://travis-ci.org/Madic-/matrix-synapse-auto-deploy.svg?branch=master)](https://travis-ci.org/Madic-/matrix-synapse-auto-deploy) [![HitCount](http://hits.dwyl.com/Madic-/matrix-synapse-auto-deploy.svg)](http://hits.dwyl.com/Madic-/matrix-synapse-auto-deploy)

This ansible role will automatically deploy a ready-to-go [matrix](http://matrix.org/) server on any server. It differs from the [matrix-docker-ansible-deploy](https://github.com/spantaleev/matrix-docker-ansible-deploy) roles by not using docker but instead a python3 virtual environment, at least for synapse.

## Services

This role configures the following services on your server:

- [Synapse](https://github.com/matrix-org/synapse): Reference "homeserver" implementation of Matrix from the core development team at matrix.org

- [Coturn](https://github.com/coturn/coturn): STUN/TURN server for WebRTC audio/video calls

- [mxisd](https://github.com/kamax-io/mxisd): Federated Matrix Identity server, to further increase privacy ([doc](docs/mxisd.md))

- [nginx](http://nginx.org/): Web server for riot.web and reverse proxy for synapse and mxisd

- [postgresql](https://www.postgresql.org/): Database for synapse and mxisd

- [Riot](https://riot.im/): WebUI preconfigured for your homeserver

- [Let's Encrypt](https://letsencrypt.org/): TLS certificate for Riot and Synapse

Small [Architecture Overview](docs/architecture.md)

## Pre-requirements

- Git
- Ansible >= 2.6
- DNS Entries
  - A Records
    - A Record for matrix-machine.yourdomain.tld.
    - A Record for riot-webclient.yourdomain.tld.
  - SRV Record
    - `_matrix._tcp.yourdomain.tld. 3600 IN SRV 10 5 443 matrix-machine.yourdomain.tld.`
    - `_matrix-identity._tcp.yourdomain.tld. 3600 IN SRV 10 5 443 matrix-machine.yourdomain.tld.`

You should have an SRV entry like that in order to tell other HomeServers on which port they need to speak.
Additionally .well-known files will be created under {{ matrix_well_known_location }}. It's up to you to move these files to the server serving your apex domain.

## Supported OS

- Ubuntu 18.04
- Debian 9

It should also run smoothly on any systemd flavoured OS. You're free to test and give me feedback (or PR to add support for your favorite system).

## Installation

All variables are defined under [roles/matrix-synapse-auto-deploy/defaults/main.yml](roles/matrix-synapse-auto-deploy/defaults/main.yml). You need to adapt to your needs. Take a look at the [playbook.example.yml](playbook.example.yml) file for the most important variables.

    ansible-playbook playbook.example.yml --extra-vars "host=matrix.domain.com" -b -K

## Enjoy

You can now connect to your Homeserver via the riot webclient or by specifying your Homeserver on any other client.
