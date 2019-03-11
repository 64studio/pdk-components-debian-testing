# the order the components are created in
# this may matter depending on the value of *_deps
components=minimal.xml standard.xml firmware-wifi.xml openssh-server.xml jackd2.xml xorg.xml lightdm.xml openbox.xml  lxde.xml raspberrypi-bootloader.xml raspberrypi-linux-image-default.xml

# minimal.xml
# packages included in a base install
minimal_pkgs=
minimal_select=Priority: required,important
minimal_exclude=tasksel, tasksel-data

# standard.xml
# some more packages you may see in a general system
standard_pkgs=alsa-utils, console-common, cpufrequtils, debconf-utils, fake-hwclock, gnupg, locales, ntp, psmisc, rfkill, rt-tests, sudo, whois
standard_deps=minimal.xml

# firmware-wifi.xml
# wifi adapter firmware
firmware-wifi_pkgs=firmware-atheros, firmware-brcm80211, firmware-realtek, firmware-ti-connectivity
firmware-wifi_deps=minimal.xml

# openssh-server.xml
# ssh server
openssh-server_pkgs=openssh-server
openssh-server_deps=minimal.xml

# jackd2.xml
# jackd2 server
jackd2_pkgs=jackd2
jackd2_deps=minimal.xml standard.xml

# xorg.xml
# Xorg server & support packages
xorg_pkgs=xserver-xorg, xserver-xorg-input-libinput, xserver-xorg-video-fbdev, x11-xserver-utils
xorg_deps=minimal.xml standard.xml

# lightdm.xml
# LightDM
lightdm_pkgs=lightdm, lightdm-autologin-greeter
lightdm_deps=minimal.xml standard.xml xorg.xml

# openbox.xml
# Openbox
openbox_pkgs=openbox
openbox_deps=minimal.xml standard.xml xorg.xml

# lxde.xml
# LXDE & support packages
lxde_pkgs=dbus-x11, lxde-core, lxterminal
lxde_deps=minimal.xml standard.xml xorg.xml

# raspberrypi-bootloader.xml
# Raspberry Pi bootloader & firmware
raspberrypi-bootloader_pkgs=raspberrypi-bootloader-nokernel, libraspberrypi-bin
raspberrypi-bootloader_deps=minimal.xml

# raspberrypi-linux-image-default.xml
# Raspberry Pi Linux image
# tracking https://github.com/raspberrypi/linux branch rpi-4.19.y
raspberrypi-linux-image-default_pkgs=$(shell ls debs/linux-image-*-raspberrypi-default*_armhf.deb | cut -d / -f 2 | cut -d _ -f 1)
raspberrypi-linux-image-default_deps=minimal.xml

all: clean update components download

.PHONY: clean
clean:
	rm -f *.xml

update:
	pdk channel update

components: $(components)

download:
	pdk download $(components)

%.xml:
	pdk abstract --arch=armhf --packages="$($*_pkgs)" --select="$($*_select)" --exclude="$($*_exclude)" --meta="<task>64studio-$*</task>" --label="$*" $@
	pdk resolve $@
	pdk closure --out-file=$@ --arch=armhf $($*_deps) $@
	pdk upgrade $@

