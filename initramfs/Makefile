# See LICENSE file for copyright and license details

ARCH ?= amd64

BUSYBOX = 1
BUSYBOX_VERSION = 1.27.2
BUSYBOX_URL = https://busybox.net/downloads/busybox-$(BUSYBOX_VERSION).tar.bz2

GNUPG = 1
GNUPG_VERSION = 2.1.18-8~deb9u1
GNUPG_URL = https://deb.debian.org/debian/pool/main/g/gnupg2/gpgv-static_$(GNUPG_VERSION)_$(ARCH).deb

all: dirs busybox gnupg initramfs.cpio.gz

dirs:
	mkdir -p build
	cd build && mkdir -p bin dev etc lib mnt/root proc root sys tmp usr var/run
	cp -a /dev/console build/dev
	cp -a /dev/null build/dev
	cp -a /dev/random build/dev
	cp -a /dev/tty build/dev
	cp -a /dev/urandom build/dev
	cp -ra skel/* build/
	cp -ra skel/.gnupg build/


busybox:
ifeq ($(BUSYBOX),1)
	wget -c $(BUSYBOX_URL)
	tar xf $(shell basename $(BUSYBOX_URL))
	cp -f busybox.config $(shell basename -s .tar.bz2 $(BUSYBOX_URL))/.config
ifeq ($(ARCH),i386)
	cd $(shell basename -s .tar.bz2 $(BUSYBOX_URL)) && \
		make -j9 CFLAGS="-Os -static -m32" LDFLAGS="-s -static -m32" && \
		cp -f busybox $(PWD)/build/bin
else
	cd $(shell basename -s .tar.bz2 $(BUSYBOX_URL)) && \
		make -j9 CFLAGS="-Os -static" LDFLAGS="-s -static" && \
		cp -f busybox $(PWD)/build/bin
endif
	cd $(PWD)/build/bin && /bin/busybox --install -s .
endif

gnupg:
ifeq ($(GNUPG),1)
	wget -c $(GNUPG_URL)
	ar x $(shell basename $(GNUPG_URL))
	tar xf data.tar.xz
	cp -f usr/bin/gpgv-static $(PWD)/build/bin/gpgv
	rm -rf usr data.tar.xz control.tar.xz debian-binary
endif

initramfs.cpio.gz:
	cd build && \
        find . -print0 | cpio --null -ov --format=newc | gzip --best > $(PWD)/$@

clean:
	rm -rf build busybox-* gpgv-*

distclean: clean
	rm -f initramfs.cpio.gz

