${{ content_synopsis }} This image will run caddy [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) and [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md), for maximum security.

${{ content_uvp }} Good question! Because ...

${{ github:> [!IMPORTANT] }}
${{ github:> }}* ... this image runs [rootless](https://github.com/11notes/RTFM/blob/main/linux/container/image/rootless.md) as 1000:1000
${{ github:> }}* ... this image has no shell since it is [distroless](https://github.com/11notes/RTFM/blob/main/linux/container/image/distroless.md)
${{ github:> }}* ... this image has a health check
${{ github:> }}* ... this image runs read-only
${{ github:> }}* ... this image is automatically scanned for CVEs before and after publishing
${{ github:> }}* ... this image is created via a secure and pinned CI/CD process
${{ github:> }}* ... this image verifies all external payloads
${{ github:> }}* ... this image is very small

If you value security, simplicity and optimizations to the extreme, then this image might be for you.

${{ content_comparison }}

${{ title_config }}
```json
${{ include: ./rootfs/caddy/etc/default.json }}
```

${{ title_volumes }}
* **${{ json_root }}/etc** - Directory of your default.json config
* **${{ json_root }}/var** - Directory of all dynamic data

${{ content_compose }}

${{ content_defaults }}

${{ content_environment }}
| `XDG_CONFIG_HOME` | Directory where to store backups of your config | /caddy/backup |

${{ content_source }}

${{ content_parent }}

${{ content_built }}

${{ content_tips }}

${{ title_caution }}
${{ github:> [!CAUTION] }}
${{ github:> }}* Don’t forget to add the ```127.0.0.1:3000``` listen directive to your config with a HTTP 200 status code for the default health check or create your own!
${{ github:> }}* The default.json config has a server listening on HTTP, don’t do that. Normally redirect HTTP to HTTPS. There are exceptions[^1] for HTTP use.

[^1]: Some OTA or other services only work via HTTP (unencrypted) since adding all the Root CA would not fit into their limited memory.