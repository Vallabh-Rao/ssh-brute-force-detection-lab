# SSH Brute Force Detection and Prevention Lab

## Overview
This project demonstrates how to detect and prevent SSH brute force attacks on a Linux system using Fail2Ban.

## Objective
Simulate repeated SSH login failures and automatically block malicious IP addresses.

## Tools Used
- Linux
- SSH
- Fail2Ban
- System Logs

## Commands Used

Check SSH service
sudo systemctl status ssh

Monitor SSH logs
sudo journalctl -u ssh -f

Check Fail2Ban service
sudo systemctl status fail2ban

Check SSH jail status
sudo fail2ban-client status sshd

Check firewall rules
sudo iptables -L

## Outcome

- Successfully simulated SSH brute force login attempts
- Fail2Ban detected repeated failures
- Attacking IP addresses were automatically banned
