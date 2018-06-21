
Debian
====================
This directory contains files used to package hthd/hth-qt
for Debian-based Linux systems. If you compile hthd/hth-qt yourself, there are some useful files here.

## hth: URI support ##


hth-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install hth-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your hth-qt binary to `/usr/bin`
and the `../../share/pixmaps/hth128.png` to `/usr/share/pixmaps`

hth-qt.protocol (KDE)

