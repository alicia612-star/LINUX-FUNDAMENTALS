# Linux Fundamentals: Scenarios
This file contains real-world troubleshooting scenarios and break-fix challenges.

**1 Moving a File**
To move a file from its current location to the target directory,
use the mv (move) command:
mv filename.txt /home/ubuntu/projects/


**2. Creating Multiple Folders at Once**
 You can create a nested structure or multiple directories simultaneously using mkdir with braces {}:
   mkdir -p project/{logs,scripts,backups}
  
**3. Locating, Copying, and Deleting a File**
   Locate it: Use find  or locate
   find / -type f -name "large_backup.tar.gz" 2>/dev/null
"Copy it then"
   cp large_backup.tar.gz /path/to/backup_directory/
    ```
*   **Delete the original:** 
    rm large_backup.tar.gz
    

---
 File Viewing & Management Scenarios
**4 Monitoring Logs**
*   **View the latest logs (last 10 lines):** 
tail /var/log/app.log

*   **Continuously monitor updates (live stream):** 
     tail -f /var/log/app.log
    

 **5. Searching for Errors**
Use the `grep` command to filter the file for the specific keyword:
grep "ERROR" /var/log/app.log

6. **Scrolling Through Large Files**
   Use the less command. It allows you to scroll page-by-page (using Spacebar) or line-by-line (using arrow keys) without loading the entire file into memory:
   less large_config.conf
   
**7. Fixing "Permission denied" on a Script**
   The file lacks execution permissions. You need to grant execute rights to the file:
   chmod +x deploy.sh
   ./deploy.sh
   
**8. Strict Owner-Only Permissions**
   The permission number is 700.
   7 (4+2+1) = Owner can Read, Write, and Execute.
   0 = Group has no permissions.
   0 = Others have no permissions.
   chmod 700 filename
   
**9. Changing Ownership**
   Use the chown (change owner) command, usually requiring sudo elevation:
   sudo chown ubuntu filename
   
10. **The Danger of "Global Write Access" (chmod 777 / World-Writable**
    )**Security Risk**: Giving everyone write access allows any local user, guest account, or compromised service/malware running on the system to alter, malicious code-inject, truncate, or entirely delete the file. If it's a system or configuration file, it can lead to full privilege escalation or server compromise.
    
    **11. Identifying High CPU Processe**s
    You can use real-time interactive monitors or a snapshot command:top or htop (Interactive, shows real-time resource hogs at the top).ps aux --sort=-%cpu | head (Lists the top CPU-consuming processes).
    
    **12 .Stopping a background process**
        First, find its Process ID (PID) using pgrep or ps, then terminate it:
killall sleep
# OR find PID and kill it
pgrep sleep
kill <PID>

If it refuses to stop, use a forced termination: kill -9 <PID>

**.13. Importance of Checking Running Processes**
Checking processes acts like a health checkup for your system. It allows you to:
Identify resource leaks (zombie processes or memory leaks).
Spot runaway processes consuming 100% CPU.
Detect unauthorized or suspicious applications running in the background

.**14. Viewing Background Jobs**If you started a job in the current terminal session using & (e.g., sleep 100 &), view it using:
jobs


.**15. Testing Internet Connectivity**
Use the ping command directed at a reliable public DNS server:
ping -c 4 8.8.8.8


**16. Checking the Server IP Address**Use either of the following modern commands:
ip a
# OR
ip route get 1.1.1.1 | awk '{print $7}'


**17. Confirming a Port is Listening**
Use ss or netstat to check active network ports:
sudo ss -tulpn | grep :80
# OR
sudo netstat -tuln | grep :80


**18. Troubleshooting DNS**
To check why a domain isn't resolving, use:
dig example.com (Detailed DNS lookup query).
nslookup example.com (Simple DNS check).
host example.com (Quick IP-to-domain mapping).

1**9. Remote Management from Windows**
Open Windows PowerShell or Command Prompt and use the native OpenSSH client:
ssh username@remote_server_ip


**20. Securing File Transfers**
Use scp (Secure Copy Protocol) or sftp which run over SSH:
scp backup.tar.gz username@remote_server_ip:/home/username/


**Bonus Challenge Questions**
**21. Why Linux Dominates Cloud & DevOp**
**Lightweight and efficient:** Linux doesn't require a GUI, meaning minimal RAM and CPU overhead.
**Open Source & Free**: No licensing fees, making it infinitely scalable for spinning up thousands of cloud instances.
**Automation-Friendly**: Everything in Linux is a file, making configuration management via tools like Ansible, Terraform, and Bash highly predictable.
**Native Containerization**: Technologies behind Docker containers (namespaces, cgroups) are core features built directly into the Linux kernel.


**22. Difference Between chmod 755 and chmod 644**
The fundamental difference comes down to Execution (x) privileges:
Permission       Owner                  Group            Others            Common Use Case
755 (rwxr-xr-x)  Read,Write,Execute    Read,Execute    Read,Execute        Scripts, Executables, Directories
644 (rw-r--r--)  Read, Write           Read Only       Read Only            Regular text files, source code, configs


**23. Why chmod +x script.sh is Crucial**
By default, newly created text files do not have execution privileges for security reasons. Even if a file contains valid Bash code, the Linux kernel will block it from running as a binary executable command unless the execute (+x) bit is explicitly switched on in the file's metadata.


**24. Why SSH is More Secure Than Older Methods**(Telnet/FTP)
**Encryption:** SSH encrypts all traffic (including passwords and commands) in transit, preventing packet-sniffing/Man-in-the-Middle attacks. Older protocols like Telnet sent everything in plain text.
**Key-Based Authentication:** SSH supports cryptographic key pairs (Public/Private keys), rendering brute-force password attacks useless.

**25. Process Management in Docker & Kubernetes**
In containerized architectures, containers are essentially just isolated Linux processes running on a host kernel.
**PID 1 Responsibility:** The main application inside a container runs as Process ID 1. If this process crashes or exits, the container dies
**.Resource Throttling**: Kubernetes uses Linux control groups (cgroups) to limit CPU/Memory allocation. Proper process management prevents a single faulty container from crashing the entire cloud node 
