# Stream Deck configuration notes

## Source repository

Use Martin Wimpress' fork of the main repository: https://github.com/flexiondotorg/streamdeck-ui

## Autostart via systemd

I am utilizing systemd from my user account.  Setup procedure:

1. Create directory if it does not exist: `mkdir -p ~/.config/systemd/user`
2. Create bash script called `streamdeck_start.sh` in a subdirectory called `scripts` with following contents:


```
#!/bin/bash
/home/eric/.local/bin/streamdeck
```

3. Make script executable: `chmod u+x ~/scripts/streamdeck_start.sh`
4. Create a systemd unit file called `streamlink.service` with following contents inside `~/.config/systemd/user`:

```
[Unit]
Description=Autostart streamdeck
After=network.target

[Service]
Type=simple
ExecStart=/bin/bash /home/eric/scripts/streamdeck_start.sh
TimeoutStartSec=0

[Install]
WantedBy=default.target
```

5. Enable service to start at every boot and start manually (if first time setting up):

```
systemctl --user enable streamlink.service
systemctl --user start streamlink.service
```

The icon should appear in the upper right tray.  If not, check status with `systemctl --user status streamlink.service`