# Terminal & SSH (Hardened)

A minimal ARM64 Home Assistant app based on the official **Terminal & SSH** image, with the SSH MAC algorithms restricted to:

- `hmac-sha2-512-etm@openssh.com`
- `hmac-sha2-256-etm@openssh.com`

The Dockerfile changes `/usr/share/tempio/sshd_config`, the template from which the official app generates `/etc/ssh/sshd_config` at startup. This makes the policy survive app restarts and container recreation. If an upstream release adds its own `MACs` directive, the build stops for review rather than silently overriding it.

## Upstream updates

The Dockerfile uses `homeassistant/aarch64-addon-ssh:latest`, tracking the current official ARM64 image **when the app is rebuilt with a fresh base-image pull**. The `latest` tag alone does **not** make Home Assistant rebuild or update an installed app. The `version` in `ssh/config.yaml` must be updated when publishing a new revision so the Supervisor can recognise an update. Review changes to upstream startup scripts and the SSH template before each build.

## Deployment

This repository's `dev` branch is for review and local testing. To test on Home Assistant OS, check out the `dev` branch beneath the device's `/local_apps` directory, reload the app store, and install **Terminal & SSH (Hardened)**. Leave the official SSH app installed as a rollback route; avoid publishing both SSH services on the same host TCP port.

Once approved, merge the tested changes into the repository's default branch to serve them through the normal custom-app repository URL. The default `master` branch is not changed by work on `dev`.

## Attribution

Derived from the official Home Assistant Terminal & SSH app. The upstream repository is [home-assistant/app-ssh](https://github.com/home-assistant/app-ssh). Retained upstream material is covered by the original Apache-2.0 `LICENSE`.
