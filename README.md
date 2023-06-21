# Pi-hole Docker Setup

This is a guide to setting up Pi-hole using Docker. deployment using Docker meant for local usage. Specialized for my own personal deployment. Edit the variable in `docker-compose.yml`

## Prerequisites

- Docker and Docker Compose installed on your system.

## Configuration

1. Create or use the existing `docker-compose.yml` file with the following content:

```yaml
version: "3"

# More info at https://github.com/pi-hole/docker-pi-hole/ and https://docs.pi-hole.net/
services:
  pihole:
    container_name: pihole
    image: pihole/pihole:latest
    # For DHCP it is recommended to remove these ports and instead add: network_mode: "host"
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "67:67/udp" # Only required if you are using Pi-hole as your DHCP server
      - "80:80/tcp"
    environment:
      TZ: 'Asia/Jakarta'
      WEBPASSWORD: 'somepassword'
      FTLCONF_LOCAL_IPV4: '192.168.1.102'
    # Volumes store your data between container upgrades
    volumes:
      - './etc-pihole:/etc/pihole'
      - './etc-dnsmasq.d:/etc/dnsmasq.d'
    #   https://github.com/pi-hole/docker-pi-hole#note-on-capabilities
    # cap_add:
    #  - NET_ADMIN # Required if you are using Pi-hole as your DHCP server, else not needed
    restart: unless-stopped
```

Update the environment section with your desired settings, such as timezone `TZ` and web interface password `WEBPASSWORD` and the static IP `FTLCONF_LOCAL_IPV4`.

If you are using Pi-hole as your DHCP server, uncomment the cap_add section and remove the port mappings for ports 53 and 67.

Run `docker-compose up -d` to start the Pi-hole container.

## Usage
Access the Pi-hole web interface at http://<your-ip-address>/admin.
Log in using the password you set in the WEBPASSWORD environment variable.
More Information
For more information, please refer to the [Pi-hole documentation](https://docs.pi-hole.net/main/basic-install/) and the [Pi-hole Docker repository](https://github.com/pi-hole/pi-hole).