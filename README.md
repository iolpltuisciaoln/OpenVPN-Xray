
```
You ──▶ [OpenVPN client]
             │
             │  TCP :9443
             ▼
      ┌─────────────────┐
      │   Mordor Xray   │  ══╗
      │  dokodemo-door  │    ║ VLESS + XTLS-Vision
      └─────────────────┘    ║ (looks like HTTPS to www.google.com)
                             ║
      ┌──────────────────────╩──────────────────────┐
      │              DisneyLand                     │
      │  ┌────────────┐       ┌───────────────────┐ │
      │  │ Xray Core  │──────▶│  OpenVPN Server   │ │──▶ 🌍 Internet
      │  │ vless :7443│       │    TCP :1194      │ │
      │  └────────────┘       └───────────────────┘ │
      └────────────────────────────────────────────-┘
```

# DisneyLand

### 1. Generate UUID :

```sh
❯ uuidgen
dda3aba2-9a3c-4415-99c1-2315e1bfc02b
```

### 2. Replace UUID in `xray/config.json`

### 3. Start Docker

```sh
cd DisneyLand
docker compose up -d
```

### 4. docker ps

```
╭───────────────────────────────────────────────────────────────────────────────╮
│ >                                                                             │
│   2/2 (0)      ────────────────────────────────────────────────────────────── │
│ ▌ xray-disneyland (teddysun/xray) 0.0.0.0:7443->7443/tcp, [::]:7443->7443/tcp │
│ ▌ openvpn (alekslitvinenk/openvpn) 0.0.0.0:1194->1194/tcp, 1194/udp, 8080/tcp │
╰───────────────────────────────────────────────────────────────────────────────╯
```

### 5. Get OpenVPN key

```sh
docker exec ovpn wget -qO- 0.0.0.0:8080>client.ovpn
```

### 6. Generate moar keys

```sh
docker exec -it openvpn sh
cd /opt/Dockovpn && /opt/Dockovpn/genclient.sh n john_doe > john_doe.ovpn
```

### 7. Edit john_doe.ovpn

update `remote` to Mordor Xray address/port:
`remote Mordor_IP 9443`


# Mordor

### 1. Replace UUID in `xray/config.json`
... with previously generated UUID (`dda3aba2-9a3c-4415-99c1-2315e1bfc02b`)

### 2. In `xray/config.json` set DisneyLand IP/Xray Port (7443)

### 3. Start Docker

```sh
cd Mordor
docker compose up -d
```
### 4. docker ps

```
╭───────────────────────────────────────────────────────────────────────────────╮
│ >                                                                             │
│   1/1 (0)      ────────────────────────────────────────────────────────────── │
│ ▌ xray-mordor   (teddysun/xray)   0.0.0.0:9443->9443/tcp, [::]:9443->9443/tcp │
╰───────────────────────────────────────────────────────────────────────────────╯
```

### 5. ???

### 6. Profit!
