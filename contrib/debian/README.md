
Debian
====================
This directory contains files used to package protond/proton-qt
for Debian-based Linux systems. If you compile protond/proton-qt yourself, there are some useful files here.

## proton: URI support ##


proton-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install proton-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your proton-qt binary to `/usr/bin`
and the `../../share/pixmaps/proton128.png` to `/usr/share/pixmaps`

proton-qt.protocol (KDE)

