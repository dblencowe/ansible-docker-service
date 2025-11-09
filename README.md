
# Ansible Docker Service

This Ansible role deploys and manages Docker containers as systemd services. It is designed for inclusion in other playbooks to automate containerized service setup with persistent storage and custom configuration.

## Installation

Add to your `requirements.yml`:
```yaml
- src: https://github.com/dblencowe/ansible-docker-service.git
  scm: git
  version: main
  name: dblencowe.docker_systemd_service
```

## Usage Example

Include the role in your playbook and provide required variables:
```yaml
---
# deploy.yml
- name: Deploy example docker service
  include_role:
    name: dblencowe.docker_systemd_service
  vars:
    docker_service_name: example-service
    docker_volumes: ['volume_name']
    docker_image: alpine:3
    docker_run_args: >-
      --restart unless-stopped
      -v /etc/localtime:/etc/localtime:ro
      -v /etc/timezone:/etc/timezone:ro
      -v volume_name:/config
    docker_command: /bin/sh -c "echo 'Hello World'"
```

## Variables

- `docker_service_name`: Name of the systemd service and Docker container
- `docker_image`: Docker image to use
- `docker_volumes`: List of Docker volume names
- `docker_run_args`: Additional arguments for `docker run`
- `docker_command`: Optional command to run in the container (appended after the image name)

## Key Files

- `tasks/main.yml`: Main role logic
- `templates/service_template.service.j2`: Systemd service file template
- `defaults/main.yml`: Default variables
- `handlers/main.yml`: Service reload/restart handlers

## Notes

- All service files are generated from the Jinja2 template
- Role expects Docker and systemd to be present on the target host
- Debug with `ansible-playbook -vvv` for verbose output