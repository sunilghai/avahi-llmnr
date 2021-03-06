# $Id$
#
# This file is part of avahi.
# 
# avahi is free software; you can redistribute it and/or modify it
# under the terms of the GNU Lesser General Public License as
# published by the Free Software Foundation; either version 2 of the
# License, or (at your option) any later version.
#
# avahi is distributed in the hope that it will be useful, but WITHOUT
# ANY WARRANTY; without even the implied warranty of MERCHANTABILITY
# or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public
# License for more details.
#
# You should have received a copy of the GNU Lesser General Public
# License along with avahi; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA 02111-1307
# USA.

AM_CFLAGS=-I$(top_srcdir) -DG_DISABLE_DEPRECATED=1 -DGDK_DISABLE_DEPRECATED=1 -DGTK_DISABLE_DEPRECATED=1

# This cool debug trap works on i386/gcc only
AM_CFLAGS+='-DDEBUG_TRAP=__asm__("int $$3")'

desktopdir = $(datadir)/applications
desktop_DATA = 

EXTRA_DIST = bssh.desktop.in bvnc.desktop.in


if HAVE_GTK
if HAVE_DBUS
if HAVE_GLIB

avahiincludedir=$(includedir)/avahi-ui

avahiinclude_HEADERS = \
	avahi-ui.h

lib_LTLIBRARIES = \
	libavahi-ui.la 

libavahi_ui_la_SOURCES = \
	avahi-ui.h avahi-ui.c
libavahi_ui_la_CFLAGS = $(AM_CFLAGS) $(GTK20_CFLAGS)
libavahi_ui_la_LIBADD = $(AM_LDADD) ../avahi-common/libavahi-common.la ../avahi-client/libavahi-client.la ../avahi-glib/libavahi-glib.la $(GTK20_LIBS)
libavahi_ui_la_LDFLAGS = $(AM_LDFLAGS)  -version-info $(LIBAVAHI_UI_VERSION_INFO)

if HAVE_GDBM
libavahi_ui_la_SOURCES += ../avahi-utils/stdb.h ../avahi-utils/stdb.c
libavahi_ui_la_CFLAGS += -DDATABASE_FILE=\"$(pkglibdir)/service-types.db\"
libavahi_ui_la_LIBADD += -lgdbm
endif

if HAVE_DBM
libavahi_ui_la_SOURCES += ../avahi-utils/stdb.h ../avahi-utils/stdb.c
libavahi_ui_la_CFLAGS += -DDATABASE_FILE=\"$(pkglibdir)/service-types.db\"
endif

bin_PROGRAMS = bssh
desktop_DATA += bssh.desktop bvnc.desktop

bssh_SOURCES = bssh.c
bssh_CFLAGS = $(AM_CFLAGS) $(GTK20_CFLAGS)
bssh_LDADD = $(AM_LDADD) $(GTK20_LIBS) ../avahi-client/libavahi-client.la ../avahi-common/libavahi-common.la libavahi-ui.la

install-exec-local:
	cd $(DESTDIR)/$(bindir) && \
		rm -f bvnc bshell && \
		$(LN_S) bssh bvnc && \
		$(LN_S) bssh bshell

bssh.desktop: bssh.desktop.in
	sed -e 's,@bindir\@,$(bindir),g' $< > $@

bvnc.desktop: bvnc.desktop.in
	sed -e 's,@bindir\@,$(bindir),g' $< > $@

endif # HAVE_GLIB
endif
endif

CLEANFILES = $(desktop_DATA)
