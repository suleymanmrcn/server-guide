# Docker Commands

Essential Docker commands for daily operations.

## Container Management

| Action           | Command                          |
| :--------------- | :------------------------------- |
| **List Running** | `docker ps`                      |
| **List All**     | `docker ps -a`                   |
| **Logs**         | `docker logs -f <container>`     |
| **Shell Access** | `docker exec -it <container> sh` |
| **Stats**        | `docker stats`                   |

## Image Management

| Action           | Command                  |
| :--------------- | :----------------------- |
| **List Images**  | `docker images`          |
| **Pull Image**   | `docker pull <image>`    |
| **Remove Image** | `docker rmi <image_id>`  |
| **Prune Unused** | `docker system prune -a` |

## Wrapper Functions (Optional)

Using `~/.zshrc` aliases:

```bash
alias dps='docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}\t{{.Ports}}"'
alias dlogs='docker logs -f'
```
