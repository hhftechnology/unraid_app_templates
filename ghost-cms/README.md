# Installation Guide for Ghost Blog UnRaidTemplates

## Template Files Setup
1. Create two files on your local computer:
   - `ghost-db.xml` - Copy the exact content from the first template
   - `ghost.xml` - Copy the exact content from the second template

## Step 1: Installing the Database Container

1. Open unRAID web interface
2. Navigate to the "Docker" tab
3. Click "Add Container"
4. Click "Upload" and select your `ghost-db.xml` file
5. The form will populate automatically. Verify these exact settings:

### Container Settings for GhostDB:
```
Name: GhostDB
Repository: maurosoft1973/alpine-mariadb
Network Type: bridge
```

### Environment Variables (EXACTLY as shown):
```
MYSQL_ROOT_PASSWORD=GObrpLskjguwiDoR4lyJzGWUXbl0cY6IOzSMZI1D
MYSQL_DATABASE=ghost
MYSQL_USER=ghost
MYSQL_PASSWORD=91XHfXc0zpJZ8SCTsTheaDW3rm15w17yPGjxHzcL
```

### Path Configuration:
```
Host Path: /mnt/user/appdata/ghost/mysql
Container Path: /var/lib/mysql
Access Mode: Read/Write
```

### Port Configuration:
```
Host Port: 3306
Container Port: 3306
```

6. Click "Apply"
7. Wait until container shows as "Running" (approximately 1-2 minutes)
8. Check logs to confirm successful initialization

## Step 2: Installing the Ghost Container

1. Stay in the Docker tab
2. Click "Add Container" again
3. Click "Upload" and select your `ghost.xml` file
4. Verify these exact settings:

### Container Settings for Ghost:
```
Name: Ghost
Repository: ghost:5-alpine
Network Type: bridge
```

### Environment Variables (EXACTLY as shown):
```
database__client=mysql
database__connection__host=ghost_db
database__connection__user=ghost
database__connection__password=91XHfXc0zpJZ8SCTsTheaDW3rm15w17yPGjxHzcL
database__connection__database=ghost
url=http://[IP]:2368
```

Note: Replace [IP] with your unRAID server's IP address

### Path Configuration:
```
Host Path: /mnt/user/appdata/ghost/content
Container Path: /var/lib/ghost/content
Access Mode: Read/Write
```

### Port Configuration:
```
Host Port: 2368
Container Port: 2368
```

## Step 3: Verification Steps

1. Check Database Container:
   - Status should show "Running"
   - Logs should show "MariaDB init process done. Ready for start up."
   
2. Check Ghost Container:
   - Status should show "Running"
   - Logs should show "Ghost boot process ended"
   
3. Access Ghost:
   - Open web browser
   - Navigate to: `http://YOUR_UNRAID_IP:2368`
   - You should see the default Ghost welcome page

4. Access Admin Panel:
   - Navigate to: `http://YOUR_UNRAID_IP:2368/ghost`
   - Create your admin account on first visit

## Important Notes About Templates

1. Template Variables:
   - All environment variables are preset in the templates
   - Database passwords are preset but should be changed
   - URL variable automatically uses your server's IP

2. Volume Mappings:
   - Templates automatically create necessary directories
   - Proper permissions are set automatically
   - All data persists in `/mnt/user/appdata/ghost/`

3. Network Configuration:
   - Both containers use bridge networking
   - Internal communication is handled automatically
   - Ports 2368 and 3306 must be free on your host

4. Container Dependencies:
   - Always start GhostDB container first
   - Wait for database initialization before starting Ghost
   - Both containers set to auto-start after reboot

## Template Modifications (if needed)

If you need to change any settings, modify these specific sections:

### In ghost-db.xml:
```xml
<Environment>
    <Variable>
        <Name>MYSQL_ROOT_PASSWORD</Name>
        <Value>YOUR_NEW_ROOT_PASSWORD</Value>
    </Variable>
    <!-- Other variables follow same pattern -->
</Environment>
```

### In ghost.xml:
```xml
<Environment>
    <Variable>
        <Name>database__connection__password</Name>
        <Value>YOUR_NEW_PASSWORD</Value>
    </Variable>
    <!-- Other variables follow same pattern -->
</Environment>
```