include $(TOPDIR)/rules.mk

PKG_NAME:=quiche
PKG_VERSION:=0.20.0
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/cloudflare/quiche/archive/refs/tags/$(PKG_VERSION).tar.gz?
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_HASH:=7125bc82ddcf38fbfbc69882ccb2723bfb4d5bfeb42718b8291d26ec06042e38

PKG_MAINTAINER:=Remi Gacogne <remi.gacogne@powerdns.com>
PKG_LICENSE:=BSD-2-Clause
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=rust/host

include $(INCLUDE_DIR)/package.mk
include ../../lang/rust/rust-package.mk

RUST_PKG_FEATURES:=ffi,boringssl-boring-crate

define Package/quiche
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Cloudflare Quiche
  URL:=https://github.com/cloudflare/quiche
  DEPENDS:=$(RUST_ARCH_DEPENDS)
endef

define Build/Compile
	+$(CARGO_PKG_VARS) \
	cargo build --release \
		--profile $(CARGO_PKG_PROFILE) \
		--no-default-features --features ffi,pkg-config-meta,boringssl-boring-crate \
		-C "$(PKG_BUILD_DIR)" \
		$(if $(filter --jobserver%,$(PKG_JOBS)),,-j1) \
		--package quiche
	find "$(PKG_BUILD_DIR)"
endef

define Package/quiche/install
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/target/release/libquiche.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_BUILD_DIR)/target/release/quiche.pc $(1)/usr/lib/pkgconfig
endef

#Build/Compile=$(call Build/Compile/Cargo,quiche/,--no-default-features)
$(eval $(call BuildPackage,quiche))
