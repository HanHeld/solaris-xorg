###############################################################################
#
# xf86-video-vesa Makefile
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
MODULE_NAME=xf86-video-vesa

# Version number (used in path names)
MODULE_VERSION=2.3.4

# Checksums for upstream tarball
TARBALL_MD5   = a893c37c589f7a31cea929a5d896a0e2
TARBALL_SHA1  = 88e74ccc5c11fdb60539bd474ba978eeb4f28a23
TARBALL_SHA256 = 7bddf4d879dd6f67088ecb203a31e12334aab980174bd0909930a21f32e251c1

# Patches to apply to source after unpacking, in order
SOURCE_PATCHES = vesa_xorg72.patch,-p1

# Man pages to apply Sun footer to & attributes to list
SUNTOUCHED_MANPAGES=man/*.man
MODULE_STABILITY=Volatile

MODULE_COMPILER=gcc
MODULE_COMPILER_SET=yes

# Cos of 23185711 - disable parfait temporarily
USE_PARFAIT=no

# Libraries/modules to link with
MODULE_LD_OPTIONS= \
	-L$(PROTODIR)$(X11_SERVERMODS_DIR) \
	-R$(X11_SERVERMODS_DIR) \
	-lfb -lpciaccess -lshadow -lvbe

include ../Makefile.inc

# Additional files to install
CONFDIR = /usr/lib/X11
CONFFILE = xorg.conf.vesa

install: $(PROTODIR)$(CONFDIR)/$(CONFFILE)

$(PROTODIR)$(CONFDIR)/$(CONFFILE): $(PROTODIR)$(CONFDIR) $(CONFFILE)
	$(INSTALL_SCRIPT) -c -m 0444 $(CONFFILE) $@

$(PROTODIR)$(CONFDIR):
	mkdir -p $@
