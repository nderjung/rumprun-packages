include ../Makefile.inc

UPSTREAM=https://busybox.net/downloads/busybox-1.29.2.tar.bz2
TARBALL=$(notdir $(UPSTREAM))

BUSYBOX_CONF_FILE = ./.config.dist
BUSYBOX_CONF_ENV += \
	CROSS_COMPILE=$(RUMPRUN_TOOLCHAIN_TUPLE)- \
	HOSTCC=$(RUMPRUN_CC)

all: bin/busybox

bin/busybox: build/busybox

build/busybox: build/.config
	$(BUSYBOX_CONF_ENV) $(MAKE) -C build

dl/$(TARBALL):
	mkdir -p dl
	../scripts/fetch.sh ${UPSTREAM} dl/$(TARBALL)

build/.config: | dl/$(TARBALL)
	mkdir -p build
	(cd build && tar -xj --strip-components 1 -f ../dl/$(TARBALL))
	cp $(BUSYBOX_CONF_FILE) build/.config

.PHONY: clean
clean:
	$(MAKE) -C build clean
	rm build/.config

.PHONY: distclean
distclean: clean
	rm -rf build

