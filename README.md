Ansible Role for Splunk in Docker
=================================

[![Build Status](https://travis-ci.org/derekmerck/ansible-splunk-docker.svg?branch=master)](https://travis-ci.org/derekmerck/ansible-splunk-docker)

Derek Merck  
<derek_merck@brown.edu>  
Rhode Island Hospital and Brown University  
Providence, RI  

Configure and run a [Splunk](http://www.splunk.com) data index in a Docker container.


Dependencies
------------

### Galaxy Roles

- [geerlingguy.docker](https://github.com/geerlingguy/ansible-role-docker) to setup the docker environment
- [geerlingguy.pip](https://github.com/geerlingguy/ansible-role-pip) to install Python reqs


### Remote Node

- [Docker][]
- [docker-py][]

[Docker]: https://www.docker.com
[docker-py]: https://docker-py.readthedocs.io


Role Variables
--------------

### Docker Image and Tag

Always uses the official [splunk][] image.

[splunk]: https://hub.docker.com/r/splunk/splunk/

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
