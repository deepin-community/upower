## Process this file with automake to produce Makefile.in

ACLOCAL_AMFLAGS = -I m4 ${ACLOCAL_FLAGS}

SUBDIRS = etc dbus libupower-glib src doc tools po

if BACKEND_TYPE_LINUX
SUBDIRS += rules
endif

# Creating ChangeLog from git log (taken from cairo/Makefile.am):
ChangeLog: $(srcdir)/ChangeLog

$(srcdir)/ChangeLog:
	@if test -d "$(srcdir)/.git"; then \
	  (cd "$(srcdir)" && \
	  ./missing --run git-log --stat) | fmt --split-only > $@.tmp \
	  && mv -f $@.tmp $@ \
	  || ($(RM) $@.tmp; \
	      echo Failed to generate ChangeLog, your ChangeLog may be outdated >&2; \
	      (test -f $@ || echo git-log is required to generate this file >> $@)); \
	else \
	  test -f $@ || \
	  (echo A git checkout and git-log is required to generate ChangeLog >&2 && \
	  echo A git checkout and git-log is required to generate this file >> $@); \
	fi

EXTRA_DIST = 				\
	HACKING 			\
	ChangeLog

MAINTAINERCLEANFILES =			\
	ABOUT-NLS		\
	aclocal.m4		\
	build-aux		\
	ChangeLog		\
	config.h.in		\
	gtk-doc.make		\
	po/Makefile.in.in~	\
	po/Makevars.template	\
	po/Rules-quot		\
	po/e*.header		\
	po/*.sin		\
	po/*.sed		\
	*.tar.xz		\
	INSTALL

snapshot:
	$(MAKE) dist distdir=$(PACKAGE)-$(VERSION)-`date +"%Y%m%d"`

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = upower-glib.pc

# xsltproc barfs on 'make distcheck'; disable for now
DISTCHECK_CONFIGURE_FLAGS=--enable-man-pages --enable-gtk-doc			\
	--with-systemdsystemunitdir=$$dc_install_base/$(systemdsystemunitdir)	\
	--with-udevrulesdir='$${libdir}/udev/rules.d-distcheck'			\
	--enable-introspection=yes

clean-local :
	rm -f *~

-include $(top_srcdir)/git.mk
