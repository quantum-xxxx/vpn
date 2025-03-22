Tested on:

* MacOS and Ubuntu with xray 25.3.6
* v2rayNG on Android v1.9.39

After deploy put some big files to /var/lib/docker/volumes/caddy_www/_data/storage, on example some sources from opensource world or

```bash
apt install p7zip-full
dd if=/dev/urandom bs=1M count=10240 \
  | 7z a -p"$(tr -dc A-Za-z0-9 </dev/urandom | head -c32)" \
  -si big_secret_archive.7z
```
