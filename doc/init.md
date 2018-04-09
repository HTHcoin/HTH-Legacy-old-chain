Sample init scripts and service configuration for protond
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/protond.service:    systemd service unit configuration
    contrib/init/protond.openrc:     OpenRC compatible SysV style init script
    contrib/init/protond.openrcconf: OpenRC conf.d file
    contrib/init/protond.conf:       Upstart service configuration file
    contrib/init/protond.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "protoncore" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes protond will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, protond requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, protond will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that protond and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If protond is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running protond without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/proton.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/protond`  
Configuration file:  `/etc/protoncore/proton.conf`  
Data directory:      `/var/lib/protond`  
PID file:            `/var/run/protond/protond.pid` (OpenRC and Upstart) or `/var/lib/protond/protond.pid` (systemd)  
Lock file:           `/var/lock/subsys/protond` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the protoncore user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
protoncore user and group.  Access to proton-cli and other protond rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/protond`  
Configuration file:  `~/Library/Application Support/ProtonCore/proton.conf`  
Data directory:      `~/Library/Application Support/ProtonCore`
Lock file:           `~/Library/Application Support/ProtonCore/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start protond` and to enable for system startup run
`systemctl enable protond`

4b) OpenRC

Rename protond.openrc to protond and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/protond start` and configure it to run on startup with
`rc-update add protond`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop protond.conf in /etc/init.  Test by running `service protond start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy protond.init to /etc/init.d/protond. Test by running `service protond start`.

Using this script, you can adjust the path and flags to the protond program by
setting the PROTOND and FLAGS environment variables in the file
/etc/sysconfig/protond. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy org.proton.protond.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.proton.protond.plist`.

This Launch Agent will cause protond to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run protond as the current user.
You will need to modify org.proton.protond.plist if you intend to use it as a
Launch Daemon with a dedicated protoncore user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
