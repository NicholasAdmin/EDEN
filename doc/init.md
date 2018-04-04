Sample init scripts and service configuration for edend
==========================================================

Sample scripts and configuration files for systemd, Upstart and OpenRC
can be found in the contrib/init folder.

    contrib/init/edend.service:    systemd service unit configuration
    contrib/init/edend.openrc:     OpenRC compatible SysV style init script
    contrib/init/edend.openrcconf: OpenRC conf.d file
    contrib/init/edend.conf:       Upstart service configuration file
    contrib/init/edend.init:       CentOS compatible SysV style init script

1. Service User
---------------------------------

All three Linux startup configurations assume the existence of a "eden" user
and group.  They must be created before attempting to use these scripts.
The OS X configuration assumes edend will be set up for the current user.

2. Configuration
---------------------------------

At a bare minimum, edend requires that the rpcpassword setting be set
when running as a daemon.  If the configuration file does not exist or this
setting is not set, edend will shutdown promptly after startup.

This password does not have to be remembered or typed as it is mostly used
as a fixed token that edend and client programs read from the configuration
file, however it is recommended that a strong and secure password be used
as this password is security critical to securing the wallet should the
wallet be enabled.

If edend is run with the "-server" flag (set by default), and no rpcpassword is set,
it will use a special cookie file for authentication. The cookie is generated with random
content when the daemon starts, and deleted when it exits. Read access to this file
controls who can access it through RPC.

By default the cookie is stored in the data directory, but it's location can be overridden
with the option '-rpccookiefile'.

This allows for running edend without having to do any manual configuration.

`conf`, `pid`, and `wallet` accept relative paths which are interpreted as
relative to the data directory. `wallet` *only* supports relative paths.

For an example configuration file that describes the configuration settings,
see `contrib/debian/examples/eden.conf`.

3. Paths
---------------------------------

3a) Linux

All three configurations assume several paths that might need to be adjusted.

Binary:              `/usr/bin/edend`  
Configuration file:  `/etc/eden/eden.conf`  
Data directory:      `/var/lib/edend`  
PID file:            `/var/run/edend/edend.pid` (OpenRC and Upstart) or `/var/lib/edend/edend.pid` (systemd)  
Lock file:           `/var/lock/subsys/edend` (CentOS)  

The configuration file, PID directory (if applicable) and data directory
should all be owned by the eden user and group.  It is advised for security
reasons to make the configuration file and data directory only readable by the
eden user and group.  Access to eden-cli and other edend rpc clients
can then be controlled by group membership.

3b) Mac OS X

Binary:              `/usr/local/bin/edend`  
Configuration file:  `~/Library/Application Support/EdenCore/eden.conf`  
Data directory:      `~/Library/Application Support/EdenCore`
Lock file:           `~/Library/Application Support/EdenCore/.lock`

4. Installing Service Configuration
-----------------------------------

4a) systemd

Installing this .service file consists of just copying it to
/usr/lib/systemd/system directory, followed by the command
`systemctl daemon-reload` in order to update running systemd configuration.

To test, run `systemctl start edend` and to enable for system startup run
`systemctl enable edend`

4b) OpenRC

Rename edend.openrc to edend and drop it in /etc/init.d.  Double
check ownership and permissions and make it executable.  Test it with
`/etc/init.d/edend start` and configure it to run on startup with
`rc-update add edend`

4c) Upstart (for Debian/Ubuntu based distributions)

Drop edend.conf in /etc/init.  Test by running `service edend start`
it will automatically start on reboot.

NOTE: This script is incompatible with CentOS 5 and Amazon Linux 2014 as they
use old versions of Upstart and do not supply the start-stop-daemon utility.

4d) CentOS

Copy edend.init to /etc/init.d/edend. Test by running `service edend start`.

Using this script, you can adjust the path and flags to the edend program by
setting the EDEND and FLAGS environment variables in the file
/etc/sysconfig/edend. You can also use the DAEMONOPTS environment variable here.

4e) Mac OS X

Copy org.eden.edend.plist into ~/Library/LaunchAgents. Load the launch agent by
running `launchctl load ~/Library/LaunchAgents/org.eden.edend.plist`.

This Launch Agent will cause edend to start whenever the user logs in.

NOTE: This approach is intended for those wanting to run edend as the current user.
You will need to modify org.eden.edend.plist if you intend to use it as a
Launch Daemon with a dedicated eden user.

5. Auto-respawn
-----------------------------------

Auto respawning is currently only configured for Upstart and systemd.
Reasonable defaults have been chosen but YMMV.
