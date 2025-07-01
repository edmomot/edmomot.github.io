This post details how to create a systemd service that automatically updates Deluge's Listen Port when the forwarded port changes on a gluetun vpn connection container.

# Background

I have two relevant docker instances
1. Deluge
2. Gluetun connection using proton vpn, connected via openvpn credentials

Deluge uses the vpn container as its network.

When the VPN connection periodically changes to a new address, the forwarded port also changes.

Gluetun helpfully exposes the port via the file `/home/MY_HOME/protonvpn/tmp/forwarded_port`

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
      - /home/MY_HOME/deluge/config:/config
      - /MY_TORRENTS:/torrents
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

# Test changing deluge setting

`docker exec -i deluge deluge-console -c /config "config --set listen_ports (50000, 50000)"`

# Problems encountered with the deluge console command

## Config directory not set, permissions error

The `-c /config` directory setting must match the deluge docker config volume mapping. Otherwise there is a permissions error.

```
Could not connect to daemon: 127.0.0.1:58846
 Password does not match
```

Opening the interactive deluge-console command lists only a localclient connection, not the full list of connections I have configured.

## Listen_ports incorrect format, set to first character of port specified

Using this incorrect command:

`docker exec -i deluge deluge-console -c /config "config --set listen_ports 50000 50000"`

I got a message like this:

`Setting "listen_ports" to: ('9', '9', '9', '9', '9', ' ', '9', '9', '9', '9', '9')`

The port argument was interpreted as an array of characters. The correct format is to wrap in parentheses and comma-separate. `(1234, 1234)` instead of `1234 1234`.

# Setup

## Install file watcher

`sudo apt install inotify-tools`

## Create the `deluge-port-watcher.sh` script

In `/home/MY_HOME/deluge-port-watcher/deluge-port-watcher.sh`

```
#!/bin/bash

PORTFILE="/home/MY_HOME/protonvpn/tmp/forwarded_port"
PORTDIR="$(dirname "$PORTFILE")"
PORTNAME="$(basename "$PORTFILE")"

update_port() {
  if [[ -f "$PORTFILE" ]]; then
    port=$(cat "$PORTFILE")
    logger -t deluge-port-watcher "Setting Deluge listen port to $port"
    docker exec -i deluge deluge-console -c /config "config --set listen_ports ($port, $port)"
  else
    logger -t deluge-port-watcher "Port file missing: $PORTFILE"
  fi
}

# Update Deluge on startup
update_port

# Watch parent directory for changes to our port file. This is required because docker moves
# the file instead of updating it in place. So we watch the parent directory and make
# the update only if the event is for the file we care about.
inotifywait -m -e close_write,move,create "$PORTDIR" | \
while read -r directory events filename; do
  if [[ "$filename" == "$PORTNAME" ]]; then
    update_port
  fi
done
```

Make sure this file uses unix-style line endings

## Make the script executable
`sudo chmod +x /home/MY_HOME/deluge-port-watcher/deluge-port-watcher.sh`

## Create systemd service

Create this file `/etc/systemd/system/deluge-port-watcher.service`

```
[Unit]
Description=Deluge Port Watcher
After=docker.service

[Service]
Type=simple
ExecStart=/home/MY_HOME/deluge-port-watcher/deluge-port-watcher.sh
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

# Reload and start the service

`sudo systemctl daemon-reload`

`sudo systemctl enable --now deluge-port-watcher`

# Watch logs

`sudo journalctl -u deluge-port-watcher`

# Test

Manually change the forwarded port file to an arbitrary value. Confirm the change took effect in the Deluge UI, then change it back.
