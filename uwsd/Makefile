include $(TOPDIR)/rules.mk

PKG_NAME:=uwsd

PKG_SOURCE_URL=https://github.com/jow-/uwsd.git
PKG_MIRROR_HASH:=88396bfe4fcaed1916684731186d6e544f398fd1d469e8e4a1403ae6b18648d3
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-08-12
PKG_SOURCE_VERSION:=5c0402fb3b4a4f583b43e3979429dfce3f170728

PKG_MAINTAINER:=Jo-Philipp Wich <jo@mein.io>
PKG_LICENSE:=ISC

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/uwsd
  SECTION:=net
  CATEGORY:=Network
  TITLE:=uwsd
  DEPENDS:=+libubox +ucode +libopenssl
endef

define Package/uwsc
  SECTION:=net
  CATEGORY:=Network
  TITLE:=uwsc client library
  DEPENDS:=+libubox +ucode +libopenssl
endef

CMAKE_OPTIONS+=-DDEBUG=YES -DCMAKE_BUILD_TYPE=Debug
EXTRA_CFLAGS=-Wno-error=unused-function

define Package/uwsd/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/uwsd $(1)/usr/sbin/
endef

define Package/uwsc/install
	$(INSTALL_DIR) $(1)/usr/lib/ucode
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/uwsc.so $(1)/usr/lib/ucode/
endef

$(eval $(call BuildPackage,uwsd))
$(eval $(call BuildPackage,uwsc))
