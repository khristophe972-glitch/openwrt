include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=nft-fullcone
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/sbwml/nft-fullcone.git
PKG_SOURCE_DATE:=2024-01-01
PKG_SOURCE_VERSION:=master
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

define Build/Compile
	cd "$(PKG_BUILD_DIR)/src" && \
	sed -i 's/prandom_u32/get_random_u32/g' nf_nat_fullcone.c && \
	sed -i 's/const struct nft_data \*\*data)/)/' nft_ext_fullcone.c
	+$(MAKE) $(PKG_JOBS) $(KERNEL_MAKEOPTS) \
		M="$(PKG_BUILD_DIR)/src" \
		EXTRA_CFLAGS="$(BUILDFLAGS)" \
		modules
endef

$(eval $(call KernelPackage,nft-fullcone))
