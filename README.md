# gokapi-backend-setups

[Gokapi](https://github.com/Forceu/Gokapi) file sharing server with pluggable storage backends.

| Backend | Storage |
|---------|---------|
| [ddrive](https://github.com/Nerahikada/ddrive) | Discord |
| [gigafile-fs](https://github.com/Nerahikada/gigafile-fs) | gigafile.nu |

## Setup

```bash
git clone https://github.com/Nerahikada/gokapi-backend-setups.git
cd gokapi-backend-setups/

git clone https://github.com/Forceu/Gokapi.git

# Copy and edit .env
cp sample.env .env
```

Then follow the setup for your chosen backend below.

## Backends

### ddrive (Discord)

```bash
git clone https://github.com/Nerahikada/ddrive.git

# Apply S3 part size patch to Gokapi
git -C Gokapi fetch https://github.com/Nerahikada/Gokapi.git patch/s3-part-size-10mb
git -C Gokapi -c user.email=patch@local -c user.name=patch cherry-pick FETCH_HEAD
```

Add Discord webhook URLs to `webhook.txt` (one per line):

```
https://discord.com/api/webhooks/...
https://discord.com/api/webhooks/...
```

```bash
docker compose -f compose.gokapi.yaml -f compose.ddrive.yaml up -d
```

### gigafile-fs (gigafile.nu)

```bash
git clone https://github.com/Nerahikada/gigafile-fs.git
```

```bash
docker compose -f compose.gokapi.yaml -f compose.gigafile.yaml up -d
```

## Cloudflare Tunnel

Add to `.env`:

```env
CLOUDFLARED_TUNNEL_TOKEN=your-token-here
GOKAPI_USE_CLOUDFLARE=true
```

Append `-f compose.cloudflared.yaml` to your command:

```bash
docker compose -f compose.gokapi.yaml -f compose.gigafile.yaml -f compose.cloudflared.yaml up -d
```

## Update

```bash
git pull
git -C Gokapi pull
```

### ddrive

```bash
git -C Gokapi fetch https://github.com/Nerahikada/Gokapi.git patch/s3-part-size-10mb
git -C Gokapi -c user.email=patch@local -c user.name=patch cherry-pick FETCH_HEAD
git -C ddrive pull

docker compose -f compose.gokapi.yaml -f compose.ddrive.yaml up -d --build
```

### gigafile-fs

```bash
git -C gigafile-fs pull

docker compose -f compose.gokapi.yaml -f compose.gigafile.yaml up -d --build
```
