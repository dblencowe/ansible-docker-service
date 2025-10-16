# Ansible Docker Service

Include this role to setup a simple Docker container managed by systemd.

```yaml
# requirements.yml
- src: git@github.com:dblencowe/ansible-docker-service.git
  scm: git
  version: main
  name: dblencowe.docker_systemd_service
```

```yaml
---
# deploy.yml
- name: Deploy example docker service
  include_role:
    name: dblencowe.docker_systemd_service
  vars:
    service_name: example-service
    volumes: ['volume_name']
    image: alpine:3
    docker_run_args: >-
        --restart unless-stopped
        -v /etc/localtime:/etc/localtime:ro
        -v /etc/timezone:/etc/timezone:ro
```