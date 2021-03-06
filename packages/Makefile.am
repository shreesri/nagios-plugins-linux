## Process this file with automake to produce Makefile.in

## Copyright (c) 2016 Davide Madrisan <davide.madrisan@gmail.com>
##
## This program is free software: you can redistribute it and/or modify
## it under the terms of the GNU General Public License as published by
## the Free Software Foundation, either version 3 of the License, or
## (at your option) any later version.
##
## This program is distributed in the hope that it will be useful,
## but WITHOUT ANY WARRANTY; without even the implied warranty of
## MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
## GNU General Public License for more details.
##
## You should have received a copy of the GNU General Public License
## along with this program.  If not, see <http://www.gnu.org/licenses/>.

multibuilddir = $(shell pwd)

DOCKER = /usr/bin/docker
SPECFILE = $(PACKAGE).spec
SRC_BZ2 = $(top_srcdir)/$(PACKAGE_TARNAME)-$(PACKAGE_VERSION).tar.bz2

# GID and UID of the user 'developer'
GID ?= "$(shell id -g)"
UID ?= "$(shell id -u)"

MULTIBUILD_SCRIPT = $(multibuilddir)/multibuild.sh

MULTIBUILD_OPTS = \
	--shared $(top_srcdir):/shared:rw \
	--gid $(GID) \
	--uid $(UID) \
	--pckver $(PACKAGE_VERSION)

TARGETS_REDHAT = \
	centos-5 centos-6 centos-7 \
	fedora-24 fedora-25 fedora-26 fedora-rawhide

centos-latest: centos-7
fedora-latest: fedora-26

TARGETS_DEBIAN = debian-squeeze debian-wheezy debian-jessie debian-stretch

debian-6: debian-squeeze
debian-7: debian-wheezy
debian-8: debian-jessie
debian-9: debian-stretch
debian-latest: debian-stretch

$(SPECFILE):
	@cd $(builddir)/specs && $(MAKE) $(AM_MAKEFLAGS) all

$(SRC_BZ2):
	@cd $(top_builddir) && $(MAKE) $(AM_MAKEFLAGS) dist

$(TARGETS_REDHAT): $(SPECFILE) $(SRC_BZ2) $(DOCKER) $(MULTIBUILD_SCRIPT)
	@distr=`echo $@ | sed s/-/:/`; \
	$(SHELL) $(MULTIBUILD_SCRIPT) $(MULTIBUILD_OPTS) \
	   --spec $(multibuilddir)/specs/$(SPECFILE) \
	   --os $$distr --target pcks/$@

$(TARGETS_DEBIAN): $(SRC_XZ) $(DOCKER) $(MULTIBUILD_SCRIPT)
	@distr=`echo $@ | sed s/-/:/`; \
	$(SHELL) $(MULTIBUILD_SCRIPT) $(MULTIBUILD_OPTS) \
	   --os $$distr --target pcks/$@

EXTRA_DIST = docker-shell-helpers multibuild.sh
SUBDIRS = specs
