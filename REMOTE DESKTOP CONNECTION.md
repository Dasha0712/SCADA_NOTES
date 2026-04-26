# ✅ STEP 1: Check Network Connectivity

## Identify target laptop IP and ping it

```cmd
ping <IP_ADDRESS>
````

Example:

```cmd
ping 10.22.91.1
```

✔️ If ping works → both laptops are in network
❌ If ping fails → fix network first

---

# ✅ STEP 2: Check RDP Service

## Open Services

```cmd
Win + R → services.msc
```

Find:

* **Remote Desktop Services**

✔️ Status must be: **Running**

---

## Or using CMD:

```cmd
sc query TermService
```

If not running:

```cmd
net start TermService
```

---

# ✅ STEP 3: Enable Remote Desktop from Settings

```text
Win + I → System → Remote Desktop
```

👉 Turn ON:

* **Enable Remote Desktop**

⚠️ This step is VERY IMPORTANT (creates port 3389)

---

# ✅ STEP 4: Enable from System Properties

```cmd
Win + R → sysdm.cpl
```

Go to:

* Remote tab

Select:

* ✔️ Allow remote connections to this computer

---

# ✅ STEP 5: Turn OFF Firewall (Testing Purpose)

```cmd
netsh advfirewall set allprofiles state off
```

⚠️ Only for testing (turn back ON later)

---

# ✅ STEP 6: Enable Firewall Rule (Recommended)

```cmd
netsh advfirewall firewall set rule group="remote desktop" new enable=Yes
```

---

# ✅ STEP 7: Check Port 3389 (Server Side)

```cmd
netstat -an | find "3389"
```

✔️ Expected:

```
0.0.0.0:3389     LISTENING
```

❌ If nothing appears:

* RDP not properly enabled
* Restart the system if it still not coming go to step 9.

---

# ✅ STEP 8: Check Port from Client Laptop

```powershell
Test-NetConnection <IP_ADDRESS> -Port 3389
```

Example:

```powershell
Test-NetConnection 10.22.91.1 -Port 3389
```

✔️ Should be:

```
TcpTestSucceeded : True
```

---

# ✅ STEP 9: Force Enable RDP via Registry

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

---

# ✅ STEP 10: Restart RDP Service

```cmd
net stop TermService
net start TermService
```

---

# ✅ STEP 11: Check RDP Listener

```cmd
qwinsta
```

✔️ Expected:

```
rdp-tcp    Listen
```

---

# ✅ STEP 12: Force Port Configuration

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v PortNumber /t REG_DWORD /d 3389 /f
```

---

# 🔁 STEP 13: Restart System

```cmd
shutdown /r /t 0
```

---

# 🔍 FINAL VERIFICATION

## On Server:

```cmd
netstat -an | find "3389"
```

## On Client:

```powershell
Test-NetConnection <IP> -Port 3389
```

---

# 🧠 TROUBLESHOOTING LOGIC

| Check     | Result        | Meaning                  |
| --------- | ------------- | ------------------------ |
| Ping      | OK            | Network working          |
| Service   | Running       | RDP service active       |
| Port 3389 | Not listening | RDP not enabled properly |
| Port test | False         | Firewall / service issue |

---

# 🚨 COMMON ISSUES

* RDP not enabled ❌
* Port 3389 not listening ❌
* Firewall blocking ❌
* Antivirus blocking ❌
* Wrong network profile ❌

---

# ⚡ QUICK ALTERNATIVE

If RDP fails:

👉 Use:

* AnyDesk
* TeamViewer
* Chrome Remote Desktop

---

# ✅ QUICK CHECKLIST

* [ ] Ping working
* [ ] RDP enabled in settings
* [ ] Service running
* [ ] Port 3389 listening
* [ ] Firewall rule enabled
* [ ] Connection successful
* [ ] Make sure password has been set to the both laptop(i mean system password)

---
