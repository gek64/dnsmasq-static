# dnsmasq static

## Features

- Static compilation dnsmasq for amd64 architecture Linux

## Install

```sh
# Download dnsmasq
curl -Lo- https://github.com/gek64/dnsmasq-static/releases/latest/download/dnsmasq.gz | gzip > /usr/local/bin/dnsmasq
chmod +x /usr/local/bin/dnsmasq

# Create a configuration example
cat << "EOF" >/usr/local/etc/dnsmasq/dnsmasq.conf
port=53

server=1.1.1.1
server=1.0.0.1

address=/local.internal/127.0.0.1
EOF

# Create service
cat << "EOF" >/etc/systemd/system/dnsmasq.service
[Unit]
Description=Dnsmasq Service
After=network.target
Before=network-online.target nss-lookup.target
Wants=nss-lookup.target

[Service]
Type=simple
ExecStartPre=/usr/local/bin/dnsmasq --test
ExecStart=/usr/local/bin/dnsmasq -k -C /usr/local/etc/dnsmasq/dnsmasq.conf
ExecReload=/bin/kill -HUP $MAINPID

[Install]
WantedBy=multi-user.target
EOF

# Enable service and start service
systemctl enable dnsmasq && systemctl restart dnsmasq
```

## License

- **MIT License**
- See `LICENSE` for details
