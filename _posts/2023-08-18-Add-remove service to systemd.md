---
title: Add & remove service to systemd
date: 2023-08-18 20:04:00 +0700
categories: [server_administrator]
tags: [systemd, service, ubutu-20.04]
---

# Add service
```console
chmod +x <executabe file>

service_name=<service>
cd /etc/systemd/system
sudo nano $service_name
```

```
[Unit]
Description=<description about this service>

[Service]
ExecStart=<script which needs to be executed, don't need ./>

[Install]
WantedBy=multi-user.target
```

```console
sudo systemctl daemon-reload
sudo systemctl start $service_name
sudo systemctl status $service_name
sudo systemctl enable $service_name
```
# Remove service
```console
service_name=<service>
sudo systemctl stop $service_name
sudo systemctl status $service_name
sudo systemctl disable $service_name
sudo rm /etc/systemd/system/$service_name
sudo rm /usr/lib/systemd/system/$service_name
sudo systemctl daemon-reload
```
