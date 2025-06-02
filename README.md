# cAdvisor

cAdvisor (Container Advisor) provides container users an understanding of the resource usage and performance characteristics of their running containers made by google. And this is forked version v0.50.0 of [google's cAdvisor](https://github.com/google/cadvisor) for add `container_restart_count` metric.

## Quick Guide

1. **Make docker image**

```bash
make docker-%
```

2. **Make `docker-compose.yml` file**

```bash
IMAGE_TAG=<your_image_tag>
cadvisor:
    container_name: cadvisor
    image: cadvisor:${IMAGE_TAG}
    restart: unless-stopped
    privileged: true
    command:
      - '-housekeeping_interval=10s'
      - '-docker_only=true'
    volumes:
      - /:/rootfs:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - /var/run:/var/run:rw
      - /sys:/sys:ro
      - /var/lib/docker:/var/lib/docker:ro
      - /sys/fs/cgroup:/cgroup:ro
      - /dev/disk/:/dev/disk:ro
    ports:
      - 9180:8080
```

*If you want to run on macOS, you might need to add below lines to your `docker-compose.yml`*
```bash
devices:
    - /dev/kmsg:/dev/kmsg
```

3. **Start**
```bash
docker-compose up -d
```
