### AndroidStudio

#### Install

- Install version
  - 3.2 Canary 14
- Install directory
  - /opt/AndroidStudioPreview3.2Canary14
- Sdk directory
  - ~/Android/Sdk/


### Setup

- Install Sdk
 - install API 19
- create .desktop to lunch this app on desktop
 - Android Studio -> [Tools] -> [Create Desktop Entry]
- adb pass setting
- Projects directory
 - /home/shouhei/StudioProjects

### KVM install

- Check whether you have KVM installed
```
shouhei@shouhei-ubuntu:~/Android/Sdk/tools$ sudo ./emulator -accel-check
accel:
0
KVM (version 12) is installed and usable.
accel
```

```
$ sudo apt-get install qemu-kvm libvirt-bin ubuntu-vm-builder bridge-utils
グループ `libvirtd' (GID 130) を追加しています...
完了。
```

https://developer.android.com/studio/run/emulator-acceleration
