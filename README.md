Jetchisel
=========

systemd-qserv


# ========================================================================================== #
#                                                                                            #
#                                  >>> README <<<                                            #
#                                                                                            #
# ========================================================================================== #
#  NOTE                                                                                      #
#                                                                                            #
#    Phpvirtualbox shift's with a vboxinit script which is based on vboxtool.                #
#    This is the rewritten/modified version for  OpenSuSE using systemd.                     #
#    This works with  phpVirtualBox.                                                         #
#                                                                                            #
# ****************************************************************************************** #
#                                                                                            # 
#                                                                                            #
#  Put this script in /usr/lib/systemd/ and name it "systemd-vboxinit"                       #
#                                                                                            #
#  Set permission:                                                                           #
#                                                                                            #
#  chmod ug+rx /usr/lib/systemd/systemd-vboxinit                                             #
#  chgrp vboxusers /usr/lib/systemd/systemd-vboxinit                                         #
#                                                                                            #
#  For testing the script:                                                                   #
#                                                                                            #
#   /usr/lib/systemd/systemd-vboxinit {stop|start}                                           #
#                                                                                            #
# ****************************************************************************************** #
#  Create an file called virtualbox in /etc/default (replace username)                       #
#  and put something like this                                                               # 
#                                                                                            #
#  VBOXWEB_USER=username                                                                     #
#  VBOXAUTOSTART_DB=/etc/vbox                                                                #
#  VBOXAUTOSTART_CONFIG=/etc/vbox/autostart.cfg                                              #
#                                                                                            #
# ****************************************************************************************** #
#  Create a directory /etc/vbox and a file named autostart.cfg inside that directory         #
#  and put something like this.(replace username)                                            #
#                                                                                            #
#                                                                                            #
#  default_policy = deny                                                                     #
#                                                                                            #
#  username  = {                                                                             #
#      allow = true                                                                          #
#  }                                                                                         #
#                                                                                            #
# ****************************************************************************************** #
#  Set the autostart path using VBoxManage as a normal user that belongs to the              #
#  vboxusers group.                                                                          #
#                                                                                            #
#  VBoxManage setproperty autostartdbpath /etc/vbox                                          #
#                                                                                            #
# ****************************************************************************************** #
#  Configure phpvirtualbox's php.ini                                                         #
#                                                                                            #
#  Uncomment "var $startStopConfig = true;"                                                  #
#                                                                                            #
#  Configure vms in phpvirtualbox's graphical menu:                                          #
#                                                                                            #
#  Settings --> General --> Basic --> "StartupMode --> Automatic"                            #
#                                                                                            # 
# ****************************************************************************************** #
#  Create an file called "vboxvmservice.service" (or any name that suits you)                #
#  inside /usr/lib/systemd/system and add the entry below. Replace "username"                # 
#  with the user that belongs to the vboxusers group.                                        # 
#                                                                                            #
#  [Unit]                                                                                    #
#  Description=VBox Virtual Machine  Service                                                 #
#  Requires=systemd-modules-load.service                                                     #
#  After=systemd-modules-load.service                                                        #
#  Documentation=http://jason.ferrer.com.ph/2013/09/phpvirtualbox-on-opensuse-reloaded.html  #
#                                                                                            #
#  [Service]                                                                                 #
#  Type=oneshot                                                                              #
#  User=username                                                                             #
#  Group=vboxusers                                                                           #
#  StandardOutput=syslog                                                                     #
#  EnvironmentFile=/etc/default/virtualbox                                                   #
#  ExecStart=/usr/lib/systemd/systemd-vboxinit start                                         #
#  ExecStop=/usr/lib/systemd/systemd-vboxinit stop                                           #
#  RemainAfterExit=yes                                                                       #
#                                                                                            #
#  [Install]                                                                                 #
#  WantedBy=multi-user.target                                                                #
#                                                                                            #
# ****************************************************************************************** # 
#  To enable that service at boot.                                                           #
#                                                                                            #
#  systemctl enable vboxvmservice.service                                                    #
#                                                                                            #
#  NOTE: replace "syslog" with "tty" in StandardOutput to check the script if it is          #
#        indeed working during startup and shutdown of the host.                             #
#                                                                                            #
#  To check logs when "StandardOutput=syslog" is set:                                        #
#                                                                                            #
#  journalctl _SYSTEMD_UNIT=vboxvmservice.service                                            #
#                                                                                            #
#  To check status:                                                                          #
#                                                                                            #
#   systemctl status vboxvmservice.service                                                   #
#                                                                                            #
# ========================================================================================== #
#                                                                                            #
#                               >>> END OF README <<<                                        #
#                                                                                            #
# ========================================================================================== #
