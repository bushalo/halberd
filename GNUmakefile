# $Id: GNUmakefile,v 1.8 2004/02/26 04:15:03 rwx Exp $

# ============================================================================
# This makefile is intended for developers. End users should rely on setup.py.
# ============================================================================

# Copyright (C) 2004 Juan M. Bello Rivas <rwx@synnergy.net>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA  02111-1307  USA


srcdir := .
hlbddir := $(srcdir)/hlbd
docdir := $(srcdir)/doc/api
testdir := $(srcdir)/tests


PYTHON := /usr/local/bin/python
PYTHON_COUNT := /usr/local/bin/python_count
EPYDOC := /usr/local/bin/epydoc
CTAGS := /usr/local/bin/ctags
CVS2CL := /usr/local/bin/cvs2cl.pl
SHTOOLIZE := /usr/local/bin/shtoolize
SHTOOL := $(srcdir)/shtool
RM := /bin/rm -f


versionfile := $(hlbddir)/version.py

SCRIPTS := halberd.py bulkscan.py
MODULES := $(filter-out $(hlbddir)/version.py, $(wildcard $(hlbddir)/*.py)) \
		   $(wildcard $(hlbddir)/clues/*.py)

SOURCES := $(SCRIPTS) $(MODULES)
TEST_SOURCES := $(wildcard $(testdir)/*.py)
ALL_SOURCES := $(SOURCES) $(TEST_SOURCES)

ALL_DIRS := $(sort $(dir $(ALL_SOURCES)))


clean:
	$(RM) tags
	$(RM) -r $(srcdir)/build
	$(RM) $(addsuffix *.pyc, $(ALL_DIRS))
	$(RM) $(addsuffix *.pyo, $(ALL_DIRS))

clobber:
	$(RM) *.bak
	$(RM) $(addsuffix *~, $(ALL_DIRS))

build: $(SOURCES)
	$(PYTHON) setup.py build

dist: setversion distclean doc ChangeLog
	$(PYTHON) setup.py sdist

check: $(ALL_SOURCES)
	$(PYTHON) setup.py test
	$(PYTHON) $(hlbddir)/clues/analysis.py

install: build
	$(PYTHON) setup.py install --root $(srcdir)/tmp

distclean: clobber clean
	$(RM) $(srcdir)/MANIFEST
	$(RM) $(srcdir)/ChangeLog
	$(RM) -r $(docdir) $(srcdir)/dist

doc: $(MODULES)
	$(EPYDOC) -o $(docdir) $^

tags: $(ALL_SOURCES)
	$(CTAGS) $^

setversion: shtool
	@version=`$(SHTOOL) version -l python $(versionfile)`; \
	$(SHTOOL) version -l python -n halberd -s $$version $(versionfile)

incversion: shtool
	$(SHTOOL) version -l python -n halberd -i l $(versionfile)

shtool:
	$(SHTOOLIZE) -o $@ version

ChangeLog: $(ALL_SOURCES)
	$(CVS2CL)

count: $(ALL_SOURCES)
	@$(PYTHON_COUNT) $^


.PHONY: clean dist distclean clobber check setversion incversion doc count


# vim: noexpandtab
