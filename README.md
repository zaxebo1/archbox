# Archbox
Installs Arch Linux inside a chroot enviroment.
## Why?
Ever since I'm running some niche distros like Void, Solus, I had a problem finding softwares I need in their not-so-large repositories, also I don't like how flatpak and snap works. so i decided to create an Arch Linux chroot enviroment everytime I distrohop. Why Arch Linux? They have a really, really good repositories, oh and don't mention how big AUR is.
## Installation
### Installing Archbox
It's pretty easy, just run ```install.sh``` as root.
### Installing chroot enviroment
Before creating chroot enviroment, edit your username in ```/etc/archbox.conf```, then do :
```
sudo archbox --create <archlinux tarball download link>
```
### Configuring filesystem automount
Execute ```/usr/local/share/archbox/bin/archboxinit``` on boot.
either create an init service, or create a @reboot cronjob.
### Removing chroot enviroment
**IMPORTANT**, Make sure you've unmounted everything in chroot enviroment, if you're unsure which partitions must be unmounted, remove the init script and reboot, then delete the folder.
### Entering chroot enviroment
To enter chroot, do :
```
archbox --enter
```
### Executing commands in chroot enviroment
To execute commands inside chroot envirotment, do :
```
archbox <command>
```
for example, to update chroot, do :
```
archbox sudo pacman -Syu
```
### Optional steps
You may want to add this if you don't want to run archbox chroot without password :
#### Sudo
```
%wheel  ALL=(root) NOPASSWD: /usr/local/share/archbox/bin/archbox,/usr/local/share/archbox/bin/copyresolv,/usr/local/share/archbox/bin/remount_run
```
#### Doas
```
Idk you're on your own
```
### Misc
#### Systemd services
Use ```servicectl``` command to manage systemd services.
More info [here](https://github.com/smaknsk/servicectl)
#### Lauching apps via rofi
Instead of opening terminal everytime you want to run application inside chroot, you may want to launch rofi inside chroot, install rofi and do :
```
archbox rofi -show drun
```
#### Prompt
If you use bash with nerd font you could add a nice little Arch Linux icon in your prompt, add :
```
[[ -e /etc/arch-release ]] && export PS1=" $PS1"
```
to your ```~/.bashrc```
#### Adding enviroment variables
Edit ENV_VAR in ```/etc/archbox.conf```. For example, if you want to use qt5ct as Qt5 theme, edit it like this :
```
ENV_VAR="QT_QPA_PLATFORMTHEME=qt5ct"
```
An example with multiple enviroment variables.
```
ENV_VAR="QT_QPA_PLATFORMTHEME=qt5ct GTK_CSD=0 LD_PRELOAD=/var/home/lemniskett/git_repo/gtk3-nocsd/libgtk3-nocsd.so.0"
```
### Known issues
#### Void Musl (or any Musl-based distros, need confirmation)
Although /run is mounted in chroot enviroment on boot, XDG_RUNTIME_DIR is not visible in chroot enviroment, remounting /run will make it visible. do :
```
archbox --remount-run
```
after user login, Also you need to kill every process that runs in XDG_RUNTIME_DIR when you log out, if you use ```startx``` you need to reinstall archbox with ```--exp``` flag and use ```startx-killxdg``` instead of ```startx```.
Tested in Void Linux musl.
