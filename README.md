# Clipboard Sync

Synchronizes the clipboard across multiple X11 and wayland instances running on the same machine.

Example use cases:

- **Multiple displays**: You run two or more desktop environments or window managers in separate ttys, switching between desktops using ctrl-alt-F*. 
- **Nested wayland**: You run a wayland compositor within a window. examples:
  - you primarily use kde, but run sway in a window to consolidate all your messenger apps into a single tiled/tabbed window.
  - you use gnome and develop extensions for gnome, so you run a nested gnome environment for testing.
- **VNC**: You run a VNC server and would like all host and client logins from the same user to share the same clipboard.

# Installation
Most users should install clipboard-sync with their operating system's package manager. If your system is not supported, please vote on [the appropriate issue](https://github.com/dnut/clipboard-sync/issues?q=is%3Aopen+is%3Aissue+label%3Adistribution).

While waiting for support on your system, the Generic Linux approach is recommended. The Cargo installation is an alternative approach for discerning users.

## Arch Linux
[clipboard-sync](https://aur.archlinux.org/packages/clipboard-sync) is available in the Arch User Repository.

## Ubuntu & Debian
For now, you have to build your own deb file from source. But the steps are simple.

1. Install the build tools:
  - `sudo apt install make gcc libxcb1-dev libxcb-render0-dev libxcb-shape0-dev libxcb-xfixes0-dev`
  - rust: https://www.rust-lang.org/tools/install
2. Download the code from the latest release on [the releases page](https://github.com/dnut/clipboard-sync/releases/) or using git:
```bash
git clone https://github.com/dnut/clipboard-sync.git
cd clipboard-sync
```
3. Build the deb file
```bash
make && make deb
```
3. Install the deb file
```bash
sudo apt install ./dist/deb/clipboard-sync_*.deb
```

## Generic Linux
1. Install rust: https://www.rust-lang.org/tools/install
2. Download the code from the latest release on [the releases page](https://github.com/dnut/clipboard-sync/releases/) or using git:
```bash
git clone https://github.com/dnut/clipboard-sync.git
cd clipboard-sync
git checkout stable
```
3. Install ***either*** system-wide ***or*** for only the current user:
```bash
make && sudo make install  # system
make && make user-install  # user
```
It can be easily uninstalled:
```bash
sudo make uninstall  # system
make user-uninstall  # user
```

## Cargo
[clipboard-sync](https://crates.io/crates/clipboard-sync) is published to crates.io, so it can be installed as a normal binary crate.

1. Install rust: https://www.rust-lang.org/tools/install
2. Install clipboard-sync
```bash
cargo install clipboard-sync
```
3. If you want it to run in the background, you can use tmux, or you can manually download the systemd service file and copy it to a systemd folder. For example:
```bash
wget -P "$HOME/.config/systemd/" https://raw.githubusercontent.com/dnut/clipboard-sync/master/clipboard-sync.service
```
It can be easily uninstalled:
```bash
cargo uninstall clipboard-sync
rm -r "$HOME/.config/systemd/clipboard-sync.service"
```

# Usage
The typical set-and-forget approach is to enable to service:
```bash
systemctl --user enable --now clipboard-sync
```

If you don't want it to run constantly, only on-demand, don't use systemd. Directly call the binary as needed:
```bash
clipboard-sync
```

You can also daemonize clipboard-sync using tmux instead of systemd. ~/.bashrc aliases may be handy for these commands.
```bash
tmux new-session -ds clipboard-sync clipboard-sync  # start in background
tmux attach -t clipboard-sync                       # view status
ctrl-b, d                                           # while viewing status, send back to background
ctrl-c                                              # while viewing status, terminate the process
```
