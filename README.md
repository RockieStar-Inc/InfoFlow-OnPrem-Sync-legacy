# InfoFlow: Sync Server

![docker_badge](https://img.shields.io/docker/pulls/rockiestar/infoflow-onprem-sync.svg)

## Get Started

### Requirements

- A linux based server, running on x86_64 (AMD64) or ARM64.
- Docker installed
- (Optional) Docker Compose installed

### Using with Docker Compose

Note: Make sure you change the `TOKEN` environment variable.

```yaml
services:
  infoflow-sync-onprem-server:
    image: rockiestar/infoflow-onprem-sync:latest # Note: If you're running it on arm64, the tag should be `arm64-latest`
    volumes:
      - ./data/:/app/data
    ports:
      - 3009:3009
    logging: &default_logging
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
    environment:
      - LISTEN_ADDRESS=0.0.0.0:3009
      - DATABASE_URL=file:/app/data/infoflow_onprem_sync.sqlite3
      # Uncomment to use your own S3 bucket (not recommended)
      # If not provided, the service will use default S3 service via Minio
      # - S3_ENDPOINT=http://localhost:9000
      # - S3_ACCESS_KEY_ID=minioadmin
      # - S3_SECRET_ACCESS_KEY=minioadmin
      # - S3_BUCKET_NAME=infoflow
      - ADMIN_USERNAME=admin # Note: To protect your data, please change this username and password
      - ADMIN_PASSWORD=admin # these credentials are used to manage the infoflow-onprem-sync service in the future
      - ADMIN_EMAIL=admin@infoflow.app
      - TOKEN=your_secure_token_here # Add this line to set a Bearer token for request authentication. PLEASE CHANGE IT!
  # Uncomment to use cloudflare tunnel to connect to the infoflow-onprem-sync service via public internet
  # cloudflared:
  #   image: cloudflare/cloudflared:latest
  #   command: tunnel --no-autoupdate run # you still have to configure the tunnel in the cloudflare dashboard
  #   logging: *default_logging
  #   environment:
  #     - TUNNEL_TOKEN= # add your cloudflare tunnel token here so that the service can connect to the infoflow-onprem-sync service via public internet

```

```shell
docker-compose up -d
```

### Upgrade

Please make sure that you've backed up the database before upgrading the app version.


#### Docker Compose

```shell
docker-compose pull && docker-compose up -d
```


#### Configuration in InfoFlow app

- Endpoint: `http://[your_ip_address]:3009` (example)
- Token: `(see token above)`
