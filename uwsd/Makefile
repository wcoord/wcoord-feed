include $(TOPDIR)/rules.mk

PKG_NAME:=uwsd

PKG_SOURCE_URL=https://github.com/blogic/uwsd.git
PKG_MIRROR_HASH:=6ca94ddd3856c176130504c1dd6f3dfb2e003e51829d5e11cb66ad0ec5399601
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2026-08-10
PKG_SOURCE_VERSION:=5a206e33a5cb60aaaa52930c775d2a0ae207ff2f

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
