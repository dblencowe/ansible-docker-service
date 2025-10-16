# Copilot Instructions for ansible-docker-service

## Project Overview
This project is an Ansible role for deploying and managing Docker containers as systemd services. It is designed to be included in other Ansible playbooks to automate the setup of containerized services with persistent storage and custom configuration.

## Key Components
- `tasks/main.yml`: Main logic for role execution. Handles Docker container setup and systemd service management.
- `templates/service_template.service.j2`: Jinja2 template for generating the systemd service file for the Docker container.
- `defaults/main.yml`: Default variables for the role. Override these in your playbook as needed.
- `handlers/main.yml`: Handlers for service reload/restart.

## Usage Pattern
- Include this role in your playbook and provide required variables such as `service_name`, `image`, and `volumes`.
- Example usage (see `README.md`):
  ```yaml
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

## Project-Specific Conventions
- All systemd service files are generated from the Jinja2 template in `templates/service_template.service.j2`.
- Variables are passed via `vars` in the playbook or overridden in `defaults/main.yml`.
- The role expects Docker and systemd to be available on the target host.
- Volumes should be specified as a list of Docker volume names.

## Developer Workflows
- No build or test scripts are present; role is tested by including it in a playbook and running against a test host.
- To debug, add `-vvv` to your `ansible-playbook` command for verbose output.
- Handlers are used for service reloads after changes to the systemd unit file or Docker configuration.

## Integration Points
- Integrates with Docker and systemd on the target host.
- Can be used as a dependency in other Ansible roles or playbooks (see `requirements.yml` example in `README.md`).

## Patterns and Examples
- All service logic is driven by variables and templates—no hardcoded service names or images.
- The role is designed for reusability and composability in larger Ansible projects.

## References
- See `README.md` for usage examples and required variables.
- Key files: `tasks/main.yml`, `templates/service_template.service.j2`, `defaults/main.yml`.

---

If you are an AI agent, follow these conventions and reference the example playbook in `README.md` for correct usage patterns. When in doubt, prefer explicit variable overrides and template-driven configuration.
