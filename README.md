# rawdog

Example configuration for [rawdog](http://offog.org/code/rawdog/) RSS aggregator under [Arch Linux](https://archlinux.org).

## Installation

Install rawdog from the [official repository](https://www.archlinux.org/packages/?name=rawdog):
```bash
# pacman -S rawdog
```

Rawdog looks for the configuration files in the `~/.rawdog` directory. You might have to create it and put a basic `config` file in there.
For more information on rawdog see `$ man rawdog`.

## Automatic update

To update your feeds you can use `systemctl.timer`.
Create a file `~/.config/systemd/user/rawdog.timer`:
```
[Unit]
Description=Run rawdog to update newsfeeds

[Timer]
OnBootSec=2m
OnUnitActiveSec=5m

[Install]
WantedBy=timers.target
```
`OnBootSec` defines the delay between boot and the first time the timer will be executed.
`OnUnitActiveSec` defines the delay between any activation and the next one.

Next you will need the file `~/.config/systemd/user/rawog.service`:
```
[Unit]
Description=rawdog update service

[Service]
Type=oneshot
Nice=19
IOSchedulingClass=2
IOSchedulingPriority=7
ExecStart=/usr/bin/rawdog -uw
```

Lastly start and enable the timer:
```
$ systemctl start rawdog.timer
$ systemctl enable rawdog.timer
```

You can check the status of the timer with `$ systemctl status -l rawdog.service` and `$ systemctl list-timers`.
