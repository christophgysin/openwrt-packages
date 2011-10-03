#
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libao
PKG_VERSION:=1.1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://downloads.xiph.org/releases/ao/
PKG_MD5SUM:=2b2508c29bc97e4dc218fa162cf883c8

PKG_FIXUP:=libtool
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libao/Default
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=A cross platform audio library
  URL:=http://www.xiph.org/ao/
endef

define Package/libao
  $(call Package/libao/Default)
  DEPENDS:=+alsa-lib
  VARIANT:=nopulseaudio
endef

define Package/libao-pulse
  $(call Package/libao/Default)
  DEPENDS:=+libao +(!PACKAGE_libpulse-dbus):libpulse
  TITLE+= (pulseaudio plugin)
  VARIANT:=pulseaudio
endef

define Package/libao/description
	Libao is a cross-platform audio library that allows programs to
	output audio using a simple API on a wide variety of platforms.
endef

define Package/libao-pulse/description
	libao plugin for pulseaudio
endef

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-esd \
	--disable-arts \
	--disable-nas \

ifeq (${BUILD_VARIANT},pulseaudio)
CONFIGURE_ARGS += \
	--disable-pulse
else
CONFIGURE_ARGS += \
	--enable-pulse
endif

TARGET_CFLAGS += $(FPIC)

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ao $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libao.{a,so*} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/ao.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libao/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libao.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/ao/plugins-4
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ao/plugins-4/lib*.so $(1)/usr/lib/ao/plugins-4/
endef

define Package/libao-pulse/install
	$(INSTALL_DIR) $(1)/usr/lib/ao/plugins-4
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/ao/plugins-4/libpulse.so $(1)/usr/lib/ao/plugins-4/
endef

$(eval $(call BuildPackage,libao))
$(eval $(call BuildPackage,libao-pulse))
