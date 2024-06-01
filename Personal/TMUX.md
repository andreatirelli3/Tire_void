Resource video https://www.youtube.com/watch?v=DzNmUNvnB04
Install tmux
``` bash
sudo pacman -S tmux
```
Install tmux package manager (TPM)
=> https://github.com/tmux-plugins/tpm
``` bash
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
Create tmux config file
> In this guide is created in the `$XDG_CONFIG_HOME/tmux/tmux.conf` that traslate in `$HOME/.config/tmux/tmux.conf`. But is also possible in the `$HOME/.tmux.conf`
``` bash
touch ~/.config/tmux/tmux.conf
```
Now edit that file to add the following ling to source **tpm**.
``` bash
# List of plugins
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# Initialize TMUX plugin manager (keep this line at the very bottom of tmux.conf)
run '~/.tmux/plugins/tpm/tpm'
```
To run tmux with this configuration file run:
``` bash
# Run tmux
tmux

# Reload the configuration
tmux source ~/.config/tmux/tmux.conf
```
Keybinds
> Tmux can manage multiple session that each one have multiple windows, tmux is attached to only 1 session at the time.
> Each window can have multiple pane - split of the window.

Prefix key (default): CTRL + b

- New window: <\prefix\> + c
- Change window: <\prefix\> + "window number"
- Ciclye trough window:
	- next: <\prefix\> + n
	- previous: <\prefix\> + p
- Close window: <\prefix\> + &
- Split pane vertical: <\prefix> + %
- Split pane horizontal: <\prefix\> + "
- Nagivate through the pane: <\prefix\> + "arrow"
- Zoom in/out a pane: <\prefix\> + z
- Close pane: <\prefix\> + x
- Install plugin: <\prefix\> + I
Make navigation like vim (hjkl)
=> https://github.com/christoomey/vim-tmux-navigator
Fix tmux color
``` bash
set-option -sa terminal-overrides ",xterm+:Tc"
```
Remember to have in the .bashrc or .zshrc this export
``` bash
export TERM=xterm-256color
```
Prefix key
Change the prefix into CTRL+SPC, add
```
unbind C-b
set -g prefix C-Space
bind C-Space send-prefix
```
Install catppucin theme
=> https://github.com/catppuccin/tmux
