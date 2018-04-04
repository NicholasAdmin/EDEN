
Debian
====================
This directory contains files used to package edend/eden-qt
for Debian-based Linux systems. If you compile edend/eden-qt yourself, there are some useful files here.

## eden: URI support ##


eden-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install eden-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your eden-qt binary to `/usr/bin`
and the `../../share/pixmaps/eden128.png` to `/usr/share/pixmaps`

eden-qt.protocol (KDE)

