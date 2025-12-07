# Frontend Proxy Service

Overview

- Base Image: envoyproxy/envoy:v1.32.2

- Template Engine: envsubst (from gettext-base) is used to substitute environment variables in the Envoy configuration.

- Configuration File: env_vars.yaml (template) → envoy.yaml (generated at runtime)

- User: Runs as envoy user for security

- At container startup, the Docker entrypoint substitutes environment variables into the template and launches Envoy with the resulting configuration.

Files

* Dockerfile – Builds the Envoy proxy image with envsubst installed.

* env_vars.yaml – Envoy configuration template containing environment variable placeholders.

* envoy.yaml – Generated configuration (not checked into source control, created at runtime).
