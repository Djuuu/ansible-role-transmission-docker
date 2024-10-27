Ansible Role: Transmission-docker
=================================

Install Transmission Docker Compose project.

- https://transmissionbt.com/
- https://github.com/transmission/transmission

Based on LinuxServer.io image: https://docs.linuxserver.io/images/docker-transmission/

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common system variables:

```yaml
timezone: UTC
```

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

transmission_project_name: transmission

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overriden)

transmission_traefik_loadbalancer_server_port: 9091
transmission_traefik_middlewares:
  - "{{ docker_project_slug }}-cors@docker"

# Main service additional docker-compose options (ex: cpu_shares, deploy, ...)
transmission_compose_service_additional_options: |
  ports:
    - "9091:9091/tcp"   # rpc-port
    - "51413:51413/tcp" # peer-port
    - "51413:51413/udp" # peer-port
```

```yaml
# Transmission project variables

transmission_user:     "transmission"
transmission_password: "CH4NGE ME!"

transmission_download_dir: "{{ docker_project_path }}/downloads"
transmission_watch_dir:    "{{ docker_project_path }}/watch"

# Transmission custom settings (see `transmission_default_settings` in vars/main.yml)
transmission_custom_settings:
  blocklist-enabled: true
  blocklist-url:     "https://mirror.codebucket.de/transmission/blocklist.p2p.gz"
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.transmission_docker
```

License
-------

Beerware License
