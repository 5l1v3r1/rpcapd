include ../vars.mk

PKG = rpcapd.tgz
INSTALLER_TARGET = install-rpcapd.sh
RPCAPD_TARGET = rpcapd
RPCAPD_DIST_DIR = wpcap/libpcap/rpcapd/rpcapd-dist
# for Linux:
RPCAPD_FILES = \
    rpcapd-64bit-linux          \
	rpcapd.sh                   \
	rpcapd.debianinit           \
	rpcapd.sysvinit

ifdef WINCC_VERSION
SUBDIRS += \
	packetNtx/Dll/Project \
	wpcap/PRJ \

endif

SUBDIRS += \
	wpcap/libpcap/rpcapd \

.PHONY: all
all: $(SUBDIRS)

.PHONY: clean
clean: $(SUBDIRS)
	rm -f $(INSTALLER_TARGET)

.PHONY: install
install: $(SUBDIRS)
	tar -cvf - -C $(RPCAPD_DIST_DIR) $(RPCAPD_FILES) \
	| cat extract.sh - > $(INSTALLER_TARGET)
	chmod 0755 $(INSTALLER_TARGET)
	$(INSTALL) -d install/www
	$(INSTALL) -d install/www/tools
	$(INSTALL) -m 0755 $(INSTALLER_TARGET) install/www/tools
#	Windows: put PS script both in .zip and outside it
	zip -j install/www/tools/rpcapd-64bit-windows.zip \
		   packetNtx/driver/npf.sys \
		   packetNtx/Dll/Project/Packet.dll \
		   ../win32-pthreads/pthreadGC2.dll \
		   wpcap/PRJ/wpcap.dll \
		   wpcap/libpcap/rpcapd/rpcapd.exe \
		   wpcap/libpcap/rpcapd/rpcapd-dist/install-rpcapd.ps1
	$(INSTALL) -m 755 wpcap/libpcap/rpcapd/rpcapd-dist/install-rpcapd.ps1 \
					  install/www/tools/install-rpcapd.ps1
ifdef PACKAGES
	tar -C install -cvzf $(PACKAGES)/$(PKG) .
endif

.PHONY: distclean
distclean: clean
	rm -rf install
ifdef PACKAGES
	rm -f $(PACKAGES)/$(PKG)
endif

.PHONY: $(SUBDIRS)
$(SUBDIRS):
	$(MAKE) -C $@ $(MAKECMDGOALS)

