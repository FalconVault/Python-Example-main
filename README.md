# Falcon Vault - Python Example

> Official Python SDK and loader example for [Falcon Vault Auth](https://vault.falconx64.store) - the authentication platform built for cheat developers.

![Python](https://img.shields.io/badge/Python-3.8+-3572A5?style=flat&logo=python&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows%20%7C%20Linux%20%7C%20Mac-lightgrey?style=flat)
![Dependencies](https://img.shields.io/badge/Dependencies-requests-28a745?style=flat)
![License](https://img.shields.io/badge/License-MIT-yellow?style=flat)

---

## What is Falcon Vault?

Falcon Vault is a KeyAuth-style authentication system built by Falcon Bytes. It gives your cheat or tool:

- **HWID Lock** - bind each user to one machine
- **License Keys** - generate, sell, and track keys from the dashboard
- **Live Sessions** - see who's online right now, kick them instantly
- **Blacklist** - ban HWIDs, IPs, or usernames with one click
- **Variables** - push values to your app at runtime without recompiling
- **Plan Gating** - free vs paid feature separation built in

---

## Files

| File | Purpose |
|---|---|
| `FLAuth.py` | Core SDK - drop into **any** Python project |
| `loader.py` | Full console loader example |
| `requirements.txt` | Dependencies (`requests` only) |

---

## Requirements

- Python 3.8+
- `pip install requests`

---

## Quick Start

**1. Install dependencies**
```bash
pip install requests
```

**2. Set your App ID** - open `loader.py` and change line 20:
```python
APP_ID = "YOUR_APP_ID_HERE"
```
Get your App ID from [vault.falconx64.store](https://vault.falconx64.store) → Manage Apps → Credentials.

**3. Run**
```bash
python loader.py
```

---

## What customers see

```
==================================================
         Falcon Bytes - Internal Panel
           Powered by Falcon Vault Auth
==================================================

  [1] Login
  [2] Register  (need a license key)
  [3] Exit

  > 1

  LOGIN

  Username : potato
  Password : ********

  [OK] Authentication successful!

  +------------------------------------------+
  |  Username : potato                       |
  |  Plan     : PAID                         |
  |  Expires  : 29d 12h 44m 10s             |
  |  Email    : user@example.com             |
  +------------------------------------------+
```

---

## Integrate FLAuth.py into your own project

Copy `FLAuth.py` into your project, then:

```python
from FLAuth import FLAuth

# 1. Create auth object
auth = FLAuth("YOUR_APP_ID", "1.0")

# 2. Login
result = auth.login(username, password)
if result["ok"]:
    user = auth.user
    print(f"Welcome {user['username']}!")
    print(f"Plan: {user['plan']}")
else:
    print(f"Error: {result['message']}")
    exit(1)

# 3. Register (new customer with license key)
result = auth.register(username, password, email, license_key)

# 4. Gate paid features
if auth.user["plan"] in ["paid", "vip", "lifetime"]:
    launch_premium()
else:
    print("Upgrade required.")
    exit(1)

# 5. Fetch a server-side variable
val = auth.get_var("variable_name")
```

---

## Feature gating patterns

**Pattern A - Entire tool is paid only:**
```python
# Call after login
if auth.user["plan"] not in ["paid", "vip", "lifetime"]:
    print("This tool requires a paid plan.")
    print("Buy at discord.gg/daY4c6kkVP")
    exit(1)
```

**Pattern B - Mixed free and paid features:**
```python
if choice == "1":
    run_free_feature()        # everyone can use this

elif choice == "2":
    if auth.user["plan"] in ["paid", "vip", "lifetime"]:
        run_premium_feature()
    else:
        print("Upgrade required - discord.gg/daY4c6kkVP")
```

---

## Dashboard

Manage your users, keys, sessions, and blacklist at:
**[vault.falconx64.store](https://vault.falconx64.store)**

Buy keys or upgrade plans on our Discord:
**[discord.gg/daY4c6kkVP](https://discord.gg/daY4c6kkVP)**

---

## Other SDKs

| Language | Repo |
|---|---|
| C++ | [Onyx-Auth-CPP-Example](https://github.com/1shot-1moan/Onyx-Auth-CPP-Example) |
| C# WinForms | [Onyx-Auth-CSharp-Example](https://github.com/1shot-1moan/Onyx-Auth-CSharp-Example) |
