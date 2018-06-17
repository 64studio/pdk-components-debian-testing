# the order the components are created in
# this may matter depending on the value of *_deps
components=minimal.xml standard.xml firmware-wifi.xml openssh-server.xml linux-image-armhf-raspi.xml jackd2.xml xorg.xml lxde.xml

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

# linux-image-raspi-armhf.xml
# RasPi firmware & kernel
linux-image-armhf-raspi_pkgs=rpi-stock-kernel-firmware
linux-image-armhf-raspi_deps=minimal.xml

# jackd2.xml
# jackd2 server
jackd2_pkgs=jackd2
jackd2_deps=minimal.xml standard.xml

# xorg.xml
# Xorg server & support packages
xorg_pkgs=xserver-xorg, xserver-xorg-input-libinput, xserver-xorg-video-fbdev
xorg_deps=minimal.xml standard.xml

# lxde.xml
# LXDE & support packages
lxde_pkgs=dbus-x11, dbus-x11, lxde-core, lightdm, lxterminal
lxde_deps=minimal.xml standard.xml xorg.xml

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
	# TODO: remove the need to specify minimal.xml in *_deps
	pdk closure --out-file=$@ --arch=armhf $($*_deps) $@
	pdk upgrade $@

