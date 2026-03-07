# OverTheWire Bandit Notes From Levels 21 Through 33

These are my personal notes and solutions while practicing Linux fundamentals through the **OverTheWire Bandit** wargame.

---

# Level 21

## Challenge
A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

## Solution 
**Step 1 - List the directory:**

```
ls /etc/cron.d
```

---

**Step 2 — Read the cronjob file**

Look at its contents:

```
cat /etc/cron.d/cronjob_bandit22
```
```
@reboot bandit22 /usr/bin/cronjob_bandit22.sh
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh
```

That means:

- Every minute, as user **bandit22**, cron runs the script:
    
```
/usr/bin/cronjob_bandit22.sh
```
    

---

**Step 3 — Read the script**

Inspect the script:

```
cat /usr/bin/cronjob_bandit22.sh
```

```
#!/bin/bash
chmod 644 /tmp/bandit22_pass
cat /etc/bandit_pass/bandit22 > /tmp/bandit22_pass
```

---

**Step 4 — Read the file that cron writes**

Since cron runs this script every minute as user **bandit22**, it writes the password into `/tmp` with open permissions.

Look in `/tmp`:

```
ls -l /tmp
```

Look for a file mentioned in the script.
Then read it:

```
cat /tmp/bandit22_pass
```

That printed value is the **password for bandit22**

## **Explanation - Why this works**

- Cron runs the script **as bandit22**, which means the script **can read** `/etc/bandit_pass/bandit22`
- The script **copies that password into /tmp**, which *you* can access
- You just read the output file

---
