# Ansible Docker Service

Include this role to setup a simple Docker container managed by systemd.

```yaml
# requirements.yml
- src: git@github.com:dblencowe/ansible-docker-service.git
  scm: git
  version: main
  name: docker-service
```

```yaml
---
# deploy.yml
- name: Deploy HomeAssistant service
  include_role:
    name: docker-service
  vars:
    service_name: hass
    volumes: ['hass']
    image: "{{ homeassistant_image }}"
    docker_run_args: >-
        --privileged
        --network host
        --restart unless-stopped
        -v /etc/localtime:/etc/localtime:ro
        -v /etc/timezone:/etc/timezone:ro
        -v hass:/config
```