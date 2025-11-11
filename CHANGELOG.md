# Changelog

All notable changes to this collection will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-11-11

### Added
- Initial release of `janleshy.paperless` collection
- Role `paperless_ngx` for complete Paperless-ngx deployment
- Automated PostgreSQL database setup with UTF8 encoding
- Redis installation and configuration
- Python virtual environment management
- Systemd service management (webserver, consumer, task-queue, scheduler)
- UFW firewall configuration
- Ansible Vault support for sensitive data
- Comprehensive documentation and examples
- GitHub Actions workflows for linting and Galaxy deployment

### Features
- Local or external database support
- Automatic database migrations
- Superuser creation
- OCR language configuration
- Timezone support
- ImageMagick PDF policy configuration
- Idempotent playbook execution
- Proper file ownership and permissions


[1.0.0]: https://github.com/JanLeshy/ansible-collection-paperless/releases/tag/v1.0.0

