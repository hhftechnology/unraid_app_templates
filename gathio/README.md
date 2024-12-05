I've created an Unraid template for Gathio application. Here are some key points about the conversion:

1. Base Structure: I followed the standard Unraid template format, similar to the example template provided in the Unraid forums.

2. Volume Mappings:
   - Converted the config path to `/mnt/user/appdata/gathio/config`
   - Converted the images path to `/mnt/user/appdata/gathio/images`

3. Network:
   - Maintained the port mapping of 3032:3000
   - Set to bridge networking mode

4. Dependencies:
   - Added a note about MongoDB dependency in the description
   - Included MongoDB host environment variable

Important notes:
1. You'll need to set up the MongoDB container separately in Unraid
2. The paths in the Data section use the standard Unraid `/mnt/user/appdata` location
3. You may want to adjust the Category based on your preferences

Now you have both templates needed for running Gathio on Unraid. Here's how to use them:

1. Install Order:
   - Install the MongoDB container (Gathio-MongoDB) first
   - Then install the Gathio container

2. Important Configuration Notes:
   - MongoDB template includes basic security with root username/password
   - Make sure to change the default password in the MongoDB template
   - The containers are configured to use the same network mode (bridge)
   - Data is persistently stored in `/mnt/user/appdata/gathio/`

3. Volume Structure:
   ```
   /mnt/user/appdata/gathio/
   ├── config/       # Gathio configuration
   ├── images/       # Gathio event images
   └── mongodb/      # MongoDB data
   ```

4. Security Considerations:
   - The MongoDB port (27017) is exposed but you can remove this if you don't need external access
   - Set strong passwords for the MongoDB root user
   - Make sure your Gathio configuration matches the MongoDB credentials
