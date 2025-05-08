# 🗂️ FileBrowser – Portainer Template

A lightweight file manager for your Portainer environment.

This template deploys [FileBrowser](https://github.com/filebrowser/filebrowser) to provide a web-based interface for browsing, uploading, downloading, and managing files inside the `/portainer/Files` directory.

## 🚀 Features

- Simple, intuitive web UI
- Full access to all files under `/portainer/Files/`
- Persistent configuration storage
- Runs as a standalone container in your Portainer stack

## 📦 Deployment

### 🧩 Using Portainer Stacks

1. In Portainer, go to **Stacks** > **Add stack**
2. Select **Repository** as the build method
3. Enter your Git repository URL:
   ```
   https://github.com/your-username/filebrowser-portainer
   ```
4. Deploy the stack
5. Access FileBrowser and start managing your files

## 🧱 Stack Configuration

### Docker Service

```yaml
services:
  filebrowser:
    image: filebrowser/filebrowser:latest
    container_name: filebrowser
    ports:
      - "32771:80"
    volumes:
      - /portainer/Files/AppData/Config/FileBrowser:/config
      - /portainer/Files/:/srv
    environment:
      - FB_BASEURL=/
    restart: unless-stopped
```

### Volumes

* `/config` – Stores FileBrowser settings and user database (persistent)
* `/srv` – The root directory shown in the file browser (mapped to `/portainer/Files` on host)

## 📁 Directory Structure

This stack uses `/portainer/Files` as the default working directory because it is the standard location used by Portainer for file management. This gives FileBrowser access to any configs, templates, or volumes created and managed through Portainer.

### 📂 Recommended Directory Organization

For better organization of your container configurations, we recommend storing them in the `/portainer/Files/AppData/Config/` directory. For example:

```
/portainer/Files/AppData/Config/
├── FileBrowser/     # FileBrowser configuration
├── Plex/           # Plex configuration
├── Sonarr/         # Sonarr configuration
└── Radarr/         # Radarr configuration
```

This structure helps keep your container configurations organized and easily accessible through FileBrowser.

## 🌐 Access

Once deployed, visit:

```
http://<your-host-ip>:32771
```

Default path: `/`

## 🔐 Security

This configuration does **not** include authentication by default. Anyone with access to the port can view and modify files.

### ✅ Recommended Hardening Steps

1. Create admin user:

   ```bash
   docker exec -it filebrowser filebrowser users add admin <password> --perm.admin
   ```

2. Disable anonymous access in the FileBrowser UI under **Settings > Auth**
3. Expose via reverse proxy with basic auth or TLS

## 📝 License

MIT License - See [LICENSE](LICENSE) file for details

