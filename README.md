# Email-Spoofing-Demo-Lab-Guide



````markdown
# Email Spoofing Demo & Lab Guide

---

## 🔍 What Is Email Spoofing?

Email spoofing is when an attacker forges the **"From"** address of an email to make it appear as if it's from a trusted source (like a boss, a company, or a known contact), even though it's not.

The attacker does **not** need access to the legitimate email account — they just forge the sender field in the email headers.

---

## 🛠️ How Attackers Spoof Email (Methods)

1. **Using SMTP Servers (Simple Mail Transfer Protocol)**  
   SMTP doesn't always verify the sender's identity by default. This allows an attacker to send emails from command-line tools or custom scripts using fake "From" addresses.

**Tools commonly used:**

* Telnet / Netcat (manual method)  
* Sendmail  
* SMTP libraries in Python (like smtplib)  
* Email spoofing tools like: SET (Social Engineering Toolkit), Metasploit, SendEmail (Linux tool)

---

## ⚠️ Python Spoofing Demo Code (Educational Use Only)

```python
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg.set_content("This is a spoofed email for demonstration.")
msg['Subject'] = 'Urgent: Action Required'
msg['From'] = 'ceo@company.com'       # forged sender address
msg['To'] = 'victim@example.com'

with smtplib.SMTP('smtp.example.com', 25) as server:
    server.send_message(msg)
````

> **Note:** Replace `'smtp.example.com'` with your SMTP server.
> Most real SMTP servers block spoofed emails or require authentication and SPF/DKIM/DMARC validation.

---

## 🧰 Techniques to Detect or Prevent Email Spoofing

| Technique                 | Description                                                       |
| ------------------------- | ----------------------------------------------------------------- |
| **SPF**                   | Checks if sending server is authorized for that domain            |
| **DKIM**                  | Cryptographic signatures verify integrity and sender of email     |
| **DMARC**                 | Builds on SPF/DKIM; defines policy for handling failed checks     |
| **Email Header Analysis** | Reviewing headers for discrepancies (Return-Path, Received lines) |

---

## 👨‍💻 Real Spoofing Scenario (SOC View)

An attacker spoofs the CEO’s email and sends a message to HR requesting sensitive documents. The email passes basic SMTP delivery but **fails SPF/DKIM checks**.
Your SOC platform (e.g., Splunk, Sentinel) detects this due to DMARC failure and flags it suspicious.

---

## 📚 Summary Table

| Topic         | Description                                                 |
| ------------- | ----------------------------------------------------------- |
| Spoofing Goal | Trick receiver into trusting the email sender               |
| Attack Tools  | Telnet, Python smtplib, SET, Metasploit                     |
| Detection     | SPF, DKIM, DMARC, header analysis                           |
| SOC Role      | Detect, alert, block spoofed emails before user interaction |

---

## 🧪 Setting Up a Safe Lab Environment

### Recommended Local SMTP Servers for Testing Spoofing

| Tool        | Type               | Description                                 | OS                    |
| ----------- | ------------------ | ------------------------------------------- | --------------------- |
| MailHog     | Fake SMTP + Web UI | Captures emails, no real delivery           | Linux, macOS, Windows |
| smtp4dev    | Fake SMTP + GUI    | Windows-friendly GUI inbox                  | Windows (also Docker) |
| Postfix     | Real SMTP server   | Configurable local SMTP server              | Linux                 |
| MailCatcher | Fake SMTP + Web UI | Lightweight Ruby tool to catch emails       | Linux, macOS          |
| smtp-sink   | SMTP sink          | Accepts & discards emails (terminal output) | Linux                 |

---

### How to install **MailCatcher** on Linux (Debian/Kali)

```bash
sudo apt update
sudo apt install ruby ruby-dev build-essential libsqlite3-dev -y
sudo gem install mailcatcher
mailcatcher
```

* SMTP server: `localhost:1025`
* Web UI: [http://localhost:1080](http://localhost:1080)

---

### Python test script for MailCatcher

```python
import smtplib
from email.message import EmailMessage

msg = EmailMessage()
msg.set_content("This is a spoofed email test.")
msg['Subject'] = 'Test Alert'
msg['From'] = 'ceo@fakecorp.com'
msg['To'] = 'victim@example.com'

with smtplib.SMTP('localhost', 1025) as server:
    server.send_message(msg)
```

Open [http://localhost:1080](http://localhost:1080) in your browser to see the intercepted email.

---

### Troubleshooting Kali Linux package issues

If you get errors like `404 Not Found` or "Unable to locate package":

1. Fix your Kali sources list:

```bash
sudo nano /etc/apt/sources.list
```

Replace contents with:

```
deb https://http.kali.org/kali kali-rolling main non-free contrib
```

2. Add Kali signing key:

```bash
sudo apt install -y gnupg curl
curl -fsSL https://archive.kali.org/archive-key.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/kali.gpg
```

3. Clean and update:

```bash
sudo apt clean
sudo rm -rf /var/lib/apt/lists/*
sudo apt update
sudo apt upgrade -y
```

4. Install needed packages:

```bash
sudo apt install ruby ruby-dev build-essential libsqlite3-dev
```

---

### Alternative: Use Docker to run MailCatcher (no package dependencies)

```bash
docker run -d -p 1025:1025 -p 1080:1080 schickling/mailcatcher
```

Access Web UI: [http://localhost:1080](http://localhost:1080)

---

## ⚠️ Disclaimer

**This repository and content is for educational purposes only. Do NOT use these techniques for unauthorized or malicious activities. Always have permission when testing security.**

---

### Need help?

* Setting up your spoofing lab environment?
* Writing email header analysis scripts?
* Understanding SOC detection techniques?

Just open an issue or contact me!

---

**Happy learning & stay secure!** 🔐

---



```

Let me know if you want me to save it as a file or help with anything else!
```
