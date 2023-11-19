# Fedora setup

## Fast dnf

open the dnf config file

```bash
sudo nano /etc/dnf/dnf.conf
```

and add these two lines

```
fastestmirror=true
max_parallel_downloads=10 (press Ctrl + O & Ctrl + X to save)
```

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
