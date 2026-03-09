**SSH Brute-Force Detection and Mitigation Lab**
________________________________________
Overview:
This lab demonstrates the simulation, detection, and mitigation of SSH brute-force attacks in a Linux environment. It is designed to replicate real-world attack scenarios and showcase how automated defense mechanisms like Fail2Ban can protect servers from unauthorized access.
Brute-force attacks are one of the most common threats faced by servers, where attackers repeatedly attempt to guess login credentials. This lab provides a hands-on approach to:
•	Understanding authentication log analysis
•	Implementing automated security tools
•	Observing system responses to repeated unauthorized access
________________________________________
Objectives:
•	Simulate SSH brute-force attacks in a controlled environment
•	Monitor authentication logs for suspicious activity
•	Configure Fail2Ban to automatically block malicious IPs
•	Verify firewall rules and ensure automated mitigation is functional
•	Document all commands, outputs, and configurations for reproducibility
________________________________________
Tools and Technologies:
Tool / Technology	Purpose
Linux (Debian / Ubuntu)	Server environment for SSH and Fail2Ban
OpenSSH	SSH server to simulate login attempts
Fail2Ban	Security tool for automated intrusion prevention
System Logs	Monitor authentication events (journalctl or /var/log/auth.log)
iptables	Verify and enforce firewall rules
GitHub	Repository for lab documentation and screenshots
________________________________________
Lab Setup:
1.	Install OpenSSH Server
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
2.	Verify SSH Service
sudo systemctl status ssh
Expected Output:
Active: active (running)
Screenshot: ssh_service_running.png
________________________________________
3.	Install Fail2Ban
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
sudo systemctl status fail2ban
Screenshot: fail2ban_service_running.png
________________________________________
Attack Simulation:
To simulate a brute-force attack:
1.	Open a separate terminal and attempt repeated SSH logins with incorrect credentials:
ssh fakeuser@localhost
2.	Enter wrong passwords multiple times (5–10 attempts).
3.	Monitor authentication logs to observe failed login attempts:
sudo journalctl -u ssh -f
Expected Output:
Failed password for invalid user fakeuser from 127.0.0.1
Screenshot: failed_login_attempts.png
________________________________________
Configuring Fail2Ban:
1.	Edit Fail2Ban configuration for SSH:
sudo nano /etc/fail2ban/jail.local
Add or modify the [sshd] section:
[sshd]
enabled = true
port = ssh
maxretry = 5
bantime = 600
findtime = 600
•	maxretry – Number of failed attempts before banning
•	bantime – Duration (seconds) for which the IP is banned
•	findtime – Time window for counting failures
Restart Fail2Ban:
sudo systemctl restart fail2ban
Screenshot: fail2ban_sshd_configuration.png
________________________________________
Monitoring and Verification:
1.	Check Fail2Ban status for SSH jail:
sudo fail2ban-client status sshd
Expected Output:
Status for the jail: sshd
Currently failed: 5
Total failed: 12
Currently banned: 1
Banned IP list: 127.0.0.1
Screenshot: fail2ban_status_sshd.png, banned_ip.png
2.	Verify firewall rules applied by Fail2Ban:
sudo iptables -L
Expected Output:
Chain f2b-sshd (1 references)
DROP all -- 127.0.0.1
Screenshot: firewall_block_rule.png
________________________________________
SSH Server Security Hardening (Optional Advanced Step)
Edit SSH configuration:
sudo nano /etc/ssh/sshd_config
Recommended changes:
PermitRootLogin no
PasswordAuthentication yes
MaxAuthTries 5
Restart SSH to apply changes:
sudo systemctl restart ssh
Screenshot: ssh_server_configuration.png
________________________________________
Key Learnings:
•	Understanding how authentication logs capture brute-force attempts
•	Practical experience configuring Fail2Ban for automated intrusion prevention
•	Observing the interaction between system logs, firewall rules, and attack mitigation
•	Exposure to real-world defensive mechanisms used in SOC environments
•	Hands-on understanding of Linux server hardening best practices
________________________________________
Folder Structure (For GitHub Repository):
ssh-brute-force-detection-lab/
├── README.md
├── screenshots/
│   ├── ssh_service_running.png
│   ├── failed_login_attempts.png
│   ├── fail2ban_service_running.png
│   ├── fail2ban_status_sshd.png
│   ├── banned_ip.png
│   ├── fail2ban_sshd_configuration.png
│   ├── firewall_block_rule.png
│   └── ssh_server_configuration.png
└── commands.txt  (optional: list all commands executed)
________________________________________
Outcome:
•	Successfully simulated SSH brute-force attack attempts
•	Fail2Ban detected suspicious login patterns and automatically banned IPs
•	Firewall rules verified, ensuring mitigation is effective
•	Lab demonstrates real-time automated intrusion detection and response, reflecting SOC practices
________________________________________
References
•	Fail2Ban Official Documentation
•	Linux Authentication Logs
•	OpenSSH Server Configuration
________________________________________
