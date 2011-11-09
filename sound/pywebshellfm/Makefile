#
# Copyright (C) 2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=pywebshellfm
PKG_REV:=8658608e25a4f8deafbcdc657c7b033708c6bac8
PKG_VERSION:=0.1
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/christophgysin/PyWebShellFM.git
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/pywebshellfm
  SECTION:=sound
  CATEGORY:=Sound
  TITLE:=Simple web-interface for shell-fm
  URL:=http://www.jezra.net/blog/python_shellfm_web_interface
  DEPENDS:=+python-webpy
endef

define Package/pywebshellfm/description
python shell-fm web interface
endef

define Build/Compile
endef

define Package/pywebshellfm/install
	$(INSTALL_DIR) \
		$(1)/etc/init.d \
		$(1)/usr/bin \
		$(1)/usr/share/${PKG_NAME}

	$(CP) ./files/pywebshellfm.conf $(1)/etc/
	$(CP) ./files/pywebshellfm.init $(1)/etc/init.d/pywebshellfm
	$(INSTALL_BIN) ./files/pywebshellfm $(1)/usr/bin/${PKG_NAME}

	$(CP) ${PKG_BUILD_DIR}/index.py $(1)/usr/share/${PKG_NAME}
	$(CP) ${PKG_BUILD_DIR}/output $(1)/usr/share/${PKG_NAME}
	$(CP) ${PKG_BUILD_DIR}/static $(1)/usr/share/${PKG_NAME}
	$(CP) ${PKG_BUILD_DIR}/templates $(1)/usr/share/${PKG_NAME}
endef

$(eval $(call BuildPackage,pywebshellfm))
