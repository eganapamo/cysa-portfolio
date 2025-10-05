Linux Log Analysis Lab 🧠

Objective:  
The goal for me was to identify brute-force SSH attempts using Linux command-line tools.

---

🔍 Scenario

I am a SOC analyst reviewing authentication logs on a Linux server.  
I suspected repeated failed SSH login attempts coming from a single IP address.

Log sample from /var/log/auth.log:

Oct 5 13:45:02 server sshd[2231]: Failed password for root from 192.168.56.105 port 58933 ssh2

Oct 5 13:45:04 server sshd[2231]: Failed password for root from 192.168.56.105 port 58934 ssh2

Oct 5 13:45:07 server sshd[2231]: Failed password for root from 192.168.56.105 port 58935 ssh2

Oct 5 13:45:11 server sshd[2231]: Accepted password for root from 192.168.56.105 port 58937 ssh2

---

🧰 Commands Used

1️⃣ Count Failed Login Attempts

grep "Failed password" /var/log/auth.log | wc -l

This command counts the total number of failed SSH logins.

2️⃣ Identify Top Offending IPs

grep "Failed password" /var/log/auth.log | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr

This command lists which IP addresses caused the most failed attempts.

3 192.168.56.105

grep "Accepted password" /var/log/auth.log

Oct  5 13:45:11 server sshd[2231]: Accepted password for root from 192.168.56.105 port 58937 ssh2

🧩 Analysis

I identified that IP 192.168.56.105 had three failed SSH attempts before one successful login — a classic brute-force pattern.

Mitigation Steps:
Disable root SSH login
Configure fail2ban to block repeated login attempts
Enforce stronger passwords and lockout policies

🧠 Lesson Learned

This exercise helped me strengthen my ability to detect brute-force activity through raw log analysis, one of the core skills tested on the CySA+ exam and expected in a real SOC
