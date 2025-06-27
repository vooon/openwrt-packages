include $(TOPDIR)/rules.mk

PKG_NAME:=kanidm
PKG_VERSION:=1.6.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/kanidm/kanidm.git
PKG_SOURCE_DATE=2025-06-11
PKG_SOURCE_VERSION:=v${PKG_VERSION}
#PKG_MIRROR_HASH:=791fa570faf40909cf8a7aab057f29582b66840a958fd24a747f6ca7d0891f95

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

define Package/kanidm/Common/description
Kanidm is a simple and secure identity management platform, allowing other applications
and services to offload the challenge of authenticating and storing identities to Kanidm.
endef

define Package/kanidm/description
$(call Package/kanidm/Common/description)
endef

define Package/kanidm/conffiles
/etc/kanidm/
endef

# NOTE: fix compact_jwt git clone problem
CARGO_PKG_VARS += \
	CARGO_NET_GIT_FETCH_WITH_CLI=true \
	KANIDM_BUILD_PROFILE=release_linux

# NOTE: fix linker=clang option passed to build, so some builds then fail to find a clang binary
define CARGO_CONFIG_OVERRIDE
  [target.$(RUSTC_TARGET_ARCH)]
  # NOTE: original:
  # linker = "clang"
  # rustflags = ["-C", "link-arg=-fuse-ld=lld", "-Ctarget-cpu=native", "-C", "force-frame-pointers=yes"]
  #linker = "$(TARGET_CC_NOCACHE)"
  #rustflags = ["-C", "link-arg=-fuse-ld=lld", "-C", "force-frame-pointers=yes"]
endef

export CARGO_CONFIG_OVERRIDE

# NOTE: based on Arch https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=kanidm
define Build/Compile

	echo "$$$$CARGO_CONFIG_OVERRIDE" > "$(PKG_BUILD_DIR)/.cargo/config.toml"

	$(call Build/Compile/Cargo,server/daemon)

	# (cd $(PKG_BUILD_DIR); \
	# $(CARGO_PKG_VARS) \
	# KANIDM_BUILD_PROFILE="release_linux" \
	# cargo build -v \
	# 	--profile $(CARGO_PKG_PROFILE) \
	# 	--target-dir release \
	# 	--features unix \
	# 	--package daemon \
	# 	--package kanidm-ipa-sync \
	# 	--package kanidm_tools \
	# 	--package kanidm_unix_int \
	# 	--package nss_kandim \
	# 	--package pam_kandim \
	# )
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
