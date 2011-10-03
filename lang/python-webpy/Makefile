#
# Copyright (C) 2010 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=web.py
PKG_VERSION:=0.36
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://webpy.org/static/
PKG_MD5SUM:=

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/python-webpy
  SECTION:=language-python
  CATEGORY:=Languages
  SUBMENU:=Python
  TITLE:=python-webpy
  URL:=http://webpy.org/
  DEPENDS:=+python
endef

define Package/python-webpy/description
simple python web framework
endef

define Build/Compile
	# avoid importing the module to get the version
	sed -i -e "s/^from web/#from web/" -e "s/__version__/'${PKG_VERSION}'/" $(PKG_BUILD_DIR)/setup.py

	$(call Build/Compile/PyMod,,install --prefix=/usr --root=$(PKG_INSTALL_DIR))
endef

define Package/python-webpy/install
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) \
		$(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/* \
		$(1)$(PYTHON_PKG_DIR)
endef

$(eval $(call BuildPackage,python-webpy))
