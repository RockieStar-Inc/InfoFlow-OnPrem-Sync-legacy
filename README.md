# InfoFlow: Sync Server


## Get Started

### Requirements

- A linux based server, running on x86_64 (AMD64) or ARM64.
- Docker installed
- (Optional) Docker Compose installed

### Using with Docker Compose

```yaml
services:
  infoflow-sync-onprem-server:
    image: rockiestar/infoflow-onprem-sync:latest
    volumes:
      - ./data/ent.sqlite3:/app/ent.sqlite3
    ports:
      - 3009:3009
    environment:
      - LISTEN_ADDRESS=0.0.0.0:3009
      - DATABASE_URL=file:/app/ent.sqlite3
      # Uncomment to use your own S3 bucket (not recommended)
      # - S3_ENDPOINT=http://localhost:9000
      # - S3_ACCESS_KEY_ID=minioadmin
      # - S3_SECRET_ACCESS_KEY=minioadmin
      # - S3_BUCKET_NAME=infoflow
      - ADMIN_USERNAME=admin # Note: To protect your data, please change this username and password
      - ADMIN_PASSWORD=admin
      - ADMIN_EMAIL=admin@infoflow.app
```

```shell
docker-compose up -d
```
