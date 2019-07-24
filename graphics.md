# Graphics (Semi-Broken)

## Error TLP
*TODO: More details*

lspci | grep "NVIDIA" | cut -b -8

RUNTIME_PM_BLACKLIST="01:00.0"
RUNTIME_PM_DRIVER_BLACKLIST="amdgpu mei_me nouveau nvidia pcieport radeon"

## Error Access
Error: Cannot access secondary GPU - error: X did not start properly

Set the `"AutoAddDevices"` option to `true` in `/etc/bumblebee/xorg.conf.nvidia`