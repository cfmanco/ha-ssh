# syntax=docker/dockerfile:1
# Rebuild with a fresh pull to follow the latest official ARM64 SSH app.
FROM homeassistant/aarch64-addon-ssh:latest

# The upstream startup script renders this template on each app start.
# Fail closed if upstream introduces its own MAC policy.
RUN set -eu; \
    template=/usr/share/tempio/sshd_config; \
    test -f "$template"; \
    if grep -Eq '^[[:space:]]*MACs[[:space:]]' "$template"; then \
        echo 'Upstream MAC policy changed; review before rebuilding' >&2; \
        exit 1; \
    fi; \
    printf '\nMACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com\n' >> "$template"
