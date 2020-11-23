# Terminal Settings

## Terminal

```yay -Syu zsh oh-my-zsh-git```

```cp /usr/share/oh-my-zsh/zshrc ~/.zshrc```

### Plugins
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)

### Disable middle click paste
Install: `xbindkeys xsel xdotool`

Edit `~/.xbindkeysrc` and add:
```
"echo -n | xsel -n -i; pkill xbindkeys; xdotool click 2; xbindkeys"
b:2 + Release
```

Reload config: `xbindkeys -p`

On startup: `xbindkeys`
