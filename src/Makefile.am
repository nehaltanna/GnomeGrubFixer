## Process this file with automake to produce Makefile.in

## Created by Anjuta


uidir = $(datadir)/sniper_grubmanager/ui
ui_DATA = sniper_grubmanager.ui


AM_CPPFLAGS = \
	-DPACKAGE_LOCALE_DIR=\""$(prefix)/$(DATADIRNAME)/locale"\" \
	-DPACKAGE_SRC_DIR=\""$(srcdir)"\" \
	-DPACKAGE_DATA_DIR=\""$(datadir)"\" \
	$(SNIPER_GRUBMANAGER_CFLAGS)

AM_CFLAGS =\
	 -Wall\
	 -g

bin_PROGRAMS = sniper_grubmanager

sniper_grubmanager_SOURCES = \
	main.c

sniper_grubmanager_LDFLAGS = \
	-Wl,--export-dynamic

sniper_grubmanager_LDADD = $(SNIPER_GRUBMANAGER_LIBS)


EXTRA_DIST = $(ui_DATA)

# Remove ui directory on uninstall
uninstall-local:
	-rm -r $(uidir)
	-rm -r $(datadir)/sniper_grubmanager
