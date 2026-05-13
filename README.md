![banner](https://raw.githubusercontent.com/11notes/static/refs/heads/master/img/banner/README.png)

# CADDY
![size](https://img.shields.io/badge/image_size-34MB-green?color=%2338ad2d)![5px](https://raw.githubusercontent.com/11notes/static/refs/heads/master/img/markdown/transparent5x2px.png)![pulls](https://img.shields.io/docker/pulls/11notes/caddy?color=2b75d6)![5px](https://raw.githubusercontent.com/11notes/static/refs/heads/master/img/markdown/transparent5x2px.png)[<img src="https://img.shields.io/github/issues/11notes/docker-caddy?color=7842f5">](https://github.com/11notes/docker-caddy/issues)![5px](https://raw.githubusercontent.com/11notes/static/refs/heads/master/img/markdown/transparent5x2px.png)![swiss_made](https://img.shields.io/badge/Swiss_Made-FFFFFF?labelColor=FF0000&logo=data:image/svg%2bxml;base64,PHN2ZyB2ZXJzaW9uPSIxIiB3aWR0aD0iNTEyIiBoZWlnaHQ9IjUxMiIgdmlld0JveD0iMCAwIDMyIDMyIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciPgogIDxyZWN0IHdpZHRoPSIzMiIgaGVpZ2h0PSIzMiIgZmlsbD0idHJhbnNwYXJlbnQiLz4KICA8cGF0aCBkPSJtMTMgNmg2djdoN3Y2aC03djdoLTZ2LTdoLTd2LTZoN3oiIGZpbGw9IiNmZmYiLz4KPC9zdmc+)

Run caddy rootless and distroless.

# INTRODUCTION 📢

Caddy is a web server written in Go, known for its simplicity and automatic HTTPS features. It acts as a powerful and flexible reverse proxy, handling various protocols like HTTP, HTTPS, WebSockets, gRPC, and FastCGI.

# SYNOPSIS 📖
**What can I do with this?** This image will run caddy [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) and [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md), for maximum security. This image will by default use the JSON format. If you don’t want that but you want to use the Caddyfile format, simply check the [compose.yml](https://github.com/11notes/docker-caddy/blob/master/compose.yml) for the command and make sure your Caddyfile contains at least these settings for the storage and health check to work:

```
{
	storage file_system /caddy/var
}
127.0.0.1:3000 {
  respond / 200
}
```

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

| **image** | **size on disk** | **init default as** | **[distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md)** | supported architectures
| ---: | ---: | :---: | :---: | :---: |
| 11notes/caddy | 34MB | 1000:1000 | ✅ | amd64, arm64, armv7 |
| caddy | 62MB | 0:0 | ❌ | amd64, amd64, amd64, arm64v8, armv6, armv7, ppc64le, riscv64, s390x |

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

x-lockdown: &lockdown
  # prevents write access to the image itself
  read_only: true
  # prevents any process within the container to gain more privileges
  security_opt:
    - "no-new-privileges=true"

services:
  caddy:
    image: "11notes/caddy:2.11.3"
    # use Caddyfile instead of json
    # command: ["run", "--config", "/caddy/etc/Caddyfile"]
    <<: *lockdown
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
To find out how you can change the default UID/GID of this container image, consult the [RTFM](https://github.com/11notes/RTFM/blob/main/linux/container/image/11notes/how-to.changeUIDGID.md#change-uidgid-the-correct-way).

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

* [2.11.3](https://hub.docker.com/r/11notes/caddy/tags?name=2.11.3)
* [2.11.3-unraid](https://hub.docker.com/r/11notes/caddy/tags?name=2.11.3-unraid)
* [2.11.3-nobody](https://hub.docker.com/r/11notes/caddy/tags?name=2.11.3-nobody)

### There is no latest tag, what am I supposed to do about updates?
It is my opinion that the ```:latest``` tag is a bad habbit and should not be used at all. Many developers introduce **breaking changes** in new releases. This would messed up everything for people who use ```:latest```. If you don’t want to change the tag to the latest [semver](https://semver.org/), simply use the short versions of [semver](https://semver.org/). Instead of using ```:2.11.3``` you can use ```:2``` or ```:2.11```. Since on each new version these tags are updated to the latest version of the software, using them is identical to using ```:latest``` but at least fixed to a major or minor version. Which in theory should not introduce breaking changes.

If you still insist on having the bleeding edge release of this app, simply use the ```:rolling``` tag, but be warned! You will get the latest version of the app instantly, regardless of breaking changes or security issues or what so ever. You do this at your own risk!

# REGISTRIES ☁️
```
docker pull 11notes/caddy:2.11.3
docker pull ghcr.io/11notes/caddy:2.11.3
docker pull quay.io/11notes/caddy:2.11.3
```

# UNRAID VERSION 🟠
This image supports unraid by default. Simply add **-unraid** to any tag and the image will run as 99:100 instead of 1000:1000.

# NOBODY VERSION 👻
This image supports nobody by default. Simply add **-nobody** to any tag and the image will run as 65534:65534 instead of 1000:1000.

# SOURCE 💾
* [11notes/caddy](https://github.com/11notes/docker-caddy)

# PARENT IMAGE 🏛️
> [!IMPORTANT]
>This image is not based on another image but uses [scratch](https://hub.docker.com/_/scratch) as the starting layer.
>The image consists of the following distroless layers that were added:
>* [11notes/distroless](https://github.com/11notes/docker-distroless/blob/master/arch.dockerfile) - contains users, timezones and Root CA certificates, nothing else
>* [11notes/distroless:localhealth](https://github.com/11notes/docker-distroless/blob/master/localhealth.dockerfile) - app to execute HTTP requests only on 127.0.0.1

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

*created 13.05.2026, 09:51:39 (CET)*