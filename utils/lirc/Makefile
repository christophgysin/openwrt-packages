#
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
# we use the $LINUX_DIR variable, which is defined in kernel.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=lirc
PKG_VERSION:=0.9.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=@SF/${PKG_NAME}
PKG_MD5SUM:=b232aef26f23fe33ea8305d276637086

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/lirc/Default
    SECTION:=utils
    CATEGORY:=Utilities
    URL:=http://lirc.org/
    TITLE:=LIRC - Linux Infrared Remote Control
endef

define Package/lirc-daemon
    $(call Package/lirc/Default)
    TITLE:=daemon
    DEPENDS:=@!LINUX_2_4
endef

define Package/lirc/Default/description
  LIRC is a package that allows you to decode and send 
  infra-red signals of many (but not all) commonly used 
  remote controls.
endef

define Package/lirc-daemon/description
  $(call Package/lirc/Default/description)
  This package contains the lircd daemon.
endef

define  Package/lirc-driver
  $(call Package/lirc/Default)
  TITLE:=LIRC drivers
endef

define Package/lirc-driver/description
	Contains the lirc drivers.
endef

define Package/lirc-lib
  $(call Package/lirc/Default)
  DEPENDS+= +lirc-daemon
  TITLE:=LIRC client library
endef

define Package/lirc-lib/description
  Contains the lirc client library.
endef

define Package/lirc-extra
  $(call Package/lirc/Default)
  TITLE:=LIRC extras
endef

define Package/lirc-extra/description
  Contains those additional daemon-tools:
   -irrecord
   -lircmd
endef

define Package/lirc-tools
  $(call Package/lirc/Default)
  TITLE:=LIRC tools
endef

define Package/lirc-tools/description
  This package contains the lirc-tools:
  ircat, irexec, irpty, irsend, irw, lircrcd, mode2
endef

CONFIGURE_ARGS += \
	--disable-nls \
	--disable-static \
	--with-kerneldir="$(LINUX_DIR)" \
	--with-usb="$(STAGING_DIR)/usr" \
	--with-driver=all \
	--without-x \
	--with-pic \
	--with-gnu-ld \
	--enable-sandboxed

BROKEN := \
	atiusb \
	i2c \
	wpc8769l

define Build/Configure
	(cd $(PKG_BUILD_DIR); ./configure $(CONFIGURE_ARGS))
	
	# don't try to build broken drivers
	for driver in $(BROKEN); do \
		sed -ie "s/lirc_$$$${driver}//" $(PKG_BUILD_DIR)/drivers/Makefile; \
	done
endef

define Package/lirc/conffiles
/etc/lircd.conf
endef

MAKE_FLAGS += \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	ARCH="$(LINUX_KARCH)"

define Build/Compile
	DESTDIR=$(PKG_INSTALL_DIR) make -C $(PKG_BUILD_DIR) install
endef

define Build/InstallDev
	$(INSTALL_DIR) \
		$(1)/usr/lib \
		$(1)/usr/include
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/lib/* \
		$(1)/usr/lib/
	$(CP) \
		$(PKG_INSTALL_DIR)/usr/include/* \
		$(1)/usr/include/
endef

define Package/lirc-daemon/install
	$(INSTALL_DIR) $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/daemons/lircd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/contrib/lircd.conf $(1)/etc/
endef

define Package/lirc-lib/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/liblirc*.so* $(1)/usr/lib/
endef

define Package/lirc-driver/install
	$(INSTALL_DIR) $(1)/lib
	$(CP) $(PKG_INSTALL_DIR)/lib $(1)
endef

define Package/lirc-extra/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irrecord $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/lircmd $(1)/usr/sbin/
endef

define Package/lirc-tools/install
	$(INSTALL_DIR) $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/mode2 $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/ircat $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irexec $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irpty $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irsend $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/irw $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/lircrcd $(1)/usr/bin/
endef

$(eval $(call BuildPackage,lirc-daemon))
$(eval $(call BuildPackage,lirc-driver))
$(eval $(call BuildPackage,lirc-lib))
$(eval $(call BuildPackage,lirc-tools))
$(eval $(call BuildPackage,lirc-extra))
