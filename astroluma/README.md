### Astroluma Application

The Astroluma application template provides an easy way to deploy the Astroluma service on your Unraid server.

**Template Features:**
- Host network mode for optimal performance
- Configurable environment variables
- Persistent storage for uploads
- Automatic container restart

### Astroluma MongoDB

The MongoDB template provides the database backend required for the Astroluma application.

**Template Features:**
- Exposed port 27017 for database access
- Persistent storage for database files
- Bridge network mode for security
- Compatible with MongoDB 6.0

## Installation

1. Add template repository to Unraid:
   - Open the Unraid GUI

2. Install MongoDB container first:
   - Search for "Astroluma-MongoDB"
   - Configure the container paths:
     - Set host path for `/data/db` to `/mnt/user/appdata/astroluma/mongodb`
   - Click "Apply"

3. Install Astroluma application:
   - Search for "Astroluma"
   - Configure the container paths:
     - Set host path for uploads to `/mnt/user/appdata/astroluma/uploads`
   - Review and adjust environment variables if needed
   - Click "Apply"

## ⚙️ Configuration

### Astroluma Application Template

**Environment Variables:**
- `PORT`: Application port (default: 8000)
- `NODE_ENV`: Environment mode (default: production)
- `SECRET_KEY`: Application secret key
- `MONGODB_URI`: MongoDB connection string

**Volumes:**
- `/app/storage/uploads`: Directory for file uploads
  - Default path: `/mnt/user/appdata/astroluma/uploads`

### MongoDB Template

**Ports:**
- 27017: MongoDB database port

**Volumes:**
- `/data/db`: MongoDB data directory
  - Default path: `/mnt/user/appdata/astroluma/mongodb`

## Security Recommendations

1. Change default environment variables:
   - Generate a new `SECRET_KEY`
   - Modify default MongoDB connection string if needed

2. Use secure passwords:
   - Consider adding MongoDB authentication
   - Store sensitive information using Unraid's secrets management

3. Regular updates:
   - Keep containers updated using Unraid's update manager
   - Monitor for security advisories

## Troubleshooting

### Common Issues

1. Container fails to start:
   - Check Unraid Docker logs
   - Verify all required paths exist
   - Ensure no port conflicts

2. Application can't connect to MongoDB:
   - Confirm MongoDB container is running
   - Check network settings
   - Verify MongoDB connection string

3. Upload issues:
   - Check folder permissions
   - Verify path mappings
   - Ensure sufficient disk space

### Logs

To view container logs in Unraid:
1. Go to the Docker tab
2. Click on the container name
3. Click "Logs" to view the container's output

## Template Maintenance

### Updates

1. Updating templates:
   - Templates are version controlled
   - New versions will appear in Unraid's update manager
   - Always backup data before major updates

2. Backup recommendations:
   - Regular backup of `/mnt/user/appdata/astroluma/`
   - Export container configurations
   - Document any custom settings
