** Project Title - Linux Log File Analysis **

Project Description - 
This project demonstrates essential Linux system administration skills by analyzing various system log files to identify security events, system activities, and potential issues. The project covers authentication logs, SSH connections, system startup messages, and disk-related events, providing hands-on experience with log analysis techniques commonly used in monitoring security and system health by summarizing key information from logs. 

How to Install and Run the Project - 

No special software installation required other than Linux environment with access to system logs. All commands used in this project are part of the core Linux utilities that come pre-installed with most distributions. Ensure the user has permissions to read system log files.

1. Open the terminal on a Linux machine.

2. Execute the following commands one by one to extract and save the log information :

   #check which log fies are available
   
    ls -la /var/log/

   #examine authentication logs for failed attempts
 
   sudo grep "Failed password" /var/log/auth.log | tail -10 > ~/failed_logins.txt

   #check SSH connections

   sudo grep "ssh" /var/log/auth.log | grep "Accepted" | tail -5 > ~/ssh_connections.txt

   #find system startup messages

   sudo grep "systemd" /var/log/syslog | grep "startup" | head -5 > ~/startup_messages.txt

   #check disk messages

   sudo dmesg | grep -i "disk" | head -10 > ~/disk_messages.txt

   #count failed login attempts

   sudo grep -c "Failed password" /var/log/auth.log > ~/failed_login_count.txt
   

Commands and their Usage - 

1.    ls -la /var/log/

       This command check and display the available list of log files generated in the system.

2.  sudo grep "Failed password" /var/log/auth.log | tail -10 > ~/failed_logins.txt

      This command analyze failed login attempts and save the last 10 failed login attempts by creating a new file with the name "failed_logins.txt" in the home directory of the current user.

3. sudo grep "ssh" /var/log/auth.log | grep "Accepted" | tail -5 > ~/ssh_connections.txt

    Initially when I executed this command, it generated an empty file with the name "ssh_connections.txt" in home directory.

     Later I entered these commands one on one -
   
     ssh
   
     ssh user@localhost   -  it shows connection refused

     sudo systemctl status ssh   - it shows ssh.service could not be found

     sudo apt install openssh-server   - started installing ssh server packages

     sudo systemctl status ssh  -  shows ssh service status like active (running)

     ssh user@localhost - added localhost to the list of known hosts

    sudo grep "ssh" /var/log/auth.log | grep "Accepted" | tail -5 > ~/ssh_connections.txt  - gives last 5 authentication logs with successful ssh attempts

4. sudo grep "systemd" /var/log/syslog | grep "startup" | head -5 > ~/startup_messages.txt

   Initially this command created an empty "startup_messages.txt" file in home directory.

   Later I entered these commands -

   sudo grep "systemd" /var/log/syslog  - it gives binary file matches

   sudo grep "systemd" /var/log/syslog | grep "startup"  - it gives binary files matches

   sudo journalctl -b | grep -i "startup" | head -5 > ~/startup_messages.txt  - it was able to read structured systemd startup messages which stores system logs in binary format while grep may miss if these startup messages are not forwarded into that file

   So, using sudo journalctl -b | grep -i "startup" | head -5 > ~/startup_messages.txt , able to store first 5 log entires from current boot that mention "startup" by creating "startup_messages.txt" file in home directory.

5.   sudo dmesg | grep -i "disk" | head -10 > ~/disk_messages.txt

      This command gives first 10 disk related kernel messages related to hardware devices and drivers during system boot and runtime by storing inside by creating "disk_messages.txt" file in home directory.

6.  sudo grep -c "Failed password" /var/log/auth.log > ~/failed_login_count.txt

    This command gives count of failed login attempts by creating a file named "failed_login_count.txt" in home directory.


Verification on each command - 

I have verified these 6 commands by retrieving content from the file and checking with the other command. Since few commands/operations gets implemented in between these 6 commands, we need to update the file content everytime and cross verify the data.

-> For failed_logins.txt generated file, I verified by implementing these 2 commands in terminal - 

  sudo grep "Failed password" /var/log/auth.log | tail -10 > ~/failed_logins.txt  - for updating

  cat ~/failed_logins.txt   - retrieving file content

  sudo grep "Failed password" /var/log/auth.log | tail -10  - cross checking with this command

  Both the commands should give same results, then it is verified

-> For ssh_connections.txt generated file, I verified by running these 2 commands

   cat ~/ssh_connections.txt

   sudo grep "ssh" /var/log/auth.log | grep "Accepted" | tail -5

 -> For startup_messages.txt generated file, I verified using these commands

   sudo journalctl -b --no-pager | grep -i "startup" | head -5 > ~/startup_messages.txt

   cat ~/startup_messages.txt

   sudo journalctl -b | grep -i "startup" | head -5

  -> For disk_messages.txt generated file, verified using these 2 commands

  cat ~/disk_messages.txt

  sudo dmesg | grep -i "disk" | head -10

  -> For failed_login_count.txt generated file, verified count using these commands

  sudo grep -c "Failed password" /var/log/auth.log > ~/failed_login_count.txt

  diff -b <(sudo grep -c "Failed password" /var/log/auth.log) ~/failed_login_count.txt

Real-World Applications - 

-> Security Monitoring 

-> System Administration

-> Performance Monitoring


   

  
   
   
  
