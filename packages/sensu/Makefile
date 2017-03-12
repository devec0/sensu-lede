include $(TOPDIR)/rules.mk

RUBY_PKG_LIBVER:=2.2
RUBY_PKG_NAME:=ruby
RUBY_PKG_VERSION:=2.2.4

PKG_NAME:=sensu
PKG_SHORT_NAME:=$(PKG_NAME)
PKG_RELEASE:=0.26
PKG_LIBVER:=0.26.5
PKG_VERSION:=$(PKG_LIBVER)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=git://github.com/sensu/sensu.git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_REV:=450a8967cf928468b8a2057cecdd2dc938caccc6

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_BUILD_DEPENDS:=ruby/host
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/sensu
	SUBMENU:=Monitoring
	SECTION:=net
	CATEGORY:=Network
	DEPENDS:=ruby +ruby-rbconfig +ruby-ffi +ruby-eventmachine +ruby-enc-extra +sensu-json +sensu-logger +sensu-settings +sensu-extension +sensu-extensions +sensu-transport +sensu-spawn +sensu-redis +ruby-parse-cron
	TITLE:=Sensu Monitoring Framework
endef

RUBY:=$(STAGING_DIR_HOST)/bin/ruby --disable=gems \
	-I$(BUILD_DIR)/$(RUBY_PKG_NAME)-$(RUBY_PKG_VERSION) \
	-r$(ARCH)-linux-gnu-fake

define Package/sensu/description
	This package contains the sensu monitoring framework.
endef

define Build/Install
	$(CP) $(PKG_BUILD_DIR)/lib/* $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/
	$(CP) $(PKG_BUILD_DIR)/lib/sensu $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/
	$(CP) $(PKG_BUILD_DIR)/exe/* $(PKG_INSTALL_DIR)/usr/bin/
endef

define Package/sensu/install
	( cd $(PKG_INSTALL_DIR); $(TAR) -cf - \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/sensu.rb \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/sensu \
	  usr/bin/sensu* \
	) | ( cd $(1); $(TAR) -xf - )
endef

$(eval $(call BuildPackage,sensu))
