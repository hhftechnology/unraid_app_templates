To set this up:

1. Create the necessary directories:
```bash
mkdir -p /mnt/user/appdata/donetick/database
mkdir -p /mnt/user/appdata/donetick/config
```

2. Create the selfhosted.yaml file:
```bash
nano /mnt/user/appdata/donetick/config/local.yaml
```
Copy the YAML content from above into this file.

3. Set proper permissions:
```bash
chmod -R 755 /mnt/user/appdata/donetick
chmod 644 /mnt/user/appdata/donetick/config/local.yaml
chown -R nobody:users /mnt/user/appdata/donetick
```

Key points:
1. Updated all paths to match the container's internal structure
2. Fixed the database path to use `/usr/src/app/data`
3. Corrected the WebUI port to 2021
4. Updated volume mappings to match the application's requirements

Remember to:
- Generate a secure random string for the `auth.jwt.secret` in selfhosted.yaml
- Double-check all paths match your Unraid setup
- Ensure the selfhosted.yaml file is in place before starting the container