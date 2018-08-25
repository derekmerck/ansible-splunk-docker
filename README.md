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

Always uses the official [Splunk][] image.

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
splunk_data_dir:           "/opt/{{ splunk_container_name }}"
splunk_http_port:          8000
splunk_admin_port:         8088
splunk_hec_port:           8089
splunk_container_timezone: "America/New_York"
```

### Service Configuration

```yaml
splunk_password:            "passw0rd!"
splunk_extra_indices:      []
splunk_extra_hecs:         {}
splunk_save_toks:          False
```

`extra_indices` should be a list like `['index1', 'index2', etc...]`

`extra_toks` should be a dict like `{'tok_name': {'desc': 'My token', 'index': 'index1'} }`


### Splunkbase

Any Splunkbase plugins in "{{ role_path }}/splunk-apps*.tgz" are copied into the container and installed.


Example Playbook
----------------

```yaml
- hosts: indexer
  roles:
     - derekmerck.splunk_docker
```


Vagrant
-----------------

You need at _least_ 1GB of RAM available to run Splunk, but Vagrant has only 500MB out of the box.  To fix, add something like this to your Vagrantfile:

```Vagrantfile
  config.vm.provider "virtualbox" do |vb|
     vb.memory = "4096"
  end
```

License
-------

MIT
