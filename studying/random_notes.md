#Random Study Notes. Mainly Linus

##What's the difference between curl and wget

http://unix.stackexchange.com/questions/47434/what-is-the-difference-between-curl-and-wget

The main differences are:

wget's major strong side compared to curl is its ability to download recursively.
wget is command line only. There's no lib or anything, but curl features and is powered by libcurl.
curl supports FTP, FTPS, HTTP, HTTPS, SCP, SFTP, TFTP, TELNET, DICT, LDAP, LDAPS, FILE, POP3, IMAP, SMTP, RTMP and RTSP. wget supports HTTP, HTTPS and FTP.
curl builds and runs on more platforms than wget.
wget is part of the GNU project and all copyrights are assigned to FSF. The curl project is entirely stand-alone and independent with no organization parenting at all
curl offers upload and sending capabilities. wget only offers plain HTTP POST support.
You can see more details at the following link:

##What does sudo service do?

http://askubuntu.com/questions/228288/what-does-sudo-service-do

By inspecting the init file /etc/init.d/sudo that 'starts the service', you can easily see what it's doing:
```
case "$1" in
  start)
        # make sure privileges don't persist across reboots
        if [ -d /var/lib/sudo ]
        then
                find /var/lib/sudo -exec touch -t 198501010000 '{}' \;
        fi
        ;;
  stop|reload|restart|force-reload)
        ;;
  *)
        echo "Usage: $N {start|stop|restart|force-reload}" >&2
        exit 1
        ;;
esac
```
So, basically, it just touches some files in /var/lib/sudo on start of the system to have it a very old modification timestamp. As a result, the 'cached' granted authentication actions are revoked on the start of the service (which happens at boot).

Some more detail on the /var/lib/sudo directory and those time stamps, please? Well, from the mapage of sudo(8):

[...]
Once a user has been authenticated, a time stamp is updated and the
user may then use sudo without a password for a short period of time
(15 minutes unless overridden in sudoers).
[...]
Since time stamp files live in the file system, they can outlive a
user's login session.  As a result, a user may be able to login, run a
command with sudo after authenticating, logout, login again, and run
sudo without authenticating so long as the time stamp file's
modification time is within 15 minutes (or whatever the timeout is set
to in sudoers).
[...]
/var/lib/sudo           Directory containing time stamps
