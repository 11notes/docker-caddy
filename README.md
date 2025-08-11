![banner](https://github.com/11notes/defaults/blob/main/static/img/banner.png?raw=true)

# CADDY
![size](https://img.shields.io/docker/image-size/11notes/caddy/2.10.0?color=0eb305)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![version](https://img.shields.io/docker/v/11notes/caddy/2.10.0?color=eb7a09)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![pulls](https://img.shields.io/docker/pulls/11notes/caddy?color=2b75d6)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)[<img src="https://img.shields.io/github/issues/11notes/docker-CADDY?color=7842f5">](https://github.com/11notes/docker-CADDY/issues)![5px](https://github.com/11notes/defaults/blob/main/static/img/transparent5x2px.png?raw=true)![swiss_made](https://img.shields.io/badge/Swiss_Made-FFFFFF?labelColor=FF0000&logo=data:image/svg%2bxml;base64,PHN2ZyB2ZXJzaW9uPSIxIiB3aWR0aD0iNTEyIiBoZWlnaHQ9IjUxMiIgdmlld0JveD0iMCAwIDMyIDMyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxyZWN0IHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiIgZmlsbD0idHJhbnNwYXJlbnQiLz4KICA8cGF0aCBkPSJtMTMgNmg2djdoN3Y2aC03djdoLTZ2LTdoLTd2LTZoN3oiIGZpbGw9IiNmZmYiLz4KPC9zdmc+)

Run caddy rootless and distroless.

# INTRODUCTION 📢

Caddy is a web server written in Go, known for its simplicity and automatic HTTPS features. It acts as a powerful and flexible reverse proxy, handling various protocols like HTTP, HTTPS, WebSockets, gRPC, and FastCGI.

# SYNOPSIS 📖
**What can I do with this?** This image will run caddy [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) and [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md), for maximum security.

# UNIQUE VALUE PROPOSITION 💶
**Why should I run this image and not the other image(s) that already exist?** Good question! Because ...

> [!IMPORTANT]
>* ... this image runs [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) as 1000:1000
>* ... this image has no shell since it is [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md)
>* ... this image has a health check
>* ... this image runs read-only
>* ... this image is automatically scanned for CVEs before and after publishing
>* ... this image is created via a secure and pinned CI/CD process
>* ... this image verifies all external payloads
>* ... this image is very small

If you value security, simplicity and optimizations to the extreme, then this image might be for you.

# COMPARISON 🏁
Below you find a comparison between this image and the most used or original one.

| **image** | 11notes/caddy:2.10.0 | caddy:2.10.0 |
| ---: | :---: | :---: |
| **image size on disk** | 19.3MB | 50.5MB |
| **process UID/GID** | 1000/1000 | 0/0 |
| **distroless?** | ✅ | ❌ |
| **rootless?** | ✅ | ❌ |


# DEFAULT CONFIG 📑
```json
{
	"apps": {
		"http": {
			"servers": {
        "health": {
					"listen": ["127.0.0.1:3000"],
					"routes": [
						{
							"handle": [{
								"handler": "static_response",
								"status_code": 200
							}]
						}
					]
				},
				"demo": {
					"listen": [":80"],
					"routes": [
						{
							"handle": [{
								"handler": "static_response",
								"body": "11notes/caddy"
							}]
						}
					]
				}
			}
		}
	},
  "storage":{
    "module": "file_system",
    "root": "/caddy/var"
  }
}

```

# VOLUMES 📁
* **/caddy/etc** - Directory of your default.json config
* **/caddy/var** - Directory of all dynamic data

# COMPOSE ✂️
```yaml
name: "proxy"
services:
  caddy:
    image: "11notes/caddy:2.10.0"
    read_only: true
    environment:
      TZ: "Europe/Zurich"
    ports:
      - "80:80/tcp"
      - "443:443/tcp"
    volumes:
      - "caddy.etc:/caddy/etc"
      - "caddy.var:/caddy/var"
      # optional volume (can be tmpfs instead) to store backups of your config
      - "caddy.backup:/caddy/backup"
    networks:
      frontend:
    sysctls:
      # allow rootless container to access port 80 and higher
      net.ipv4.ip_unprivileged_port_start: 80
    restart: "always"

volumes:
  caddy.etc:
  caddy.var:
  caddy.backup:

networks:
  frontend:
```

# DEFAULT SETTINGS 🗃️
| Parameter | Value | Description |
| --- | --- | --- |
| `user` | docker | user name |
| `uid` | 1000 | [user identifier](https://en.wikipedia.org/wiki/User_identifier) |
| `gid` | 1000 | [group identifier](https://en.wikipedia.org/wiki/Group_identifier) |
| `home` | /caddy | home directory of user docker |

# ENVIRONMENT 📝
| Parameter | Value | Default |
| --- | --- | --- |
| `TZ` | [Time Zone](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones) | |
| `DEBUG` | Will activate debug option for container image and app (if available) | |
| `XDG_CONFIG_HOME` | Directory where to store backups of your config | /caddy/backup |

# MAIN TAGS 🏷️
These are the main tags for the image. There is also a tag for each commit and its shorthand sha256 value.

* [2.10.0](https://hub.docker.com/r/11notes/caddy/tags?name=2.10.0)

### There is no latest tag, what am I supposed to do about updates?
It is of my opinion that the ```:latest``` tag is dangerous. Many times, I’ve introduced **breaking** changes to my images. This would have messed up everything for some people. If you don’t want to change the tag to the latest [semver](https://semver.org/), simply use the short versions of [semver](https://semver.org/). Instead of using ```:2.10.0``` you can use ```:2``` or ```:2.10```. Since on each new version these tags are updated to the latest version of the software, using them is identical to using ```:latest``` but at least fixed to a major or minor version.

If you still insist on having the bleeding edge release of this app, simply use the ```:rolling``` tag, but be warned! You will get the latest version of the app instantly, regardless of breaking changes or security issues or what so ever. You do this at your own risk!

# REGISTRIES ☁️
```
docker pull 11notes/caddy:2.10.0
docker pull ghcr.io/11notes/caddy:2.10.0
docker pull quay.io/11notes/caddy:2.10.0
```

# SOURCE 💾
* [11notes/caddy](https://github.com/11notes/docker-CADDY)

# PARENT IMAGE 🏛️
> [!IMPORTANT]
>This image is not based on another image but uses [scratch](https://hub.docker.com/_/scratch) as the starting layer.
>The image consists of the following distroless layers that were added:
>* [11notes/distroless](https://github.com/11notes/docker-distroless/blob/master/arch.dockerfile) - contains users, timezones and Root CA certificates
>* [11notes/distroless:curl](https://github.com/11notes/docker-distroless/blob/master/curl.dockerfile) - app to execute HTTP requests

# BUILT WITH 🧰
* [caddy](https://github.com/caddyserver/caddy)

# GENERAL TIPS 📌
> [!TIP]
>* Use a reverse proxy like Traefik, Nginx, HAproxy to terminate TLS and to protect your endpoints
>* Use Let’s Encrypt DNS-01 challenge to obtain valid SSL certificates for your services

# CAUTION ⚠️
> [!CAUTION]
>* Don’t forget to add the ```127.0.0.1:3000``` listen directive to your config with a HTTP 200 status code for the default health check or create your own!
>* The default.json config has a server listening on HTTP, don’t do that. Normally redirect HTTP to HTTPS. There are exceptions[^1] for HTTP use.

[^1]: Some OTA or other services only work via HTTP (unencrypted) since adding all the Root CA would not fit into their limited memory.

# ElevenNotes™️
This image is provided to you at your own risk. Always make backups before updating an image to a different version. Check the [releases](https://github.com/11notes/docker-caddy/releases) for breaking changes. If you have any problems with using this image simply raise an [issue](https://github.com/11notes/docker-caddy/issues), thanks. If you have a question or inputs please create a new [discussion](https://github.com/11notes/docker-caddy/discussions) instead of an issue. You can find all my other repositories on [github](https://github.com/11notes?tab=repositories).

*created 03.08.2025, 02:20:05 (CET)*