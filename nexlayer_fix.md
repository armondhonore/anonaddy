# Nexlayer build/deploy fix guidance (pinned)

## Fixed nexlayer.yaml

```yaml
application:
  name: anonaddy
  pods:
  - name: app
    image: mirror.gcr.io/anonaddy/anonaddy:latest
    path: /
    servicePorts:
    - 8000
    vars:
      APP_NAME: addy.io
      APP_ENV: production
      APP_KEY: "base64:0j3faZvX73wJBchmAN/ARSU/aoZEb7pWJqw75KqGvSk="
      ANONADDY_SECRET: "227bd92fc8364e831b05182fb8ed7d8745582d611a634dfc87fafa553ca1b47f"
      APP_URL: https://relaxed-weasel-anonaddy.cloud.nexlayer.ai
      DB_CONNECTION: mysql
      DB_HOST: anonaddy-mysql.pod
      DB_PORT: "3306"
      DB_DATABASE: anonaddy
      DB_USERNAME: anonaddy
      DB_PASSWORD: "anonadypass"
      DB_TIMEOUT: "300"
      CACHE_DRIVER: redis
      QUEUE_CONNECTION: redis
      SESSION_DRIVER: redis
      SESSION_SECURE_COOKIE: "false"
      REDIS_CLIENT: phpredis
      REDIS_HOST: anonaddy-redis.pod
      REDIS_PORT: "6379"
      MAIL_DRIVER: log
      MAIL_FROM_NAME: addy.io
      MAIL_FROM_ADDRESS: mailer@relaxed-weasel-anonaddy.cloud.nexlayer.ai
      ANONADDY_DOMAIN: relaxed-weasel-anonaddy.cloud.nexlayer.ai
      ANONADDY_HOSTNAME: relaxed-weasel-anonaddy.cloud.nexlayer.ai
      ANONADDY_ENABLE_REGISTRATION: "true"
  - name: anonaddy-mysql
    image: mirror.gcr.io/library/mariadb:10.11
    servicePorts:
    - 3306
    vars:
      MARIADB_DATABASE: anonaddy
      MARIADB_USER: anonaddy
      MARIADB_PASSWORD: "anonadypass"
      MARIADB_ROOT_PASSWORD: "anonadyrootpass"
    volumes:
    - name: anonaddy-db-v3
      mountPath: /var/lib/mysql
      size: 10Gi
  - name: anonaddy-redis
    image: mirror.gcr.io/library/redis:7-alpine
    servicePorts:
    - 6379
```

Pinned facts:
- Pre-built image mirror.gcr.io/anonaddy/anonaddy:latest — do NOT rebuild from source
- MariaDB 10.11 with hardcoded passwords matching app vars (no provisioned secrets)
- Redis 7-alpine for caching and queues
- Volume anonaddy-db-v3 for fresh MariaDB data with correct passwords
- Do not regenerate Dockerfile or nexlayer.yaml
