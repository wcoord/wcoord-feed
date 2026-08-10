include $(TOPDIR)/rules.mk

PKG_NAME:=uconfig

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://git.informatics.coop/wcoord/uconfig.git
PKG_SOURCE_VERSION:=3866623ab43ff082c1d3b884c779bc5a023e4aeb

PKG_MAINTAINER:=John Crispin <john@phrozen.org>
PKG_LICENSE:=ISC

PKG_BUILD_DEPENDS:=python3/host
PYTHON3_PKG_BUILD:=0
HOST_PYTHON3_PACKAGE_BUILD_DEPENDS:=./host-pip-requirements.txt

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_SOURCE_DIR:=$(PKG_BUILD_DIR)/src
CMAKE_BINARY_DIR:=$(PKG_BUILD_DIR)/src

define uconfig/module
	$(INSTALL_DIR) $(1)/etc/uconfig/modules/
	touch $(1)/etc/uconfig/modules/$(2)
endef

define Package/uconfig
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=uConfig
  TITLE:=UCI renderer
  DEPENDS=+ucode-mod-nl80211 +ucode-mod-rtnl +ucode-mod-ubus \
	  +ip-full +ip-bridge +ratelimit +pkgen +nlbwmon \
	  +wifi-dynamic +cli +wifi-scripts
endef

define Package/uconfig-mod-ucoord
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=uConfig
  TITLE:=UCI renderer (ucoord plugin)
  DEPENDS=uconfig +unetd +unet-cli +unetmsg
endef

define Package/uconfig-mod-state
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=uConfig
  TITLE:=UCI renderer (state/discovery daemon)
  DEPENDS=uconfig +ucode-mod-rtnl +ucode-mod-nl80211 +ucode-mod-fs +nlbwmon
endef

define Package/uconfig-mod-ui
  SECTION:=utils
  CATEGORY:=Utilities
  SUBMENU:=uConfig
  TITLE:=UCI renderer (webui plugin)
  DEPENDS=uconfig +uconfig-mod-state +uwsd +ucode-mod-math
endef

define Build/Configure
        $(call Py3Build/InstallBuildDepends)
	$(call Build/Configure/Default)
endef

define Build/Compile
	$(PKG_BUILD_DIR)/generate.sh $(PKG_BUILD_DIR)/
endef

define Package/uconfig/conffiles
/etc/uconfig/configs/
/etc/uconfig/certificates/
/etc/uconfig/data/
/etc/uconfig/webui/settings
/etc/uconfig/webui/credentials
endef

define Package/uconfig-mod-state/conffiles
/etc/uconfig/devices/
endef

define Package/uconfig/install
	$(INSTALL_DIR) $(1)/usr/share/ucode/ureader $(1)/etc/uconfig/configs $(1)/etc/uconfig/data
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/generated/schema.uc $(1)/usr/share/ucode/ureader/
	$(CP) $(PKG_BUILD_DIR)/files/* $(1)/
	date +%s > $(1)/etc/uconfig/build_time
endef

define Package/uconfig-mod-state/install
	$(CP) $(PKG_BUILD_DIR)/modules/state/* $(1)/
endef

define Package/uconfig-mod-ucoord/install
	$(CP) $(PKG_BUILD_DIR)/modules/ucoord/* $(1)/
	$(INSTALL_DIR) $(1)/etc/uconfig/data
	$(call uconfig/module,$1,ucoord)
endef

define Package/uconfig-mod-ui/install
	$(CP) $(PKG_BUILD_DIR)/modules/webui/* $(1)/
	$(INSTALL_DIR) $(1)/usr/lib/ucode/uconfig
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/src/utils.so $(1)/usr/lib/ucode/uconfig
endef

$(eval $(call BuildPackage,uconfig))
$(eval $(call BuildPackage,uconfig-mod-state))
$(eval $(call BuildPackage,uconfig-mod-ucoord))
$(eval $(call BuildPackage,uconfig-mod-ui))
