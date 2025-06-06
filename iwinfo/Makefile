#
# Copyright (C) 2010-2016 Jo-Philipp Wich <jo@mein.io>
# Copyright 2022 Morse Micro
#
# This is free software, licensed under the GPL 2 license.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libiwinfo
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/MorseMicro/iwinfo.git
PKG_SOURCE_DATE:=2024-10-18
PKG_SOURCE_VERSION:=88de1400de808127891ff9780d09fe56526d504b
PKG_HASH:=261be17a411c45d29075c2484e25859da5982ab2ecb8049651e79c276a23a319
PKG_MAINTAINER:=Morse Micro <info@morsemicro.com>
PKG_LICENSE:=GPL-2.0

IWINFO_ABI_VERSION:=20230701

include $(INCLUDE_DIR)/package.mk


define Package/libiwinfo
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Generalized Wireless Information Library With HaLow Support(iwinfo)
  # Don't select morsecli explicitly to avoid strange circular dep.
  # If morsecli isn't present, some things won't work
  # (as of writing, only noise stats).
  DEPENDS:=+libnl-tiny +libuci +libubus +libiwinfo-data
  ABI_VERSION:=$(IWINFO_ABI_VERSION)
endef

define Package/libiwinfo/description
  Wireless information library with simplified API for nl80211
  and wext driver interfaces. With HaLow support
endef


define Package/libiwinfo-lua
  SUBMENU:=Lua
  SECTION:=lang
  CATEGORY:=Languages
  TITLE:=libiwinfo Lua binding
  DEPENDS:=+libiwinfo +liblua
endef

define Package/libiwinfo-lua/description
  This is the Lua binding for the iwinfo library. It provides access to all enabled
  backends. With HaLow support
endef


define Package/libiwinfo-data
  TITLE:=libiwinfo Lua binding
  HIDDEN:=1
endef


define Package/iwinfo
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Generalized Wireless Information utility With HaLow support
  DEPENDS:=+libiwinfo
endef

define Package/iwinfo/description
  Command line frontend for the wireless information library.  With HaLow support
endef


define Build/Configure
endef

TARGET_CFLAGS += \
	-I$(STAGING_DIR)/usr/include/libnl-tiny \
	-I$(STAGING_DIR)/usr/include \
	-D_GNU_SOURCE

MAKE_FLAGS += \
	FPIC="$(FPIC)" \
	CFLAGS="$(TARGET_CFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	BACKENDS="nl80211" \
	SOVERSION="$(IWINFO_ABI_VERSION)"

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/iwinfo
	$(CP) $(PKG_BUILD_DIR)/include/iwinfo.h $(1)/usr/include/
	$(CP) $(PKG_BUILD_DIR)/include/iwinfo/* $(1)/usr/include/iwinfo/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libiwinfo.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/lua
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/iwinfo.so $(1)/usr/lib/lua/iwinfo.so
endef

define Package/libiwinfo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libiwinfo.so.$(IWINFO_ABI_VERSION) $(1)/usr/lib/libiwinfo.so.$(IWINFO_ABI_VERSION)
endef

define Package/libiwinfo-lua/install
	$(INSTALL_DIR) $(1)/usr/lib/lua
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/iwinfo.so $(1)/usr/lib/lua/iwinfo.so
endef

define Package/libiwinfo-data/install
	$(INSTALL_DIR) $(1)/usr/share/libiwinfo
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/devices.txt $(1)/usr/share/libiwinfo/devices.txt
endef

define Package/iwinfo/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/iwinfo $(1)/usr/bin/iwinfo
endef

$(eval $(call BuildPackage,libiwinfo))
$(eval $(call BuildPackage,libiwinfo-lua))
$(eval $(call BuildPackage,libiwinfo-data))
$(eval $(call BuildPackage,iwinfo))