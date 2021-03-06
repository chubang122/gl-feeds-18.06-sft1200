#
# Copyright (C) 2014-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libumqtt
PKG_VERSION:=1.1.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL=https://github.com/zhaojh329/libumqtt/releases/download/v$(PKG_VERSION)
PKG_HASH:=94e90b5021ca7831d8c1408d56cdabd0776dad35712e75c54a5975f990500da8
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Jianhui Zhao <zhaojh329@gmail.com>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE

CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/libumqtt/Default
  SECTION:=libs
  CATEGORY:=Libraries
  SUBMENU:=Networking
  TITLE:=A lightweight mqtt client library based on libev
  DEPENDS:=+libev $(2)
  VARIANT:=$(1)
  PROVIDES:=libumqtt
endef

Package/libumqtt-openssl=$(call Package/libumqtt/Default,openssl,+PACKAGE_libumqtt-openssl:libopenssl)
Package/libumqtt-wolfssl=$(call Package/libumqtt/Default,wolfssl,+PACKAGE_libumqtt-wolfssl:libwolfssl)
Package/libumqtt-mbedtls=$(call Package/libumqtt/Default,mbedtls,+PACKAGE_libumqtt-mbedtls:libmbedtls +PACKAGE_libumqtt-mbedtls:zlib)
Package/libumqtt-nossl=$(call Package/libumqtt/Default,nossl)

ifeq ($(BUILD_VARIANT),openssl)
  CMAKE_OPTIONS += -DUSE_OPENSSL=ON
else ifeq ($(BUILD_VARIANT),wolfssl)
  CMAKE_OPTIONS += -DUSE_WOLFSSL=ON
else ifeq ($(BUILD_VARIANT),mbedtls)
  CMAKE_OPTIONS += -DUSE_MBEDTLS=ON
else
  CMAKE_OPTIONS += -DSSL_SUPPORT=OFF
endif

define Package/libumqtt-$(BUILD_VARIANT)/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/usr/lib/libumqtt.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libumqtt-openssl))
$(eval $(call BuildPackage,libumqtt-mbedtls))
$(eval $(call BuildPackage,libumqtt-wolfssl))
$(eval $(call BuildPackage,libumqtt-nossl))
