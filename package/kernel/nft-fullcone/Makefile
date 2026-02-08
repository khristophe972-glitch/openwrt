include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=nft-fullcone
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/fullcone-nat-nftables/nft-fullcone.git
PKG_SOURCE_DATE:=2023-01-01
PKG_SOURCE_VERSION:=HEAD
PKG_MIRROR_HASH:=skip

include $(INCLUDE_DIR)/package.mk

define KernelPackage/nft-fullcone
  SUBMENU:=Netfilter Extensions
  DEPENDS:=+kmod-nft-nat +kmod-nf-conntrack
  TITLE:=nftables fullcone expression support
  FILES:=$(PKG_BUILD_DIR)/src/nft_fullcone.ko
  KCONFIG:= \
	CONFIG_NF_NAT=y \
	CONFIG_NF_CONNTRACK_EVENTS=y \
	CONFIG_NF_CONNTRACK_CHAIN_EVENTS=y
  AUTOLOAD:=$(call AutoProbe,nft_fullcone)
endef

define KernelPackage/nft-fullcone/description
  nftables fullcone expression kernel module for OpenWrt.
  RFC3489-compatible full cone SNAT for UDP traffic.
endef

define Build/Compile
	+$(MAKE) $(PKG_JOBS) $(KERNEL_MAKEOPTS) \
		M="$(PKG_BUILD_DIR)/src" \
		EXTRA_CFLAGS="$(BUILDFLAGS)" \
		modules
endef

$(eval $(call KernelPackage,nft-fullcone))
