# DOCKER-ADVANCED

## Networking / Volumes / Debug / Prod Ops

| Command                             | Purpose                   |
| ----------------------------------- | ------------------------- |
| `docker network ls`                 | List networks             |
| `docker network inspect net`        | Inspect network           |
| `docker run --network host`         | Use host network          |
| `docker volume ls`                  | List volumes              |
| `docker volume inspect vol`         | Inspect volume            |
| `docker exec -it c nsenter -t 1 -n` | Enter container netns     |
| `docker stats`                      | Live resource usage       |
| `docker inspect c`                  | Container metadata        |
| `docker events`                     | Docker daemon events      |
| `docker logs --since 1h c`          | Recent logs               |
| `docker update --cpus 2 c`          | Limit CPU                 |
| `docker update --memory 512m c`     | Limit memory              |
| `docker system df`                  | Disk usage                |
| `docker system prune -af`           | Full cleanup              |
| `docker run --rm -it busybox sh`    | Ephemeral debug container |
