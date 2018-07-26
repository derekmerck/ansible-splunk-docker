Splunk-Docker
=============

[![Build Status](https://travis-ci.org/derekmerck/ansible-splunk-docker.svg?branch=master)](https://travis-ci.org/derekmerck/ansible-splunk-docker)

Configure and run a Splunk data index in a Docker container.


Requirements
------------

Requires Docker, Docker-py and the `docker_image` and `docker_container` modules.


Role Variables
--------------

### Docker Image and Tag

Set the Splunk version tag.

```yaml
splunk_docker_image_tag:   "latest"
```

This probably only works with Splunk v7 or later, because they changed the mechanism for setting the initial administrator password.

### Docker Container Configuration

```yaml
splunk_container_name:     "splunk"
splunk_use_data_container: True
splunk_data_dir:           "/opt/splunk"
splunk_http_port:          8000
splunk_admin_port:         8088
splunk_hec_token:          8089
splunk_container_timezone: "America/New_York"
```

### Service Configuration

```yaml
splunk_password:            "passw0rd!"
```

Dependencies
------------

- `geerlingguy.docker` to setup the docker environment

Example Playbook
----------------

```yaml
- hosts: servers
  roles:
     - derekmerck.splunk-docker
```


License
-------

MIT

Author Information
------------------

Derek Merck  
<derek_merck@brown.edu>  
Rhode Island Hospital and Brown University  
Providence, RI  