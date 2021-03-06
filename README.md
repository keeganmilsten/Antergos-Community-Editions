# Antergos Community Edition (antiso)
Modified version of archiso to build Antergos-Deepin (livecd)

## Dependencies
- antergos-gfxboot for a graphical boot (or isolinux/syslinux)
- arch-install-scripts
- cpio
- dosfstools
- gfxboot
- libisoburn
- mkinitcpio-nfs-utils
- make
- opendesktop-fonts
- patch
- squashfs-tools
- wget

## Free space

Please, check that you have 5GB (or more) of free harddisk space in your root partition:
`df -h /`

## Instructions (without docker) 

1. Install dependencies:
```
sudo pacman -S arch-install-scripts cpio dosfstools gfxboot libisoburn mkinitcpio-nfs-utils make patch squashfs-tools wget
```
2. Clone this repository using `--recursive` like this:
```
git clone https://github.com/keeganmilsten/Antergos-Community-Editions.git --recursive
```

4. Install our modified mkarchiso and configurations by running:
```
cd Antergos-Community-Editions
sudo make install
```

5a. While inside the `antergos-iso` folder, clone antergos-gfxboot:
```
su
cd /
cd usr/share/antergos-iso/
git clone https://github.com/keeganmilsten/antergos-gfxboot
exit
cd
```
5b. antergos-gfxboot is not using the `colors` branch, so these commands are necessary:
```
cd Antergos-Community-Editions/antergos-gfxboot/
git checkout colors 
su
cd
cd /
cd usr/share/antergos-iso/antergos-gfxboot/
git checkout colors
exit
```

6. Create `/work` and `/out` destination folders:
```
cd
sudo mkdir /work
sudo mkdir /out
```

The `/work` folder will store the livecd filesystem while the `/out` folder will store your new ISO file.

7. Go to the `config` directory you wish to build from.
- The "official" iso is in the `antergos` folder.
```
cd /home/user/antergos-iso/configs/antergos
```

8. Check text configuration file `config` with your favourite text editor.

9. Download openfonts package by heading here:

https://www.archlinux.org/packages/community/any/opendesktop-fonts/download/ 

Next, copy this to Antergos-Community-Editions/configs/deepin (or whatever version you want to build)

10. Clone the antergos iso-hotfix-utility:
```
cd Antergos-Community-Editions/configs/deepin/
sudo git clone https://github.com/antergos/iso-hotfix-utility
```

11. Copy the plymouth config files to deepin:
```
cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouth.initcpio_hook /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouth.initcpio_install /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/plymouth/plymouthd.conf /home/$USER/Antergos-Community-Editions/configs/deepin
```

12. Copy pacman files to deepin:
```
cp /home/$USER/Antergos-Community-Editions/configs/antergos/etc-pacman.d-gnupg.mount /home/$USER/Antergos-Community-Editions/configs/deepin

cp /home/$USER/Antergos-Community-Editions/configs/antergos/pacman-init.service /home/$USER/Antergos-Community-Editions/configs/deepin
```

13. Copy xorg directory to deepin:
```
cp -R /home/$USER/Antergos-Community-Editions/configs/antergos/root-image/etc/antergos/xorg /home/$USER/Antergos-Community-Editions/configs/deepin
```

14. Build the iso:
```
cd Antergos-Community-Editions/configs/deepin
sudo ./build.sh build
```

 **If you want to try to build the iso again, please remember to clean all generated files first:** 
 ```
 sudo ./build.sh clean
 ```
