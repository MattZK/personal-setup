# Power Management

## Sleep state
By default, the very inefficient s2idle suspend variant is incorrectly selected. The much more efficient deep variant should be selected instead:

in `/etc/default/grub` and append `mem_sleep_default=deep` to `GRUB_CMDLINE_LINUX_DEFAULT`

```
GRUB_CMDLINE_LINUX_DEFAULT="mem_sleep_default=deep"
```

## Powertop

`powertop` is efficient to manage power consumption.