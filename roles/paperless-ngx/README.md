# Ansible Role: Paperless-ngx

Installs and configures [Paperless-ngx](https://github.com/paperless-ngx/paperless-ngx) - a document management system with OCR support.

## Requirements

- Debian/Ubuntu based system
- Ansible >= 2.9
- Python 3.8+
- PostgreSQL 12+ (automatically installed)
- Redis (automatically installed)

## Role Variables

### Version and Paths

```yaml
paperless_version: "latest"              # Paperless-ngx version (or e.g. "v2.19.5")
paperless_home: "/opt/paperless"         # Installation directory
paperless_user: "paperless"              # System user
paperless_group: "paperless"             # System group
paperless_tmp_dir: "/tmp"                # Temporary directory for downloads
```

### Directories

```yaml
paperless_media_dir: "{{ paperless_home }}/media"      # Documents
paperless_data_dir: "{{ paperless_home }}/data"        # Database data (for SQLite)
paperless_consume_dir: "{{ paperless_home }}/consume"  # Inbox folder
```

### Redis

```yaml
paperless_redis: "redis://localhost:6379"  # Redis connection string
```

### Database

#### Option 1: Local PostgreSQL (recommended)
```yaml
paperless_db_url: ""                     # Leave empty for local DB
paperless_dbengine: "postgres"           # postgres, mariadb or sqlite
paperless_dbhost: "localhost"
paperless_dbport: "5432"
paperless_dbname: "paperless"
paperless_dbuser: "paperless"
paperless_db_password: "change_me"       # ⚠️ Encrypt in vault!
```

#### Option 2: External Database
```yaml
paperless_db_url: "postgresql://user:pass@host:5432/dbname"  # Overrides other DB settings
```

### Application Configuration

```yaml
paperless_secret_key: "change_me_secret_key"  # ⚠️ Encrypt in vault!
paperless_url: ""                             # Base URL if behind reverse proxy
paperless_time_zone: "Europe/Berlin"
paperless_ocr_language: "eng"                 # OCR language (eng, deu, etc.)
```

### Superuser

```yaml
paperless_create_superuser: true              # Create superuser during installation?
paperless_superuser_name: "admin"
paperless_superuser_email: "admin@example.com"
paperless_superuser_password: "change_me"     # ⚠️ Encrypt in vault!
```

### Additional Options

```yaml
paperless_imagemagick_allow_pdf: true         # Allow PDF processing in ImageMagick
paperless_webserver_port: "8000"              # Webserver port
```

## Dependencies

This role has no external Ansible dependencies. All required packages are installed automatically.

## Example Playbook

### Minimal Configuration

```yaml
---
- hosts: paperless_servers
  become: true
  
  roles:
    - role: paperless-ngx
```

### Advanced Configuration

```yaml
---
- hosts: paperless_servers
  become: true
  
  vars:
    paperless_version: "v2.19.5"
    paperless_time_zone: "Europe/Berlin"
    paperless_ocr_language: "deu"
    paperless_url: "https://paperless.example.com"
    
    # Superuser configuration
    paperless_superuser_name: "admin"
    paperless_superuser_email: "admin@example.com"
    
  vars_files:
    - vault.yml  # Contains: paperless_db_password, paperless_secret_key, paperless_superuser_password
  
  roles:
    - role: paperless-ngx
```

### With External Database

```yaml
---
- hosts: paperless_servers
  become: true
  
  vars:
    paperless_db_url: "postgresql://paperless:secure_password@db.example.com:5432/paperless"
  
  roles:
    - role: paperless-ngx
```

## Vault Example

Create `vault.yml` with sensitive data:

```yaml
# Encrypt with: ansible-vault encrypt vault.yml
paperless_db_password: "supersecret123"
paperless_secret_key: "verylongrandomstring"
paperless_superuser_password: "admin_password"
```

## Systemd Services

The role automatically creates and enables the following services:

- `paperless-webserver.service` - Web interface (Port 8000)
- `paperless-consumer.service` - Document import
- `paperless-task-queue.service` - Async tasks (Celery)
- `paperless-scheduler.service` - Scheduled tasks (Celery Beat)

Check status:
```bash
systemctl status paperless-webserver
systemctl status paperless-consumer
systemctl status paperless-task-queue
systemctl status paperless-scheduler
```

## Firewall

The role automatically configures UFW (if installed) to allow traffic on `paperless_webserver_port`.

## Security

⚠️ **Important:**
- Encrypt all passwords using Ansible Vault
- Generate `paperless_secret_key` with at least 50 random characters
- Run Paperless behind a reverse proxy (nginx/traefik) with TLS
- Regular backups of `/opt/paperless/media` and the PostgreSQL database

## License

MIT

## Author

JanLeshy
