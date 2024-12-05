# Complete Setup Guide for Gathio on Unraid

## Prerequisites
- Unraid server with Docker enabled
- Access to the Unraid web interface
- Basic understanding of Docker networking
- At least 1GB of free space for initial setup

## Step 1: Prepare Directory Structure

First, create the necessary directories on your Unraid server. You can do this via SSH or the terminal:

```bash
mkdir -p /mnt/user/appdata/gathio/config
mkdir -p /mnt/user/appdata/gathio/images
mkdir -p /mnt/user/appdata/gathio/mongodb
```

## Step 2: Set Up MongoDB Container

1. Open the Unraid Docker interface
2. Click "Add Container"
3. Paste the MongoDB template XML content
4. Important settings to modify:
   - Change `MONGO_INITDB_ROOT_PASSWORD` to a secure password
   - Note down the username (`root`) and password for later
   - Verify the path `/mnt/user/appdata/gathio/mongodb` exists
5. Click "Apply" to create the container
6. Wait for the container to download and start

## Step 3: Configure MongoDB Security

1. After MongoDB starts, create a specific database and user for Gathio:
   ```bash
   docker exec -it Gathio-MongoDB mongosh -u root -p your_password
   ```
2. In the MongoDB shell, run:
   ```javascript
   use gathio
   db.createUser({
     user: "gathio_user",
     pwd: "choose_secure_password",
     roles: [{ role: "readWrite", db: "gathio" }]
   })
   exit
   ```

## Step 4: Set Up Gathio Configuration

1. Create a configuration file in `/mnt/user/appdata/gathio/config/config.json`:
   ```json
   {
     "port": 3032,
     "baseUrl": "http://your-server-ip:3032",
     "mongodb": {
       "host": "mongo",
       "port": 27017,
       "database": "gathio",
       "username": "gathio_user",
       "password": "your_gathio_user_password"
     },
     "title": "My Gathio Instance",
     "timezone": "UTC"
   }
   ```

## Step 5: Deploy Gathio Container

1. Open the Unraid Docker interface
2. Click "Add Container"
3. Paste the Gathio template XML content
4. Verify these settings:
   - Port mapping: 3032:3000
   - Config directory: `/mnt/user/appdata/gathio/config`
   - Images directory: `/mnt/user/appdata/gathio/images`
5. Click "Apply" to create the container

## Step 6: Verify Installation

1. Wait for both containers to show as running (green status)
2. Access Gathio at `http://your-unraid-ip:3032`
3. Create a test event to verify functionality

## Maintenance and Backup

### Regular Backups
Include these directories in your Unraid backup strategy:
- `/mnt/user/appdata/gathio/config`
- `/mnt/user/appdata/gathio/images`
- `/mnt/user/appdata/gathio/mongodb`

### Updating
1. MongoDB updates:
   - Stop the MongoDB container
   - Change the tag in template if desired
   - Start the container

2. Gathio updates:
   - Stop the Gathio container
   - Change the tag in template if desired
   - Start the container

## Troubleshooting

### Common Issues and Solutions

1. MongoDB Connection Issues:
   - Verify MongoDB container is running
   - Check MongoDB logs: `docker logs Gathio-MongoDB`
   - Verify credentials in config.json match MongoDB user

2. Gathio Won't Start:
   - Check Gathio logs: `docker logs gathio-app`
   - Verify config.json format and permissions
   - Ensure MongoDB is running and accessible

3. Image Upload Issues:
   - Check permissions on `/mnt/user/appdata/gathio/images`
   - Verify directory ownership matches container user

### Getting Help

If you encounter issues:
1. Check container logs first
2. Verify all paths and permissions
3. Ensure network connectivity between containers
4. Check Gathio's GitHub issues page for similar problems

## Security Recommendations

1. Network Security:
   - Consider using a reverse proxy (like SWAG)
   - Enable HTTPS if exposed to internet
   - Restrict MongoDB port access

2. Authentication:
   - Use strong passwords for MongoDB
   - Regularly rotate credentials
   - Monitor access logs

3. Filesystem:
   - Regular permission checks
   - Periodic security audits
   - Keep backups encrypted