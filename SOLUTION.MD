# SSH Connection and Security Best Practices Lab - Solution

**Student Name:** Pragash KUMARAVEL 
**Date Completed:** 14-0762026

---

## Instance Details

- **Instance ID:** [i-0a7fbb1d58070c9af]
- **Public IP:** [15.188.65.58 — redact if repo is public]
- **Security Group:** [week2-web-server-sg]
- **Key Pair:** [bootcamp-week2-key.pem]

---

## Step 1: Create SSH Config File

![SSH Config Working](Screenshots/ssh-config-working.png)

**My `~/.ssh/config`:**
```
Host bootcamp-web
    HostName 15.188.65.58
    User ec2-user
    IdentityFile ~/.ssh/bootcamp-week2-key.pem
    ServerAliveInterval 60
    ServerAliveCountMax 3
```

- [x] Connected using the alias: `ssh bootcamp-web`

---

## Step 2: Test Security Group Restrictions

![Security Tests](Screenshots/security-tests.png)

| Test | Expected | Result |
|------|----------|--------|
| `ssh bootcamp-web` | ✅ Connects | [x] |
| `curl -I http://YOUR_PUBLIC_IP` | ✅ HTTP 200 | [x] |
| `ping YOUR_PUBLIC_IP` | ❌ Timeout | [x] |
| `nc -zv YOUR_PUBLIC_IP 3306` | ❌ Blocked | [x] |

**Why did ping and port 3306 fail?**

Ping failed because inbound riles only allow TCP port > 22 SSH and 80 for HTTP, there no rules set for ICMP traffic. same logic for 3306 for port 3306 is the default port MySQL/MariaDB.

---

## Step 3: Modify Security Group Rules

![Security Group Edit](Screenshots/security-group-edit.png)

- [x] Added HTTPS rule (port 443, source 0.0.0.0/0)
- [x] Tested it — what happened? [cmd not found because no HTTPS service running]
- [X] Removed the HTTPS rule again

---

## Step 4: Configure SSH Connection Timeouts

**What these settings do:**

- `ServerAliveInterval 60` — Send keep-alive every 60 seconds
- `ServerAliveCountMax 3` — Disconnect after 3 failed keep-alives
- `TCPKeepAlive yes` — Enable TCP keep-alive
- `Compression yes` — Compress data (faster on slow connections)

**Idle for 2 minutes — did the connection stay alive?** [Yes]

---

## Step 5: Practice SCP (Secure Copy)

![SCP File Transfer](Screenshots/scp-file-transfer.png)

- [x] Copied a file TO the instance
- [x] Copied a file FROM the instance
- [x] Copied a whole directory

**Commands I used:**
```bash
[echo "Hello from my computer!" > test-file.txt
scp test-file.txt bootcamp-web:~/]
```

---

## Step 6: Harden SSH Configuration (On Instance)

![SSH Hardening](Screenshots/ssh-hardening.png)

**Output of `sudo sshd -T | grep -i "passwordauthentication\|permitrootlogin\|pubkeyauthentication"`:**
```
[Paste output here]
```

| Setting | Expected | Actual |
|---------|----------|--------|
| PasswordAuthentication | no | [x] |
| PermitRootLogin | without-password | [x] |
| PubkeyAuthentication | yes | [x] |

> Only verify these settings — do not modify `sshd_config`.

---

## Step 7: Create Connection Aliases with Scripts

**My `connect-bootcamp.sh`:**
```bash
#!/bin/bash
# Quick connection script for bootcamp instance
# Colors for output
GREEN='\033[0;32m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color
echo -e "${BLUE}Connecting to bootcamp instance...${NC}"
ssh bootcamp-web
```

- [x] Made it executable and tested it

---

## Step 8: Security Group Audit

![Security Group Audit](Screenshots/security-group-audit.png)

**Inbound rules:**

| Port | Source | Purpose |
|------|--------|---------|
| [22] | [YOUR_IP/32] | [SSH access] |
| [80] | [0.0.0.0/0] | [Web traffic] |

**Is this configuration secure? What would you improve?**

Yes, this configuration is secure. SSH (port 22) is locked to a single IP, minimizing exposure to brute-force attacks, while port 80 is open to all as expected for a public web server.

---

## Step 9: Troubleshooting

**Problems I ran into and how I fixed them:**

NANO file copy paste > so I did it notepad


---

## Step 10: Security Best Practices

**Which advanced practices did you try or read about (custom SSH port, MFA, session logging)?**

[Your answer]

---

## Bonus Challenges (Optional)

- [ ] **Challenge 1:** Restrict outbound traffic — what broke? [Your answer]
- [ ] **Challenge 2:** Multiple security groups — how do they combine? [Your answer]
- [ ] **Challenge 3:** Connection multiplexing — was it faster? [Your answer]
- [ ] **Challenge 4:** Port forwarding with `ssh -L` — what is it useful for? [Your answer]

---

## Reflection Questions

### 1. Why use an SSH config file instead of typing the full command every time?

[Your answer]

### 2. Why restrict SSH to your IP instead of 0.0.0.0/0?

[Your answer]

### 3. What did the blocked ping and port 3306 tests teach you about security groups?

[Your answer]

### 4. Why disable password authentication for SSH?

[Your answer]

---

## Key Learnings

**What was most challenging about this lab?**

[Your reflection]

**What security practice will you always follow from now on?**

[Your reflection]

---

## Checklist

- [ ] SSH config file working with alias
- [ ] Security group tests done and documented
- [ ] Security group rule added and removed
- [ ] SCP tested both directions
- [ ] SSH daemon settings verified
- [ ] Connection script created
- [ ] Security group audited
- [ ] All screenshots captured
- [ ] Reflection questions answered
- [ ] Work committed to Git
- [ ] Pull request created

---

**Completed By:** [Your Name]  
**Date:** [Date]
