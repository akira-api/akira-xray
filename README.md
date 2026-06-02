# Akira Xray

A lightweight Xray proxy running on Docker with SOCKS5 inbound and VLESS+WebSocket outbound.

## Prerequisites

- Docker
- Docker Compose

## Setup

### 1. Create Docker Network

Before running the container, create an external Docker network:

```bash
docker network create akira-net
```

### 2. Configuration

Edit `config.json` to match your Xray server settings:

```json
{
  "inbounds": [
    {
      "port": 1080,
      "listen": "0.0.0.0",
      "protocol": "socks",
      "settings": {
        "auth": "noauth",
        "udp": true
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "vless",
      "settings": {
        "vnext": [
          {
            "address": "YOUR_SERVER_ADDRESS",
            "port": 80,
            "users": [
              {
                "id": "YOUR_UUID",
                "encryption": "none"
              }
            ]
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "security": "none",
        "wsSettings": {
          "path": "/YOUR_PATH",
          "headers": {
            "Host": "YOUR_HOST"
          }
        }
      }
    }
  ]
}
```

### 3. Run

```bash
docker compose up -d
```

The SOCKS5 proxy will be available on `localhost:1080`.
