# Fedora setup

## Fast dnf

open the dnf config file

```bash
sudo nano /etc/dnf/dnf.conf
```

and add these two lines

```
fastestmirror=true
max_parallel_downloads=10
```
(press Ctrl + O & Ctrl + X to save)

# Disable discord switching profiles for bluetooth headset

https://askubuntu.com/questions/1481852/disable-auto-switch-of-bluetooth-device-profiles-specifically-input-when-apps

The solution is described here: https://wiki.archlinux.org/title/PipeWire#Automatic_profile_selection

One needs to create a global configuration file for wireplumber:
`/etc/wireplumber/policy.lua.d/11-bluetooth-policy.lua` (or a user-specific one at `~/.config/wireplumber/`)

and add the following option:

```lua
bluetooth_policy.policy["media-role.use-headset-profile"] = false
```

Restart pipewire or your machine and now the profiles shouldn't change automatically, and thus preserve manual control of input / mic settings across apps!

# RPM FUSION

## Allowing non-free software on your machine

- This is every user must-do unless they are crazy to not want proprietary software at all

https://rpmfusion.org/Configuration

# Installing video codecs

https://www.reddit.com/r/Fedora/comments/v4qq99/unable_to_play_youtube_videos/

```bash
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
sudo dnf install lame\* --exclude=lame-devel
sudo dnf group upgrade --with-optional Multimedia
```

AND

https://rpmfusion.org/Howto/Multimedia

```bash
sudo dnf swap ffmpeg-free ffmpeg --allowerasing
sudo dnf groupupdate multimedia --setop="install_weak_deps=False" --exclude=PackageKit-gstreamer-plugin
sudo dnf groupupdate sound-and-video
```

# Installing broadcom drivers:

```bash
sudo dnf install broadcom-wl
```

# NumLock on by default

https://github.com/sddm/sddm/issues/1717#issuecomment-1799140279

```bash
sudo nano /var/lib/sddm/.config/kcminputrc
```

```
[Keyboard]
NumLock=0
```

# Disable sleep/suspend

```bash
sudo systemctl mask suspend.target
sudo systemctl mask sleep.target
```

## Remove it from the menu

```bash
sudo mkdir -p /usr/local/etc/polkit-1/rules.d/
sudo nano /usr/local/etc/polkit-1/rules.d/51-disable-suspend.rules
```

```polkit.addRule(function(action, subject) {
    if (action.id == "org.freedesktop.login1.suspend" ||
        action.id == "org.freedesktop.login1.suspend-multiple-sessions" ||
        action.id == "org.freedesktop.login1.hibernate" ||
        action.id == "org.freedesktop.login1.hibernate-multiple-sessions" ||
        action.id == "org.freedesktop.consolekit.system.suspend" ||
        action.id == "org.freedesktop.consolekit.system.suspend-multiple-users" ||
        action.id == "org.freedesktop.consolekit.system.hibernate" ||
        action.id == "org.freedesktop.consolekit.system.hibernate-multiple-users" ||
        action.id == "org.freedesktop.consolekit.system.hybridsleep" ||
        action.id == "org.freedesktop.consolekit.system.hybridsleep-multiple-users")
    {
        return polkit.Result.NO;
    }
});
```

# Make windows default in grub

https://www.gnu.org/software/grub/manual/grub/grub.html#Simple-configuration
https://askubuntu.com/questions/52963/how-do-i-set-windows-to-boot-as-the-default-in-the-boot-loader#answer-82965
https://unix.stackexchange.com/questions/152222/what-is-the-equivalent-of-update-grub-for-rhel-fedora-and-centos-systems
1. 
```
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```
2. 
```
sudo grep menuentry /boot/efi/EFI/fedora/grub.cfg
```
3. copy the entry you want to boot as default ('Windows Boot Manager (on /dev/nvme0n1p1)' or 'windows' for my computer)
4.
```
sudo nano -B /etc/default/grub
```
6. Edit this line or add it if it doesn't exist. You will replace windows boot manager with what you got in 3.
```
GRUB_DEFAULT="Windows Boot Manager (on /dev/nvme0n1p1)"
```
8. apply the changes
```
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```
```
sudo reboot now
```
## Remembering the last booted system instead:
1. ```sudo nano -B /etc/default/grub```
2. Edit these lines or add them if they don't exist
```GRUB_DEFAULT=saved```
```GRUB_SAVEDEFAULT=true```
3. apply the changes
```
sudo grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```
```
sudo reboot now
```

- -B parameter makes a backup in the directory with the same file and ~ before it. It's safer when working with grub.
