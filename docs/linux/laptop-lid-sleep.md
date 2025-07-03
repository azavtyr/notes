# Configure Laptop Lid Close Action and Screen Sleep Timeout

## Close laptop lid

Edit `/etc/systemd/logind.conf`:

- Uncomment `HandleLidSwitch` and change `suspend` on `ignore`.
- Uncomment `HandleLidSwitchDocked`.
- `systemctl restart systemd-logind.service`

## Put your screen to sleep

Edit `/etc/default/grub`:

- Add to `GRUB_CMDLINE_LINUX` this: `consoleblank=300` (After 300 seconds the screen will go to sleep)
- `update-grub`