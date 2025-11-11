# Ansible Collection: janleshy.paperless

![Ansible](https://img.shields.io/badge/ansible-%231A1918.svg?style=for-the-badge&logo=ansible&logoColor=white)
![Paperless-ngx](https://img.shields.io/badge/Paperless--ngx-%2338B6FF?style=for-the-badge&logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAAA4AAAAOCAYAAAAfSC3RAAAACXBIWXMAAAsTAAALEwEAmpwYAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAACOSURBVHgBrZLBDYAwDAMvYgRG6Qh0lH+PwAiMwgjdoJUqUYkQ+YAl+xLHTmxjAJRSSimllFLqf6C1NsYY4xxjzBhjjDHGGGOMMcYYY4wxxhhjjDHGGGOMMcYYY4wxxhhjjDHGGGOMMcYYY4wxxhhjjDHGGGOMMcYYY4wxxhhjjDHGGGOMMcYYY4wx/gBqrfVXAQ4AAAAASUVORK5CYII=)

Ansible Collection for deploying and managing **Paperless-ngx** - a powerful document management system with OCR support.

## 📦 What's Included

### Roles

- **`paperless_ngx`** - Complete Paperless-ngx deployment with PostgreSQL and Redis

## 🚀 Installation

### From Ansible Galaxy

```bash
ansible-galaxy collection install janleshy.paperless
```

### From GitHub

```bash
ansible-galaxy collection install git+https://github.com/JanLeshy/ansible-collection-paperless.git
```

### Requirements File

Create `requirements.yml`:

```yaml
---
collections:
  - name: janleshy.paperless
    version: ">=1.0.0"
```

Install:

```bash
ansible-galaxy collection install -r requirements.yml
```

## 📖 Usage

### Quick Start

```yaml
---
- hosts: paperless_servers
  become: true
  
  collections:
    - janleshy.paperless
  
  roles:
    - paperless_ngx
```

### With Custom Variables

```yaml
---
- hosts: paperless_servers
  become: true
  
  collections:
    - janleshy.paperless
  
  vars:
    paperless_version: "v2.19.5"
    paperless_time_zone: "Europe/Berlin"
    paperless_ocr_language: "deu"
    paperless_webserver_port: "8000"
  
  vars_files:
    - vault.yml  # Contains passwords and secrets
  
  roles:
    - paperless_ngx
```

### Vault Configuration

Create `vault.yml` with sensitive data:

```yaml
# Encrypt with: ansible-vault encrypt vault.yml
paperless_db_password: "supersecret123"
paperless_secret_key: "verylongrandomstring"
paperless_superuser_password: "admin_password"
```

Run playbook:

```bash
ansible-playbook -i inventory playbook.yml --ask-vault-pass
```

## 📚 Documentation

For detailed documentation, see [roles/paperless_ngx/README.md](roles/paperless_ngx/README.md)

### Key Features

✅ **Automated Installation**
- PostgreSQL database setup
- Redis configuration
- Python virtual environment
- System dependencies

✅ **Production Ready**
- Systemd service management
- Automatic migrations
- UFW firewall configuration
- Proper file permissions

✅ **Flexible Configuration**
- Local or external database
- Custom installation paths
- OCR language support
- Timezone configuration

✅ **Security**
- Ansible Vault support
- Secure password handling
- Non-root execution

## 🛠️ Requirements

- **OS**: Debian/Ubuntu based systems
- **Ansible**: >= 2.10
- **Python**: 3.8+
- **PostgreSQL**: 12+ (auto-installed)
- **Redis**: (auto-installed)

## 📋 Role Variables

### Essential Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `paperless_version` | `"latest"` | Paperless-ngx version |
| `paperless_home` | `"/opt/paperless"` | Installation directory |
| `paperless_db_password` | `""` | PostgreSQL password (⚠️ use vault!) |
| `paperless_secret_key` | `""` | Django secret key (⚠️ use vault!) |
| `paperless_time_zone` | `"UTC"` | Application timezone |
| `paperless_ocr_language` | `"eng"` | OCR language |

For complete list, see [roles/paperless_ngx/defaults/main.yml](roles/paperless_ngx/defaults/main.yml)

## 🔧 Services

The collection creates and manages these systemd services:

- `paperless-webserver` - Web interface (default port: 8000)
- `paperless-consumer` - Document import worker
- `paperless-task-queue` - Celery task queue
- `paperless-scheduler` - Celery beat scheduler

Check status:

```bash
sudo systemctl status paperless-webserver
sudo systemctl status paperless-consumer
sudo systemctl status paperless-task-queue
sudo systemctl status paperless-scheduler
```

## 🐛 Troubleshooting

### Check logs

```bash
journalctl -u paperless-webserver -f
journalctl -u paperless-consumer -f
journalctl -u paperless-task-queue -f
```

### Verify installation

```bash
sudo -u paperless /opt/paperless/venv/bin/python /opt/paperless/src/manage.py check
```

### Database migrations

```bash
sudo -u paperless bash
cd /opt/paperless/src
source ../venv/bin/activate
source ../paperless.conf
python manage.py migrate
```

## 🤝 Contributing

Contributions are welcome! Please:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Test thoroughly
5. Submit a pull request

## 📄 License

MIT

## 👤 Author

**Jan Leshy**
- GitHub: [@JanLeshy](https://github.com/JanLeshy)

## 🙏 Acknowledgments

- [Paperless-ngx Team](https://github.com/paperless-ngx/paperless-ngx) for the amazing document management system
- Ansible Community

## 📞 Support

- **Issues**: [GitHub Issues](https://github.com/JanLeshy/ansible-collection-paperless/issues)
- **Paperless-ngx Docs**: https://docs.paperless-ngx.com/

---

**⭐ If you find this collection useful, please star the repository!**
