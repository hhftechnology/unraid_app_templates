# Complete Lemmy Setup Guide for Unraid

## Prerequisites
1. Create the following directory structure on your Unraid server:
```bash
/mnt/user/appdata/lemmy/
├── config/          # Lemmy configuration
├── postgres/        # PostgreSQL data
└── pictrs/          # Image storage
```

## Step 1: PostgreSQL Setup
1. Install the Lemmy-Postgres template
2. Configure these essential variables:
   - POSTGRES_USER: `lemmy`
   - POSTGRES_PASSWORD: `[choose a secure password]`
   - POSTGRES_DB: `lemmy`
   - Data Path: `/mnt/user/appdata/lemmy/postgres`
   - Port: `5433` (or different if in use)

3. Start the container and verify it's running by checking logs

## Step 2: Pict-rs Setup
1. Install the Pict-rs template
2. Configure:
   - API Key: `[generate a secure key]`
   - Media Path: `/mnt/user/appdata/lemmy/pictrs`
3. Note down the API key for later use in Lemmy configuration
4. Start the container

## Step 3: Lemmy Server Setup
1. Install the Lemmy-Server template
2. Create `/mnt/user/appdata/lemmy/config/lemmy.hjson` with this content:
```hjson
{
  hostname: "your_domain.com"
  port: 8536
  tls_enabled: false # Set to true if using HTTPS
  
  # Database settings
  database: {
    host: "lemmy-postgres"
    port: 5432
    user: "lemmy"
    password: "[your postgres password]"
    database: "lemmy"
    pool_size: 5
  }
  
  # Picture service
  pictrs: {
    url: "http://pict-rs:8080/"
    api_key: "[your pictrs api key]"
  }

  # Rate limits
  rate_limit: {
    message: 180
    message_per_second: 60
    post: 50
    post_per_second: 600
    register: 20
    register_per_second: 3600
    image_upload: 6
    image_upload_per_second: 3600
  }
}
```

3. Configure:
   - Config Path: `/mnt/user/appdata/lemmy/config`
   - WebSocket Port: `8536`
4. Start the container

## Step 4: Lemmy UI Setup
1. Install the Lemmy-UI template
2. Configure:
   - WebUI Port: `1236`
   - Environment Variables:
     - LEMMY_UI_LEMMY_INTERNAL_HOST: `lemmy-server:8536`
     - LEMMY_UI_LEMMY_EXTERNAL_HOST: `your_domain.com` (or `localhost:1236` for local testing)
     - LEMMY_UI_HTTPS: `false` (set to true if using HTTPS)
3. Start the container

## Step 5: Reverse Proxy (Optional but Recommended)
If you're using SWAG or another reverse proxy, add this configuration:

```nginx
location / {
    proxy_pass http://lemmy-ui:1236;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
}

location /api {
    proxy_pass http://lemmy-server:8536;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;

    # WebSocket support
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
}

location /pictrs {
    proxy_pass http://pict-rs:8080;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    client_max_body_size 20M;
}
```

## Verification Steps
1. Check all container logs for errors
2. Access the UI at `http://your_server_ip:1236`
3. Create an admin account (first account becomes admin)
4. Test image upload functionality
5. Verify WebSocket connection (realtime updates should work)

## Troubleshooting
1. Connection Issues:
   - Verify all containers are running
   - Check container logs for errors
   - Ensure ports are not conflicting
   - Verify network connectivity between containers

2. Database Issues:
   - Check PostgreSQL logs
   - Verify database credentials in lemmy.hjson
   - Ensure database is properly initialized

3. Image Upload Issues:
   - Verify Pict-rs API key matches in configuration
   - Check Pict-rs logs
   - Verify storage permissions

4. UI Issues:
   - Clear browser cache
   - Verify LEMMY_UI_LEMMY_INTERNAL_HOST setting
   - Check browser console for errors

## Backup Considerations
Regular backup of these directories is recommended:
- `/mnt/user/appdata/lemmy/postgres/` (Database)
- `/mnt/user/appdata/lemmy/config/` (Configuration)
- `/mnt/user/appdata/lemmy/pictrs/` (Uploaded media)

## Security Recommendations
1. Use strong, unique passwords for:
   - PostgreSQL database
   - Pict-rs API key
   - Admin account
2. Enable HTTPS if exposed to the internet
3. Regularly update all containers
4. Monitor logs for suspicious activity