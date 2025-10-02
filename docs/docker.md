# Docker Operations Guide

When deploying Sheikh Proxy with Docker, containers typically run in detached mode so the proxy can serve requests continuously. Use the following patterns to interact with these long-running containers without interrupting service or to launch new utility containers for debugging.

## Start a Container With an Interactive Shell

To launch a fresh container and immediately drop into a Bash session, combine the interactive (`-i`) and TTY (`-t`) flags when running the image. This pattern is useful for ad-hoc diagnostics or image exploration before formalizing the `CMD`/`ENTRYPOINT` values.

```bash
docker run -it --name sheikh_api_debug sheikh-proxy-image /bin/bash
```

Because the container starts with Bash as PID 1, exiting the shell stops the container. Use this flow for short-lived debugging sessions only.

## Run Commands in a Detached Container

Use `docker exec` to launch commands in an already running container.

```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARG...]
```

### Useful Flags

- `-d, --detach` – Run the command in the background without attaching to STDOUT/STDIN.
- `-i` – Keep STDIN open for commands that expect input.
- `-t` – Allocate a TTY (interactive shell).
- `-w, --workdir` – Set the working directory inside the container.
- `-e, --env` – Provide environment variables to the command.

### Examples

Run a maintenance command in the background:

```bash
docker exec -d sheikh_api touch /tmp/marker_file
```

Open an interactive shell:

```bash
docker exec -it sheikh_api /bin/bash
```

Run a single Bash command without launching an interactive session:

```bash
docker exec sheikh_api bash -c "python scripts/seed_demo_data.py"
```

The `-c` flag instructs Bash to execute the supplied string and exit, keeping the container focused on its primary workload while you run targeted maintenance tasks.

Set a working directory while opening a shell:

```bash
docker exec -it -w /usr/src/app sheikh_api /bin/bash
```

### Operational Tips

- Start long-lived services in detached mode with `docker run -d` or Docker Compose.
- Use `docker exec` to inspect, debug, or run maintenance tasks without restarting containers.
- Review background container output with `docker logs <container>`.
- Be mindful of environment-specific paths and variables when running commands inside containers.

These practices keep detached Sheikh Proxy containers manageable while maintaining uptime.
