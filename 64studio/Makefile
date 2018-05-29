components=minimal.xml standard.xml openssh-server.xml linux-image-armhf-raspi.xml

# minimal packages for a base install
minimal_pkgs=
minimal_select=Priority: required,important
minimal_exclude=tasksel, tasksel-data

# standard packages
standard_pkgs=gnupg, locales, debconf-utils
standard_deps=minimal.xml

# openssh-server.xml: openssh-server
openssh-server_pkgs=openssh-server
openssh-server_deps=minimal.xml

# linux-image-raspi.xml: raspi firmware
linux-image-armhf-raspi_pkgs=rpi-stock-kernel-firmware
linux-image-armhf-raspi_deps=minimal.xml


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
	pdk abstract --arch=armhf --packages="$($*_pkgs)" --select="$($*_select)" --exclude="$($*_exclude)" --meta="<task>$*</task>" --label="$*" $@
	pdk resolve $@
	# TODO: remove the need to specify minimal.xml in *_deps
	pdk closure --out-file=$@ --arch=armhf $($*_deps) $@
	pdk upgrade $@

