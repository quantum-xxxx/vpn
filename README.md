**VPNs playbooks in Ansible**

- [Main requirements](#main-requirements)
- [Preparation](#preparation)
  - [GO-Gost HTTPS proxy](#go-gost-https-proxy)
    - [Descriptions](#descriptions)
    - [Manual preparation steps](#manual-preparation-steps)
    - [Run](#run)
  - [Shadowsocks](#shadowsocks)
    - [Run](#run-1)
- [Not simple solutions](#not-simple-solutions)
  - [OpenVPN](#openvpn)
  - [WG](#wg)
  - [IPSEC](#ipsec)


## Main requirements

1. VM on hosting on Ubuntu 22.04. Tested solutions:
   1. https://www.robovps.biz/
   2. https://webhost1.ru/
   3. https://pq.hosting/

## Preparation
   1. Create VM
   2. Login to VM via SSH
   3. If you login as not user root, execute `sudo su -`
   4. Upgrade all soft and install needed software by `apt update && apt upgrade -y && apt install -y ansible git && reboot`
   6. Clone this repo by `cd /root && git clone https://github.com/quantum-xxxx/vpn.git`


###  GO-Gost HTTPS proxy

#### Descriptions

[gost](roles/gost) - the [go-gist](https://github.com/go-gost/gost) server in simple https proxy configuration. Include:
   1. [lego](https://github.com/go-acme/lego) for SSL certificates management
   2. freemyip.com for DNS names

Create https server on  433 port which answer as broken http server with 500 error on any not proxy requests.

This is the best battery safe option on smartphone because you can configure only use in dedicated applications.

1. Unfortunately not all applications supports configuration https proxy with login and password
2. For Chrome on desktop and phone the best choose is [FoxProxy](https://chromewebstore.google.com/detail/foxyproxy/gcknhkkoolaabfmlnjonogaaifnjlfnp) for proxy configuration.

#### Manual preparation steps

Create host on freemyip.com and get API token. On example it will be `my-super-vpn-host.freemyip.com`

#### Run

```sh
cd /root/vpn
ansible-playbook -t gost \
  -i ./inventory \
  -e 'gost_user=proxy_user' \
  -e 'gost_user_pass=proxy_user_pass' \
  -e 'lego_email=any_your_email' \
  -e 'freemyip_domain=my-super-vpn-host.freemyip.com' \
  -e 'freemyip_token=some_freemyip_token' \
  main.yaml
```

when change
* 'proxy_user' - to your any proxy login
* 'proxy_user_pass' - to your any proxy password
* 'any_your_email' - any our email for let's encrypt login
* 'some_freemyip_token' - api token from freemyip.com for you host my-super-vpn-host.freemyip.com


###  Shadowsocks

[shadowsocks](roles/shadowsocks)

**Documentation IN PROGRESS**

#### Run

```sh
cd /root/vpn
ansible-playbook -t shadowsocks \
  -i ./inventory \
  -e 'shadowsocks_secret=shadowsocks_password' \
  main.yaml
```

when change
* 'shadowsocks_password' - to your password for shadowsocks


## Not simple solutions

### OpenVPN

[openvpn](roles/openvpn)

**Documentation IN PROGRESS**

### WG

[wg](roles/wg)

**Documentation IN PROGRESS**

### IPSEC

[ipsec](roles/ipsec)

**Documentation IN PROGRESS**
