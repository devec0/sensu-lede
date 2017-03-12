include $(TOPDIR)/rules.mk

RUBY_PKG_LIBVER:=2.2
RUBY_PKG_NAME:=ruby
RUBY_PKG_VERSION:=2.2.4

PKG_NAME:=sensu-json
PKG_SHORT_NAME:=$(PKG_NAME)
PKG_RELEASE:=2.1
PKG_LIBVER:=2.1.0
PKG_VERSION:=$(PKG_LIBVER)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=git://github.com/sensu/sensu-json.git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_REV:=d14773b5904c74c925bf7e6ffb74e985406364e5

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_BUILD_DEPENDS:=ruby/host
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/sensu-json
	SUBMENU:=Monitoring
	SECTION:=net
	CATEGORY:=Network
	DEPENDS:=ruby +ruby-rbconfig +ruby-oj
	TITLE:=Sensu JSON Library
endef

RUBY:=$(STAGING_DIR_HOST)/bin/ruby --disable=gems \
	-I$(BUILD_DIR)/$(RUBY_PKG_NAME)-$(RUBY_PKG_VERSION) \
	-r$(ARCH)-linux-gnu-fake

define Package/sensu-json/description
	This package contains the sensu monitoring framework's JSON library.
endef

define Build/Install
	$(CP) $(PKG_BUILD_DIR)/lib/sensu/* $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/sensu/
endef

define Package/sensu-json/install
	( cd $(PKG_INSTALL_DIR); $(TAR) -cf - \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/sensu \
	) | ( cd $(1); $(TAR) -xf - )
endef

$(eval $(call BuildPackage,sensu-json))
