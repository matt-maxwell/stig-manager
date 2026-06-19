# stig-manager

A workspace for deploying and operating [STIG Manager](https://github.com/NUWCDIVNPT/stig-manager) - an open-source API and web client for managing the assessment of information systems against DISA Security Technical Implementation Guides (STIGs) and Security Requirements Guides (SRGs).

This repository bundles the STIG Manager application together with supporting services for ingestion and monitoring.

## Repository structure

```
.
├── stig-manager/      # STIG Manager API + web client deployment
├── stig-monitoring/   # Monitoring / observability stack for the deployment
├── stig-watcher/      # Watcher service that auto-imports .ckl/.cklb/XCCDF results
├── LICENSE
└── README.md
```

### `stig-manager/`
Deployment configuration for the STIG Manager API and UI. Typically run via Docker Compose alongside a database (e.g. MySQL/MariaDB) and an OIDC provider such as Keycloak.

### `stig-monitoring/`
Monitoring and observability components running the STIG Manager stack (logs, metrics, health, etc.).

### `stig-watcher/`
A STIGMan Watcher instance that watches a filesystem path for checklist (`.ckl`, `.cklb`) and XCCDF result files and pushes parsed reviews into a STIG Manager Collection via the API. Useful for CI pipelines, scheduled scans, or any automated workflow that drops result files on disk.

## Prerequisites

- Docker and Docker Compose
- An OIDC provider supporting the Client Credentials flow (e.g. Keycloak) for the Watcher
- A STIG Manager API Collection grant of **Manage** for the Watcher's client

## Quick start

> ⚠️ The provided compose files are reference deployments intended for evaluation and development. They are **not** production-ready - they do not include CAC authentication, persistent storage hardening, MTLS database connections, log archiving, or other production controls.

1. Clone the repository:
   ```bash
   git clone https://github.com/matt-maxwell/stig-manager.git
   cd stig-manager
   ```

2. Start the STIG Manager stack:
   ```bash
   cd stig-manager
   docker compose up -d
   ```

3. (Optional) Start the monitoring stack:
   ```bash
   cd ../stig-monitoring
   docker compose up -d
   ```

4. (Optional) Configure and start the Watcher to auto-import scan results:
   ```bash
   cd ../stig-watcher
   # edit configuration (API URL, OIDC client, watch path, collection ID, etc.)
   docker compose up -d
   ```

## Configuration

Each subdirectory contains its own configuration files (`.env`, compose files, etc.). Review and edit those before bringing the stack up - at minimum you will need to set:

- STIG Manager API URL and database credentials
- OIDC issuer URL, client ID, and client secret
- Watcher target collection ID and watch directory

## Documentation

- STIG Manager: <https://stig-manager.readthedocs.io>
- STIG Manager source: <https://github.com/NUWCDIVNPT/stig-manager>
- STIGMan Watcher: <https://github.com/NUWCDIVNPT/stigman-watcher>

## License

This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.