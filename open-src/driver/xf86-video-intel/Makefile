###############################################################################
#
# xf86-video-intel Makefile
#
# Copyright (c) 2006, 2016, Oracle and/or its affiliates. All rights reserved.
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice (including the next
# paragraph) shall be included in all copies or substantial portions of the
# Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.
#
#

# Package name used in tarballs
MODULE_NAME=xf86-video-intel

# Version number (used in path names)
MODULE_VERSION=2.99.917

# Checksums for upstream tarball
TARBALL_MD5  = fa196a66e52c0c624fe5d350af7a5e7b
TARBALL_SHA1 = 9af9ded7a29026c211e5eb50a547e3e33976301d
TARBALL_SHA256 = 00b781eea055582820a123c47b62411bdf6aabf4f03dc0568faec55faf9667c9

# Patches to apply to source after unpacking, in order
SOURCE_PATCHES=solaris-port.patch sna.patch

# Man pages to apply Sun footer to & attributes to list
SUNTOUCHED_MANPAGES=man/*.man
MODULE_STABILITY=Volatile

# Need to build 32-bit libXvMC libraries even when X server is 64-bit only
BUILD_TYPES=32 64
BUILD_TYPES_SET=yes

# but we only want those libraries built in the 32-bit build, not the whole
# driver (which would overwrite the 64-bit driver)
BUILD_TARGETS = build-in-subdir $(MODTYPE_ADD_BUILD_TARGETS)
BUILD_TARGETS_32 = build_legacy_xvmc
BUILD_TARGETS += $(BUILD_TARGETS_$(BUILD_TYPE))
BUILD_TARGETS_SET=yes
subdir_32 = $(BUILD_DIR)/xvmc
subdir_64 = $(BUILD_DIR)
subdir = $(subdir_$(BUILD_TYPE))
install_subdir_cmd = install
INSTALL_TARGETS = install-in-subdir $(MODTYPE_ADD_INSTALL_TARGETS)
INSTALL_TARGETS_32 = install_legacy_xvmc
INSTALL_TARGETS_64 = install_docs
INSTALL_TARGETS += $(INSTALL_TARGETS_$(BUILD_TYPE))
INSTALL_TARGETS_SET=yes

# Intel driver doesn't work on some chipsets if built with Sun compiler
MODULE_COMPILER=gcc
MODULE_COMPILER_SET=yes

# Cos of 23185711 - disable parfait temporarily
USE_PARFAIT=no

MODULE_CFLAGS = -DHAVE_DRI1

# Extra flags to pass to configure script
# --libdir is needed to set path for XvMC libraries
MODULE_CONFIG_OPTS=--enable-dri2 --enable-dri3 \
	  	   --libdir='$(MODULE_FULLARCH_LIB_DIR)' \
		   --enable-sna --enable-uxa --with-default-accel='uxa' \
		   --disable-udev

# echo is set to workaround configure/libtool strangeness in 2.15.0 that
# causes ../../libtool: line 791: X--mode=install: command not found errors
MODULE_CONFIG_ENV = echo=/usr/gnu/bin/echo
MODULE_BUILD_ENV += echo=/usr/gnu/bin/echo

# Instead of allowing the default MODTYPE_LD_SHARED_OPTIONS to link everything
# against the Xorg server binary, libraries.patch adds the XORG_EXTERNS_FLAG 
# variable to the link of just intel_drv.so not to the client-side XvMC libs.
XORG_EXTERNS_IN_LD_SHARED_OPTIONS=no
MODULE_BUILD_ENV += XORG_EXTERNS_FLAG='$(XORG_EXTERNS_FLAG)'

# Library paths at build time
MODULE_LD_OPTIONS = \
	-L$(PROTODIR)$(X11_SERVERMODS_DIR) \
	-L$(PROTODIR)$(X11_SERVERLIBS_DIR)$(ARCHLIBSUBDIR) \
	-L$(PROTODIR)$(X11_LIB_DIR)$(ARCHLIBSUBDIR) \
	-ldrm -ldrm_intel

MODULE_LDFLAGS += -R $(MODULE_PREFIX)/lib$(X11_SERVERLIBS_SUBDIR)$(ARCHLIBSUBDIR)

# Need to regenerate configure & Makefile.in's after patching
# configure.ac & Makefile.am's in libraries.patch & build-dri.patch
AUTORECONF=yes

include ../Makefile.inc

install_docs: $(INSTALL_DEPS)
	mkdir -p $(PROTODIR)$(X11_DOC_DIR)
	cp -pf $(SOURCE_DIR)/README $(PROTODIR)$(X11_DOC_DIR)/README.intel

build-in-subdir: $(BUILD_DEPS)
install-in-subdir: $(INSTALL_DEPS)

LEGACY_XVMC_SUBDIR = $(BUILD_DIR)/src/legacy/i810/xvmc

build_legacy_xvmc: $(BUILD_DEPS)
	$(DEFAULT_BUILD_COMMAND:@DIR@=$(LEGACY_XVMC_SUBDIR)) $(subdir_cmd)

install_legacy_xvmc: $(INSTALL_DEPS)
	$(DEFAULT_INSTALL_COMMAND:@DIR@=$(LEGACY_XVMC_SUBDIR)) $(subdir_cmd) \
                $(install_subdir_cmd)
