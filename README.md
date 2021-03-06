This repository includes two helpers for administering runit services

[full documentation](https://github.com/rubyists/sv-helper/wiki)

### sv-helper.sh

sv-helper.sh provides the following sv/runit helpers:

    * sv-enable <service> - Enable a service and start it (will restart on boots)
    * sv-disable <service> - Disable a service from starting (also stop the service)
    * svls [<service>] - Show list of services (Default: all services, pass a service name to see just one)
    * sv-find <service> - Find a service, if it exists
    * sv-list - List available services
    * sv-start <service> - Start a service
    * sv-stop <service> - Stop a service
    * sv-restart <service> - Restart a service
    * make-links) sv-helper make-links Create symlinks for the commands

### rsvlog.sh

rsvlog.sh is meant to be symlinked in a <service>/log directory
          as the 'run' script.

example use of rsvlog.sh

    mkdir -p /etc/sv/aservice/log
    ln -s /usr/bin/rsvlog /etc/sv/aservice/log/run
    ...
  Create the /etc/aservice/run script, and symlink to your $SVDIR. 

in /etc/sv/aservice/log/ you may drop a file named 'conf',
which can modify the log service

    # example conf for syslog logging
    SV_LOG_SYSLOG=true 
    SV_LOG_SYSLOG_PRIORITY=local7.info

    # example conf for svlogd logging, logs go to /var/log/aservice/service_logs in this case
    USERGROUP=log:adm
    LOGDIR=aservice/service_logs
    CURRENT_LOG_FILE=aservice.log

* `SV_LOG_SYSLOG` if set, will log to syslog, ignoring all other options except for `SV_LOG_SYSLOG_PRIORITY`
* `SV_LOG_SYSLOG_PRIORITY` - The priority to use, in facility.level format.  Defaults to daemon.info
* `USERGROUP` is the user and group which the log service runs as
          (default is log:adm)

* `LOGDIR` is the relative directory (under /var/log/<servicename>) where the 
       service will log. (default is /var/log/<servicename>)
       
* `CURRENT_LOG_FILE` is the filename for the link to the 'current' logfile.
                   (default is 'current')
