
# Summary

Create a systemd service that automatically updates Deluge's Listen Port when the forward port changes on my vpn connection container.

# Background

I have two relevant docker instances
1. Deluge
2. Gluetun connection using proton vpn, connected via openvpn credentials

Deluge uses the vpn container as its network.

When the VPN connection periodically changes to a new address, the forwarded port also changes.

Gluetun helpfully exposes the port via the file `/home/ed/protonvpn/tmp/forwarded_port`

We can watch this file for changes and use deluge console to update the `Incoming Port` setting.


## Deluge Docker compose file

```
services:
  deluge:
    image: lscr.io/linuxserver/deluge:latest
    container_name: deluge
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
      - DELUGE_LOGLEVEL=error #optional
    volumes:
      - /home/ed/deluge/config:/config
      - /mnt/raid/media/torrents:/torrents
    network_mode: "container:gluetun"
    restart: unless-stopped
    # Port forwarding is configured in protonvpn / gluetun container
```

## Gluetun docker compose file

```
version: "3"
services:
  gluetun:
    image: qmcgaw/gluetun
    container_name: gluetun
    cap_add:
      - NET_ADMIN
    devices:
      - /dev/net/tun:/dev/net/tun
    environment:
      - VPN_SERVICE_PROVIDER=protonvpn
      - VPN_PORT_FORWARDING=on
      - PORT_FORWARD_ONLY=on
      - VPN_PORT_FORWARDING_UP_COMMAND=/bin/sh -c "echo {{PORTS}}"
      - OPENVPN_USER=MY_OPENVPN_USER
      - OPENVPN_PASSWORD=MY_OPENVPN_PASSWORD
    ports:
      - 8112:8112 # Deluge web ui
      - 58846:58846 # Deluge daemon
      # Other ports for other containers here that you want to use this connection. e.g. cardigann, jackett
    volumes:
      - /home/MY_USER/protonvpn/tmp:/tmp/gluetun
    restart: unless-stopped
```