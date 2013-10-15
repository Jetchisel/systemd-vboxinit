systemd-vboxinit
=============

Autostart and auto save of your vms during start up and shutdown of your host machine
without doing your favorite sudo or even su just to have your vms autostart and auto save.
Enable the service one time and the vms you can set to auto via phpvirtualbox.
                                          
  
NOTE                                                                                      
                                                                                            
Phpvirtualbox shift's with a vboxinit script which is based on vboxtool.                
This is the rewritten/modified version for  OpenSuSE using systemd.                     
This works with  phpVirtualBox.                                                         
                                                                                            
copy systemd-vboxinit  in /usr/lib/systemd/                                               
                                                                                            
Set permission:                                                                           
                                                                                            
chmod ug+rx /usr/lib/systemd/systemd-vboxinit                                             
chgrp vboxusers /usr/lib/systemd/systemd-vboxinit                                         
                                                                                            
For testing the script:                                                                   
                                                                                            
/usr/lib/systemd/systemd-vboxinit {stop|start}                                           

                                                                                            
Copy the file  "vboxvmservice.service" inside /usr/lib/systemd/system and                  
Replace "username" with the user that belongs to the vboxusers group.                      
The Documentation entry is optional                                                        
                                                                                            
Create an file called virtualbox in /etc/default (replace username)                       
and put something like this                                                                
                                                                                            
VBOXWEB_USER=username                                                                     
                                                                                            

Configure phpvirtualbox's php.ini                                                         
                                                                                            
Uncomment "var $startStopConfig  true;"                                                  
                                                                                            
Configure vms in phpvirtualbox's graphical menu:                                          
                                                                                            
Settings --> General --> Basic --> "StartupMode --> Automatic"                            
                                                                                             
To enable that service at boot.                                                           
                                                                                            
systemctl enable vboxvmservice.service                                                    
                
check some info about the service

journalctl _SYSTEMD_UNIT=vboxvmservice.service                                            
                                                                                            
To check status:                                                                          
                                                                                            
systemctl status vboxvmservice.service                                                   
                                                                                        
