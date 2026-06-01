# Dummy Systemd Service

This project covers how to set up a basic, long-running background script managed by systemd on a remote Linux server. 

## Project Steps Taken

### 1. Created the Background Script
A simple Bash script was created to run an infinite loop that logs a message every 10 seconds.

File location: `/usr/local/bin/dummy.sh`
```bash
#!/bin/bash
while true; do
  echo "Dummy service is running..." >> /var/log/dummy-service.log
  sleep 10
done
```
The file permissions were updated to make it executable:
```
sudo chmod +x /usr/local/bin/dummy.sh
```
## 2. Created the Systemd Service File

A custom service file was created so systemd can manage the script, ensure it starts on boot, and automatically restart it if it crashes.

File location: /etc/systemd/system/dummy.service
```Plaintext

[Unit]
Description=Dummy Roadmap Background Service
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash /usr/local/bin/dummy.sh
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### How to Manage the Service

After saving the configuration, the systemd daemon was reloaded to read the new file:
```Bash

sudo systemctl daemon-reload
```

The service can now be managed using standard systemctl commands:
```Bash

# Start the service immediately
sudo systemctl start dummy

# Enable the service to run on server boot
sudo systemctl enable dummy

# Check if the service is running properly
sudo systemctl status dummy

# Stop and disable the service
sudo systemctl stop dummy
sudo systemctl disable dummy
```

### Checking the Logs

The service writes logs to two places which were verified using the terminal:

The custom text log file:
```Bash
    tail -f /var/log/dummy-service.log
```

The systemd journal logs:
```Bash

    sudo journalctl -u dummy -f
```
