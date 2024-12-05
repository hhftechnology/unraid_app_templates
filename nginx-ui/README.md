## Guide for setting up the nginx-ui components in unRAID.

1. **Installation Setup**
   - Open your unRAID dashboard
   - Go to the "Apps" tab
   - Click "Add Container" button
   - Copy and paste the template we created into the template field

2. **Path Configuration**:
You'll need to verify/create these directories:
```
/mnt/user/appdata/nginx     # For nginx configurations
/mnt/user/appdata/nginx-ui  # For nginx-ui specific data
/var/www                    # For web files
```

3. **Port Configuration**:
- Port 8080: Web interface access
  - Can be changed if this port conflicts with other services
  - Default mapping: 8080 -> 80 (internal)
- Port 8443: SSL access
  - Can be changed if this port conflicts
  - Default mapping: 8443 -> 443 (internal)

4. **Environment Variables**:
- `TZ=Asia/Singapore` 
  - Change this to your timezone if needed
  - Common values: `America/New_York`, `Europe/London`, etc.

5. **Post-Installation Steps**:
After installing:
1. Access the web interface at: `http://[your-unraid-ip]:8080`
2. Initial setup will guide you through:
   - Creating an admin account
   - Basic nginx configuration
   - SSL setup (if needed)

6. **Directory Structure**:
```
/mnt/user/appdata/nginx/
├── nginx.conf         # Main nginx configuration
├── conf.d/           # Site configurations
└── ssl/              # SSL certificates

/mnt/user/appdata/nginx-ui/
├── config/           # nginx-ui configurations
└── database/         # nginx-ui database

/var/www/            # Web root directory
└── html/            # Default web files
```

7. **Permissions**:
- All directories are set to read/write (rw)
- The container runs unprivileged
- Make sure the directories are owned by the correct user:
  ```bash
  chown -R 99:100 /mnt/user/appdata/nginx
  chown -R 99:100 /mnt/user/appdata/nginx-ui
  ```

8. **Backup Considerations**:
Important directories to backup:
- `/mnt/user/appdata/nginx`
- `/mnt/user/appdata/nginx-ui`

9. **Security Recommendations**:
- Change default ports if exposed to internet
- Use SSL for external access
- Set up strong admin passwords
- Consider using unRAID's built-in SSL reverse proxy

10. **Troubleshooting Tips**:
- Check container logs in unRAID dashboard
- Verify port availability with:
  ```bash
  netstat -tuln | grep '8080\|8443'
  ```
- Ensure all paths exist and have correct permissions
- Check nginx-ui logs at `/mnt/user/appdata/nginx-ui/logs`