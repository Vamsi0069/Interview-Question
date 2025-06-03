#### 1. How to find files in Linux?


find /path -name "filename"
Examples:

By name: find . -name "*.log"
By size: find / -size +100M
Recently modified: find /var/log -mtime -1

#### 2. Which Linux distro did you use in your project?

Mostly Amazon Linux 2, Ubuntu, or CentOS depending on the cloud provider and application needs.

#### 3.: How can you set up password-less authentication in Linux using SSH?

1.Generate SSH key
    ssh-keygen
    
2.Copy public key to remote server:
    ssh-copy-id user@remote-server
    
3. Now you can SSH without a password:
    ssh user@remote-server

#### 4. What is the command to check memory in Linux?
free -h

#### 5. What is the command to check file size in Linux?
du -sh filename

#### 6. How can you search for a word in Linux?
grep 'word' filename
To search recursively in all files: grep -r 'word' /path/to/directory

#### 7. How do you troubleshoot if an application is running slow on Linux?
Check CPU/memory usage: top, htop, vmstat
Check disk I/O: iostat, iotop, df -h
Check network issues: netstat, ping, traceroute
Check logs: /var/log/syslog, application-specific logs
Check processes: ps aux --sort=-%mem

#### 8: Which command is used in Linux to check disk usage?

df -h           **Shows disk usage in human-readable format**

du -sh *        **Shows folder sizes in the current directory**

#### 9: How can you delete the contents of a file in Linux without deleting the file itself?

> filename           **Truncates the file**

: > filename         **Same as above**

truncate -s 0 filename  # Explicitly sets file size to 0

#### 10. How can you modify permissions in Linux?

Use chmod:
chmod 755 file.sh
Use chown to change ownership:
 chown user:group file

#### 11. What is crontab, and what are allow/deny files?

•	crontab schedules jobs.

•	/etc/cron.allow – only users in this file can use crontab.

•	/etc/cron.deny – users listed here cannot use crontab.

#### 12. How can you kill a process in Linux?

kill <PID>
kill -9 <PID>       #### force kill

#### 13. How can you check CPU utilization?

Use:
top
htop
mpstat

#### 14. How can you check if a port is open and listening?

netstat -tuln | grep 80
ss -tuln | grep 80

#### 15. How can you check if the network is working using traceroute?

traceroute google.com

It shows the path and delays to the destination.

#### 16. How can you capture last 20 lines of a file?

tail -n 20 filename.log

#### 17: Have you ever come across a situation where a process in Linux is automatically killed?

Yes, this typically happens when the system runs out of memory. The OOM Killer (Out Of Memory Killer) in Linux terminates processes to free up RAM. You can check this using:
dmesg | grep -i "killed process"

#### 18: How can you create user groups in Linux?

#### Create a group
sudo groupadd devteam

#### Add user to group
sudo usermod -aG devteam username

#### Verify
groups username
