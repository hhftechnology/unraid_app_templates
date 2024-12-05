## A detailed Guide setting up the Donetick components in unRAID.

1. **Installation Setup**
   - Open your unRAID dashboard
   - Go to the "Apps" tab
   - Click "Add Container" button
   - Copy and paste the template we provided into the template field

2. **Path Configuration**
You'll need to verify/create these directories:
```bash
/mnt/user/appdata/Donetick/         # Main application directory
/mnt/user/appdata/Donetick/config   # Configuration files
/mnt/user/appdata/Donetick/database # SQLite database storage
```

3. **Port Configuration**
- Port 2021: Web interface access
  - Can be changed if this port conflicts with other services
  - Default mapping: 2021 -> 2021 (internal)
  - Used for both API and frontend when serve_frontend is true

4. **Environment Variables**
- `DT_ENV=local`
  - Determines which config file to use
  - Options: `local` or `selfhosted`

5. **Configuration Setup**
Place the appropriate YAML file in `/mnt/user/appdata/Donetick/config/`:

```yaml
# local.yaml or selfhosted.yaml structure
name: "local"  # or "selfhosted"
is_done_tick_dot_com: false
telegram:
  token: ""
database:
  type: "sqlite"
  migration: true
jwt:
  secret: "your-secure-secret-here"
  session_time: 168h
  max_refresh: 168h
server:
  port: 2021
  read_timeout: 2s
  write_timeout: 1s
  rate_period: 60s
  rate_limit: 300
  cors_allow_origins:
    - "http://localhost:5173"
    - "http://localhost:7926"
  serve_frontend: true  # false for local.yaml
```

6. **Directory Structure**
```
/mnt/user/appdata/Donetick/
├── config/
│   ├── local.yaml       # Local configuration
│   └── selfhosted.yaml  # Selfhosted configuration
└── database/
    └── donetick.db     # SQLite database file
```

7. **Permissions**
```bash
# Set correct permissions
chmod -R 755 /mnt/user/appdata/Donetick
chmod 644 /mnt/user/appdata/Donetick/config/*.yaml
chown -R nobody:users /mnt/user/appdata/Donetick
```

8. **Backup Considerations**
Important directories to backup:
- `/mnt/user/appdata/Donetick/config`
- `/mnt/user/appdata/Donetick/database`

9. **Security Recommendations**
- Change default JWT secret in config file
- Use a strong, randomly generated string
- Consider configuring email notifications
- Update CORS settings if exposing to internet
- Configure rate limiting appropriately

10. **Troubleshooting Tips**
- Check container logs in unRAID dashboard
- Verify config file exists and is named correctly
- Common issues:
  ```bash
  # Check if config file exists
  ls -l /mnt/user/appdata/Donetick/config/

  # Verify port availability
  netstat -tuln | grep 2021

  # Check config file permissions
  ls -l /mnt/user/appdata/Donetick/config/*.yaml

  # View container logs
  docker logs donetick
  ```

11. **Additional Features Setup**
- Email Notifications:
  ```yaml
  email:
    host: "smtp.example.com"
    port: "587"
    key: "your-smtp-password"
    email: "your@email.com"
    appHost: "your-app-url"
  ```

- Telegram Integration:
  ```yaml
  telegram:
    token: "your-bot-token"
  ```

12. **Post-Installation Steps**
1. Access the web interface at: `http://[your-unraid-ip]:2021`
2. Set up your initial user account
3. Configure any additional notification settings
4. Test task creation and management

This should give you a comprehensive understanding of the Donetick setup process in unRAID.