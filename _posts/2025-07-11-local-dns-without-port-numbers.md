I am bad at remembering port numbers, and I have a few things running on my server. It's much easier to remember "homeassistant.internal" than it is to remember that "home assistant is on 10.1.1.2 on port 8123".

This document describes my setup for local DNS resolution and reverse proxy to give each service its own URL without port numbers.

# Network Diagram

```

                        ┌───────────────────────────────┐ 
                        │             Server            │ 
┌────────────────────┐  │            10.1.1.2           │ 
│       Router       │  │  ┌──────────────────────────┐ │ 
│                    │  │  │ DNS                  :53 │ │ 
│ Default DNS Config │  │  └──────────────────────────┘ │ 
└────────────────────┘  │  ┌──────────────────────────┐ │ 
                        │  │ Caddy Reverse Proxy  :80 │ │ 
                        │  └──────────────────────────┘ │ 
┌───────────────────┐   │  ┌──────────────────────────┐ │ 
│  Client Computer  │   │  │ Home Assistant     :8123 │ │ 
│                   │   │  └──────────────────────────┘ │ 
│ Initiates request │   │  ┌──────────────────────────┐ │ 
└───────────────────┘   │  │ Octoprint          :8080 │ │ 
                        │  └──────────────────────────┘ │ 
                        └───────────────────────────────┘ 

```

[https://asciiflow.com/](https://asciiflow.com/)

# Request flow

## Before request

1. Client connects to router and gets the router's recommended DNS server list.

## Request

1. User navigates to homeassistant.internal in browser
1. Browser sends a network address lookup to the computer's predefined DNS servers, where 10.1.1.2 is the first entry, asking "what IP address is homeassistant.internal on?"
1. DNS server on the server uses its configuration to reply "homeassistant.internal is on 10.1.1.2"
1. The browser then sends a web request to 10.1.1.2:80 with a header that includes "homeassistant.internal".
1. Caddy, the reverse proxy, is listening on port 80. It inspects the header for matching configs, and sees that "homeassistant.internal" is mapped to "127.0.0.1:8123".
1. Caddy initiates a new web request to 127.0.0.1:8123 with the same contents as the incoming request.
1. The home assistant server receives a web request, processes, and replies back to Caddy.
1. Caddy replies to the client with the contents of Home Assistant's reply.


# dnsmasq DNS server install

`sudo apt install dnsmasq`

# DNS server config

Create file `/etc/dnsmasq.d/internal.conf` with these contents

```
# Forward unknown .internal queries to Unifi router
server=/internal/10.1.1.1

# Define local services only as needed
address=/homeassistant.internal/10.1.1.2
address=/octoprint.internal/10.1.1.2
```

Restart: `sudo systemctl restart dnsmasq`

# Configure router to use the server as a DNS server

For my UNIFI router

Settings > Networks > My Network 

Set `Advanced` to `Manual`

Under DNS Server, add the server's IP address as DNS Server 1, then add some global fallbacks, e.g. `1.1.1.1` or `8.8.8.8`.

Save, then reconnect to the router to get the latest DNS settings pushed to your computer. On windows, easiest to turn the network interface off and on. On Linux,  `sudo dhclient -r && sudo dhclient`

# Test DNS resolution

`nslookup homeassistant.internal`

```bash
➜  ~ nslookup homeassistant.internal
Server:         10.1.1.2
Address:        10.1.1.2#53

Name:   homeassistant.internal
Address: 10.1.1.2
```

This confirms that the DNS server is working, and the current machine knows to use that DNS server.

# Caddy reverse proxy install

```
sudo apt update
sudo apt install -y debian-keyring debian-archive-keyring apt-transport-https
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | sudo gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | sudo tee /etc/apt/sources.list.d/caddy-stable.list
sudo apt update
sudo apt install caddy
```

# Configure Caddy

Add a caddy config file in `/etc/caddy/Caddyfile`

```
# Global config
{
  auto_https off
}

# Home Assistant on port 8123
homeassistant.internal:80 {
  reverse_proxy 127.0.0.1:8123
}

# OctoPrint on port 8080
octoprint.internal:80 {
  reverse_proxy 127.0.0.1:8080
}
```

Then reload: `sudo systemctl reload caddy`

# Port conflicts

Caddy must run on port 80. If another service is already running on port 80, it must be moved to a different port first, otherwise that service will accept all traffic from the proxy instead of the intended service.

# Home assistant

`~/homeassistant/config/configuration.yml` need to contain this config at the root level for home assistant to accept traffic from the proxy

```
http:
    use_x_forwarded_for: true
    trusted_proxies:
        - 127.0.0.1
```

# Using .internal for hostname mapping for other network devices

For consistency, I want my router to use `.internal` for automatic hostname based URLs. E.g. `my_desktop.internal` should automatically work with any device that joins the network.

Using a wildcard in the dnsmasq config prevents this from working.

Instead, each device needs to be manually mapped, with an explicit fallback to the router's IP address, where it can respond with the automatic hostname mapping.

The downside is that two places must change when adding a new service to the config: caddy and DNS config. I'm sure this can be automated, but odds are low that this will happen often.

