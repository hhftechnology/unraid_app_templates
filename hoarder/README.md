# Hoarder for Unraid

This collection includes three templates required to run Hoarder on Unraid.

## Templates

1. **Hoarder Main**
   - Primary application interface
   - Port: 3032
   - Volume: `/mnt/user/appdata/hoarder/data`
   - Requirements: Chrome and Meilisearch containers

2. **Hoarder Chrome**
   - Provides web browsing capabilities
   - Port: 9222
   - No persistent storage required
   - Runs with minimal privileges

3. **Hoarder Meilisearch**
   - Search engine component
   - Volume: `/mnt/user/appdata/hoarder/meilisearch`
   - Required for content indexing

## Installation

1. Install all three templates via the Unraid Docker tab
2. Configure OPENAI_API_KEY in Hoarder Main (optional)
3. Start containers in order:
   - Meilisearch
   - Chrome
   - Hoarder Main

## Default Configuration

- Network Mode: bridge
- UI Access: http://[IP]:3032
- Data persistence: All data stored in `/mnt/user/appdata/hoarder/`

## Support

For issues and updates, visit:
[HHF Forums](https://forum.hhf.technology)