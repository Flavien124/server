version: "3.5"
services:

  # 1. 🦆 DuckDNS Client (Mise à jour IPv6)
  duckdns:
    image: lscr.io/linuxserver/duckdns
    container_name: duckdns
    restart: unless-stopped
    environment:
      # --- À MODIFIER ---
      - SUBDOMAINS=flavienbce.duckdns.org
      - TOKEN=76f8e4e0-d8a3-48c9-86e8-21e1dd336e80
      # --- NE PAS MODIFIER ---
      - IP_VERSION=6
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Paris
      - LOG_FILE=true
    volumes:
      - ./duckdns:/config

  # 2. 🔒 WireGuard VPN Server
  wireguard:
    image: lscr.io/linuxserver/wireguard
    container_name: wireguard
    cap_add:
      - NET_ADMIN
    environment:
      # --- À MODIFIER ---
      - SERVERURL=flavienbce.duckdns.org)
      # --- NE PAS MODIFIER ---
      - INTERNAL_SUBNET=10.13.13.0/24,fd42:42:42::/64
      - PEERDNS=10.13.13.1,fd00:db8:1::1 
      - ALLOWEDIPS=0.0.0.0/0, ::/0
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Paris
      - SERVERPORT=51820 
    ports:
      - "51820:51820/udp"
    volumes:
      - ./wireguard:/config
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
      - net.ipv6.conf.all.disable_ipv6=0
    restart: unless-stopped
    
  # 3. 🛡️ AdGuard Home
  adguardhome:
    image: adguard/adguardhome
    container_name: adguardhome
    restart: unless-stopped
    ports:
      - "53:53/tcp"
      - "53:53/udp"
      - "8080:80/tcp" 
    volumes:
      - ./adguard/work:/opt/adguardhome/work
      - ./adguard/conf:/opt/adguardhome/conf
