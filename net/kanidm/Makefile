include $(TOPDIR)/rules.mk

PKG_NAME:=reaction
PKG_VERSION:=1.6.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/kanidm/kanidm.git
PKG_SOURCE_DATE=2025-06-27
PKG_SOURCE_VERSION:=v${PKG_VERSION}
PKG_MIRROR_HASH:=8bbd6325e3687f9daab554a3f9e78947019571490c0131e8174a93519564ac26

PKG_MAINTAINER:=Vladimir Ermakov <vooon341@gmail.com>
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=rust/host
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include ../../lang/rust/rust-package.mk

define Package/kanidm
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=$(RUST_ARCH_DEPENDS)
  TITLE:=A simple, secure, and fast identity management platform (client)
  URL:=https://kanidm.com
endef

define Package/kanidm/description
endef

define Package/kanidm/conffiles
/etc/kanidm/
endef

define Build/Compile
	$(call Build/Compile/Cargo)
endef

define Package/kanidm/install
	$(INSTALL_DIR) $(1)/etc/kanidm
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) $(CURDIR)/files/kanidm.init $(1)/etc/init.d/kanidm
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/kanidm $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib/kanidm
endef

$(eval $(call RustBinPackage,kanidm))
$(eval $(call BuildPackage,kanidm))
