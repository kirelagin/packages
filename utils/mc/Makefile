#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mc
PKG_VERSION:=4.8.13
PKG_RELEASE:=1
PKG_MAINTAINER:=Dirk Brenken <dibdot@gmail.com>
PKG_LICENSE:=GPL-3.0+

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://ftp.midnight-commander.org/
PKG_MD5SUM:=d967caa12765eb86e52a6a63ca202500

PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

define Package/mc
  SECTION:=utils
  CATEGORY:=Utilities
  DEPENDS:=+glib2 +libncurses $(LIBRPC_DEPENDS) $(ICONV_DEPENDS)
  TITLE:=Midnight Commander - a powerful visual file manager
  URL:=http://www.midnight-commander.org/
  MENU:=1
endef

define Package/mc/config
	source "$(SOURCE)/Config.in"
endef

define Package/mc/description
 GNU Midnight Commander is a visual file manager, 
 licensed under GNU General Public License and therefore qualifies as Free Software.
 It's a feature rich full-screen text mode application that allows you to copy, 
 move and delete files and whole directory trees, search for files 
 and run commands in the subshell. Internal viewer and editor are included. 
endef

CONFIGURE_ARGS += \
	--enable-utf8 \
	--disable-doxygen-doc \
	--disable-vfs-sftp \
	--with-screen=ncurses \
	--without-gpm-mouse \
	--without-x \
	ac_cv_search_addwstr=no

ifeq ($(CONFIG_MC_DIFF_VIEWER),n)
CONFIGURE_ARGS += \
	--without-diff-viewer
endif

ifeq ($(CONFIG_MC_EDITOR),n)
CONFIGURE_ARGS += \
	--without-edit
endif

ifeq ($(CONFIG_MC_SUBSHELL),n)
CONFIGURE_ARGS += \
	--without-subshell
endif

ifeq ($(CONFIG_MC_DISABLE_VFS),y)
CONFIGURE_ARGS += \
	--without-vfs
endif

define Package/mc/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/mc $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/mc
ifeq ($(CONFIG_MC_DIFF_VIEWER),y)
	ln -sf mc $(1)/usr/bin/mcdiff
endif
ifeq ($(CONFIG_MC_EDITOR),y)
	ln -sf mc $(1)/usr/bin/mcedit
endif
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/misc/mc.menu $(1)/etc/mc
	$(INSTALL_DIR) $(1)/etc/mc/skins
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/misc/skins/default.ini $(1)/etc/mc/skins
	$(INSTALL_DIR) $(1)/root/.mc/cedit/Syntax
endef

define Package/mc/conffiles
/etc/mc/mc.menu
/etc/mc/skins/default.ini
endef

$(eval $(call BuildPackage,mc))
