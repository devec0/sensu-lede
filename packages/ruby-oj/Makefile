include $(TOPDIR)/rules.mk

RUBY_PKG_LIBVER:=2.2
RUBY_PKG_NAME:=ruby
RUBY_PKG_VERSION:=2.2.4

PKG_NAME:=ruby-oj
PKG_SHORT_NAME:=oj
PKG_RELEASE:=2.18
PKG_LIBVER:=2.18.2
PKG_VERSION:=$(PKG_LIBVER)

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=git://github.com/ohler55/oj.git
PKG_SOURCE_VERSION:=$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_SHORT_NAME)-$(PKG_VERSION)
PKG_SOURCE_PROTO:=git
PKG_REV=fad7c764981a96edf58f2a19ffc0b57760786deb

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_BUILD_DEPENDS:=ruby/host
PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/ruby-oj
  SUBMENU:=Ruby
  SECTION:=lang
  CATEGORY:=Languages
  DEPENDS:=ruby +ruby-rbconfig
  TITLE:=Ruby OJ (Optimized JSON) gem
endef
ifeq ($(ARCH),rlx)
	ARCH:=mips
endif
RUBY:=$(STAGING_DIR_HOST)/bin/ruby --disable=gems \
	-I$(BUILD_DIR)/$(RUBY_PKG_NAME)-$(RUBY_PKG_VERSION) \
	-r$(ARCH)-linux-gnu-fake

define Package/ruby-oj/description
 This package contains the ruby oj gem.
endef

define Build/Configure
	(cd $(PKG_BUILD_DIR)/ext/oj; \
		$(RUBY) extconf.rb; \
	)
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/ext/oj
endef

define Build/Install
	$(INSTALL_DIR) $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/$(ARCH)-linux-gnu
	$(CP) $(PKG_BUILD_DIR)/lib/oj.rb $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/
	$(CP) $(PKG_BUILD_DIR)/lib/oj $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/
	$(CP) $(PKG_BUILD_DIR)/ext/oj/*.so $(PKG_INSTALL_DIR)/usr/lib/ruby/$(RUBY_PKG_LIBVER)/$(ARCH)-linux-gnu/
endef

define Package/ruby-oj/install
	( cd $(PKG_INSTALL_DIR); $(TAR) -cf - \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/oj \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/oj.rb \
		usr/lib/ruby/$(RUBY_PKG_LIBVER)/$(ARCH)-linux-gnu/*.so \
	) | ( cd $(1); $(TAR) -xf - )
endef

$(eval $(call BuildPackage,ruby-oj))
