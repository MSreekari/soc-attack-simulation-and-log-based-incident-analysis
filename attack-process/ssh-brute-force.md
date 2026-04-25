# SSH - Brute Force Attack SImulation and Log Analysis 

SSH stands for Secure Shell and runs on **port 22.** It is a network protocol to gain remote access of a system. It encrypts the data between the client and the server thus preventing Man-In-The-Middle(MITM) attacks. It is used to access a computer miles away open terminal, execute commands, move files, and share files. It is preferred over protols like telnet (Port 23) because in telnet, the data is not encrypted, the data can be clearly read.

## Working Mechanisms : 

It is a process that happens in three distinct phases. 

**Phase 1: The Handshake (Transport Layer)**
* To establish a secure, encrypted channel so that no one (including eavesdroppers) can see what is happening.
* TCP Handshake: The client connects to the server on Port 22.
* Key Exchange: Both machines use the Diffie-Hellman algorithm to mathematically calculate a shared secret key without ever sending it over the network.
* Host Verification: The server proves its identity using its Host Public Key.
* Encryption Enabled: Once the handshake is complete, all subsequent data is encrypted.

**Phase 2: Authentication**

* To verify that the person attempting to connect is actually authorized to access the system.
* This step happens inside the secure tunnel created in Phase 1.
  a.) Password Authentication: The client sends a username and password. The server checks these against its local user database. (This is the phase vulnerable to           your Brute Force attack).
  b.) Public Key Authentication: The client signs a challenge from the server using a Private Key. The server verifies the signature using the corresponding Public        Key. Because there is no password to guess, this is immune to brute force.

**Phase 3: The Connection Layer**

* To actually perform work, such as running commands or transferring files.
* Multiplexing: SSH allows multiple channels to be opened inside the single tunnel.
* Service Delivery: Once the user is authenticated, the server opens the requested service, typically a shell session (like Bash) or an SFTP session (for file transfers).
* Data Transfer: All commands, keystrokes, and output are sent as encrypted packets through the tunnel until the connection is closed.

## Attack Methodology 

1. Identified the target IP.
   Using the command - `ip a`

2. Checked whether SSH is running in the target machine.
   `sudo systemctl status ssh`

3. It was not installed so ran the below commands -
   `sudo apt update
   sudo apt install openssh-server -y
   sudo systemctl start ssh`

4. Opened kali linux terminal and entered the following command -
   `hydra -l user -P /usr/share/wordlists/rockyou.txt ssh://<UBUNTU_IP>`
   Hydra - Password cracking tool
   -l - Specifies the username
   -P - Specifies the password wordlist
   ssh:// - Target protocol (SSH)
   <target-ip> - IP address of the Ubuntu machine
   -t 2 - Limits parallel login attempts this reduces system overload

6. The rockyou.txt was not found. To extract the file, the below commands were executed - 
   1. `ls /usr/share/wordlists/` - First, look at the files.
   2. The rockyou.txt.gz file was found.
   3. `sudo gunzip /usr/share/wordlists/rockyou.txt.gz` - This file has to be extracted.
   4. `ls /usr/share/wordlists/rockyou.txt` - Now, the rockyou.txt file can be found.
  
The hydra starts brute forcing by using different usernames and passwords combinations on the port 22. 

* The SSH service on the target system receives repeated login requests.
* Each attempt is validated against system authentication mechanisms.
* Failed attempts are logged by the system.
* High-frequency login attempts create a recognizable pattern.

## Log Analysis 

Log file monitored : /var/log/auth.log
Command used : `grep "sshd" /var/log/auth.log | tail -n 20`
**Observed patterns :**

* Repeated "Failed password" entries.
* Same source IP address across multiple attempts.
* Rapid succession of login failures.
* Entries referencing the SSH service (sshd[PID]).

### Mitigation :
* **Key-Based Authentication:** Disabled password authentication in `/etc/ssh/sshd_config` to eliminate the credential-guessing vector.
* **Fail2Ban Implementation:** Installed and configured Fail2Ban to monitor `/var/log/auth.log` and automatically ban IPs after 3 failed login attempts.
* **Service Hardening:** Modified `sshd_config` to disallow `root` logins and restricted authentication retries.
* **Port Hardening:** Relocated the SSH service from the default Port 22 to a non-standard high port to reduce exposure to automated scanning bots.



  
