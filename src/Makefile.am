## We require new-style dependency handling.
AUTOMAKE_OPTIONS = 1.7

NULL =

saverdir = $(libexecdir)/light-locker

INCLUDES =							\
	-I.							\
	-I$(srcdir)						\
	$(LIGHT_LOCKER_CFLAGS)					\
	$(LIGHT_LOCKER_DIALOG_CFLAGS)				\
	$(LIGHT_LOCKER_CAPPLET_CFLAGS)				\
	$(DISABLE_DEPRECATED_CFLAGS)				\
	-DPREFIX=\""$(prefix)"\" 				\
	-DBINDIR=\""$(bindir)"\"				\
	-DLIBDIR=\""$(libdir)"\" 				\
	-DLIBEXECDIR=\""$(libexecdir)"\"			\
	-DDATADIR=\""$(datadir)"\"				\
	-DSYSCONFDIR=\""$(sysconfdir)"\" 			\
	-DLOCALEDIR=\""$(datadir)/locale"\"			\
	-DSAVERDIR=\""$(saverdir)"\"				\
	-DGTKBUILDERDIR=\"$(pkgdatadir)\"			\
	-DPAM_SERVICE_NAME=\""light-locker"\"			\
	$(WARN_CFLAGS)						\
	$(AUTH_CFLAGS)						\
	$(DEBUG_CFLAGS)						\
	$(DBUS_CFLAGS)						\
	#$(LIBGNOMEKBDUI_CFLAGS)					\
	$(LIBNOTIFY_CFLAGS)					\
	$(SYSTEMD_CFLAGS)					\
	$(NULL)

bin_PROGRAMS = \
	light-locker		\
	light-locker-command	\
	$(NULL)

libexec_PROGRAMS = \
	light-locker-dialog	\
	$(NULL)

#noinst_PROGRAMS = 	\
#	test-passwd	\
#	test-watcher	\
#	test-window	\
#	$(NULL)

desktopdir = $(datadir)/applications
desktop_in_files = light-locker.desktop.in
desktop_DATA = $(desktop_in_files:.desktop.in=.desktop)
@INTLTOOL_DESKTOP_RULE@

light_locker_command_SOURCES = 	\
	light-locker-command.c	\
	gs-bus.h			\
	$(NULL)

light_locker_command_LDADD =	\
	$(LIGHT_LOCKER_COMMAND_LIBS)	\
	$(NULL)

AUTH_SOURCES =				\
	gs-auth-@AUTH_SCHEME@.c		\
	$(NULL)

#test_passwd_SOURCES = 			\
#	test-passwd.c 			\
#	gs-auth.h			\
#	$(AUTH_SOURCES)			\
#	setuid.c			\
#	setuid.h			\
#	subprocs.c			\
#	subprocs.h			\
#	$(NULL)

#test_passwd_LDADD =			\
#	$(LIGHT_LOCKER_DIALOG_LIBS)	\
#	$(AUTH_LIBS)			\
#	$(NULL)

#test_watcher_SOURCES =			\
#	test-watcher.c			\
#	gs-bus.h			\
#	gs-watcher.h			\
#	gs-watcher-x11.c		\
#	gs-marshal.c 			\
#	gs-marshal.h			\
#	gs-debug.c			\
#	gs-debug.h			\
#	$(NULL)

#test_watcher_LDADD =			\
#	$(LIGTH_LOCKER_LIBS)		\
#	$(SAVER_LIBS)			\
#	$(NULL)

#test_window_SOURCES =			\
#	test-window.c			\
#	gs-window.h			\
#	gs-window-x11.c			\
#	gs-grab-x11.c			\
#	gs-grab.h			\
#	gs-marshal.c 			\
#	gs-marshal.h			\
#	gs-debug.c			\
#	gs-debug.h			\
#	subprocs.c			\
#	subprocs.h			\
#	$(NULL)

#test_window_LDADD =			\
#	$(LIGTH_LOCKER_LIBS)		\
#	$(SAVER_LIBS)			\
#	$(NULL)

light_locker_dialog_SOURCES = 	\
	light-locker-dialog.c	\
	gs-lock-plug.c			\
	gs-lock-plug.h			\
	gs-debug.c			\
	gs-debug.h			\
	setuid.c			\
	setuid.h			\
	subprocs.c			\
	subprocs.h			\
	gs-auth.h			\
	$(AUTH_SOURCES)			\
	$(NULL)

light_locker_dialog_LDADD =	\
	$(LIGHT_LOCKER_DIALOG_LIBS)	\
	$(SAVER_LIBS)			\
	$(AUTH_LIBS)			\
	#$(LIBGNOMEKBDUI_LIBS)		\
	$(LIBNOTIFY_LIBS)		\
	$(NULL)

BUILT_SOURCES = 		\
	gs-marshal.c 		\
	gs-marshal.h		\
	$(NULL)

gs-marshal.c: gs-marshal.list
	echo "#include \"gs-marshal.h\"" > $@ && \
	@GLIB_GENMARSHAL@ $< --prefix=gs_marshal --body >> $@

gs-marshal.h: gs-marshal.list
	@GLIB_GENMARSHAL@ $< --prefix=gs_marshal --header > $@

light_locker_SOURCES =	\
	light-locker.c		\
	light-locker.h		\
	gs-bus.h		\
	gs-monitor.c		\
	gs-monitor.h		\
	gs-watcher-x11.c	\
	gs-watcher.h		\
	gs-listener-dbus.c	\
	gs-listener-dbus.h	\
	gs-manager.c		\
	gs-manager.h		\
	gs-window-x11.c		\
	gs-window.h		\
	gs-prefs.c		\
	gs-prefs.h		\
	gs-debug.c		\
	gs-debug.h		\
	subprocs.c		\
	subprocs.h		\
	gs-grab-x11.c		\
	gs-grab.h		\
	$(BUILT_SOURCES)	\
	$(NULL)

light_locker_LDADD =		\
	$(LIGHT_LOCKER_LIBS)	\
	$(SAVER_LIBS)			\
	$(SYSTEMD_LIBS)                 \
	$(NULL)

light_locker_LDFLAGS = -export-dynamic

EXTRA_DIST =				\
	debug-screensaver.sh		\
	gs-marshal.list			\
	light-locker.desktop.in		\
	$(NULL)

CLEANFILES = 				\
	$(desktop_DATA) 		\
	light-locker.desktop.in		\
	$(BUILT_SOURCES)

MAINTAINERCLEANFILES =			\
	*~				\
	Makefile.in

instags-exec-hook:
	@if [ "x@NEED_SETUID@" = "xyes" ]; then \
	    echo "***" ; \
	    echo "***  Warning: light-locker has been compiled with support for" ; \
	    echo "***           bsdauth(3) and must be instagsed as a setuid root" ; \
	    echo "***           program in order for locking to work.  To do this, you" ; \
	    echo "***           must run:" ; \
	    echo "***" ; \
	    echo "***           chown root $(DESTDIR)$(libexecdir)/light-locker-dialog" ; \
	    echo "***           chmod +s $(DESTDIR)$(libexecdir)/light-locker-dialog" ; \
	    echo "***" ; \
	    echo "***           For now, it wigs be instagsed non-setuid, which" ; \
	    echo "***           means that locking might not work." ; \
	    echo "***" ; \
	fi

-include $(top_srcdir)/git.mk
