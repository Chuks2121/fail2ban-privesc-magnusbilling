# Fail2Ban Privilege Escalation (TryHackMe - Magnus Billing)

This repository contains a practical exploitation of a misconfigured Fail2Ban service in the **Magnus Billing** room on [TryHackMe](https://tryhackme.com).

> ⚠️ This is for **educational purposes** only. Do not attempt this on unauthorized systems.

## 📚 Summary

A local privilege escalation was achieved by injecting a malicious `actionban` command into an active Fail2Ban jail, allowing execution of commands as root. This resulted in the creation of a SUID-enabled bash shell and root access.

## 📁 Contents

- `REPORT.md` – Full vulnerability write-up in a professional pentest format.
- `exploit.sh` – PoC exploit script used in the lab environment.
- `screenshots/` – [Optional] Screenshots from the exploitation process.

## 🛡️ Skills Demonstrated

- Linux privilege escalation
- Misconfiguration exploitation
- Fail2Ban internals
- Pentest report writing
