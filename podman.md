# Podman Containers

Install: `yay -Syu podman podman-compose`

/etc/subuid and /etc/subgid configuration:
```
sudo touch /etc/subuid && chmod +rw /etc/subuid
sudo touch /etc/subgid && chmod +rw /etc/subgid
usermod --add-subuids 200000-201000 --add-subgids 200000-201000 $USER
```

If pulling fails after adding uid and guids
```
rm -rf ~/.config/containers ~/.local/share/containers
podman system migrate
podman unshare cat /proc/self/uid_map
```
