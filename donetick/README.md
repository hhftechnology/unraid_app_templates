To set this up:

1. Create the necessary directories:
```bash
mkdir -p /mnt/user/appdata/Donetick/database
mkdir -p /mnt/user/appdata/Donetick/config
```

2. Create the selfhosted.yaml file:
```bash
nano /mnt/user/appdata/Donetick/config/local.yaml
```
Copy the YAML content from above into this file.

3. Set proper permissions:
```bash
chmod -R 755 /mnt/user/appdata/Donetick
chmod 644 /mnt/user/appdata/Donetick/config/local.yaml
chown -R nobody:users /mnt/user/appdata/Donetick
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