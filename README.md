# ddrive + Gokapi

A file sharing server using Discord as a storage backend.

## Setup

```bash
git clone https://github.com/Nerahikada/ddrive-gokapi.git ddrive+Gokapi
cd ddrive+Gokapi/

# Clone dependencies
git clone https://github.com/Nerahikada/ddrive.git
git clone https://github.com/Forceu/Gokapi.git

# Apply S3 part size patch to Gokapi
git -C Gokapi fetch https://github.com/Nerahikada/Gokapi.git patch/s3-part-size-10mb
git -C Gokapi -c user.email=patch@local -c user.name=patch cherry-pick FETCH_HEAD

# Edit .env and webhook.txt
cp sample.env .env
touch webhook.txt

docker compose up -d
```

### webhook.txt

One Discord webhook URL per line:

```
https://discord.com/api/webhooks/...
https://discord.com/api/webhooks/...
```

## Cloudflare Tunnel

Add to `.env` and start with `--profile cloudflare`:

```env
CLOUDFLARED_TUNNEL_TOKEN=your-token-here
GOKAPI_USE_CLOUDFLARE=true
```

```bash
docker compose --profile cloudflare up -d
```

## Update

```bash
# Update this repo
git pull

# Update ddrive
git -C ddrive pull

# Update Gokapi and re-apply patch
git -C Gokapi pull
git -C Gokapi fetch https://github.com/Nerahikada/Gokapi.git patch/s3-part-size-10mb
git -C Gokapi -c user.email=patch@local -c user.name=patch cherry-pick FETCH_HEAD

# Rebuild and restart
docker compose up -d --build
```
