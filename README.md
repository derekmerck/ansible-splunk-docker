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

Run with `--skip-tags deps` to skip installing dependency roles.


### Remote Node

- [Docker][]
- [docker-py][]

[Docker]: https://www.docker.com
[docker-py]: https://docker-py.readthedocs.io


Role Variables
--------------

### Global Vars

```yaml
data_dir:         "/data"
config_dir:       "/config"
service_password: "passw0rd!"
common_name:      "example.com"
public_host_name: "splunk"
```

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
splunk_container_name: "splunk"
splunk_use_data_container: True
splunk_http_port:      8000
splunk_admin_port:     8089
splunk_hec_port:       8088
```

### Service Configuration

```yaml
splunk_indices:        []
splunk_hec_enabled:    True
splunk_create_hec_tokens: {}
splunk_hec_tokens:      {}
splunk_secured:         False
splunk_required_disk_size: 0
```

`indices` should be a list like `['index1', 'index2', etc...]`

`create_hec_tokens` should be a dict like `{'tok_name': {'desc': 'My token', 'index': 'index1'} }`

`hec_tokens` adds values directly, should be a dict like `{'tok_name': {'desc': 'My token', 'index': 'index1', 'value': xxxxyyyy-xxx...} }`

For vagrant or other small footprint installs, indicate `required_disk_size` in MB (0=default 5000, but 500 is ok)


### Splunkbase

Any Splunkbase plugins in "{{ role_path }}/splunk-apps/*.tgz" are copied into the container and installed.


Example Playbook
----------------

```yaml
- hosts: indexer
  roles:
     - derekmerck.splunk_docker
```

Extra Tasks
-----------------

Call `docker_logger_play` to setup the Splunk logger for Docker for other `derekmerck` namespace roles.

```yaml
- include_role: derekmerck.splunk_docker
  tasks_from:  docker_logger_play
```

Requires a properly configured host group `indexers'

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
