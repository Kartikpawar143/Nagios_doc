# Cockpit Installation and Configuration on RHEL 9.8
## Objective

Install, enable, and access Cockpit on Red Hat Enterprise Linux (RHEL) 9.8 for web-based server administration.

---

# Prerequisites

- RHEL 9.8 installed
- Root or sudo privileges
- Internet connectivity (for package installation)
- Firewall enabled (recommended)

---
# Step 1: Verify Whether Cockpit is Installed

Check if Cockpit is already available on the system.

```bash
rpm -q cockpit
```

### Expected Output

If installed:

```text
cockpit-311.1-1.el9.x86_64
```

If not installed:

```text
package cockpit is not installed
```

---
# Step 2: Update the System

Update all installed packages.

```bash
sudo dnf update -y
```

---

# Step 3: Install Cockpit

Install Cockpit using DNF.

```bash
sudo dnf install cockpit -y
```

Optional modules:

```bash
sudo dnf install cockpit cockpit-storaged cockpit-packagekit -y
```
| Package | Purpose |
|----------|---------|
| cockpit | Main Cockpit web interface |
| cockpit-storaged | Storage and disk management |
| cockpit-packagekit | Package management from the web UI |

---

# Step 4: Enable and Start Cockpit

Enable the Cockpit socket so it starts automatically after every reboot.

```bash
sudo systemctl enable --now cockpit.socket
```

---

# Step 5: Verify the Service

Check whether Cockpit is running.

```bash
sudo systemctl status cockpit.socket
```

Expected output:

```text
Active: active (listening)
```

---
# Step 6: Verify Port 9090

Cockpit listens on TCP port **9090**.

Check using:

```bash
sudo ss -tulpn | grep 9090
```

Expected output:

```text
LISTEN 0 4096 *:9090
```

---
# Step 7: Configure Firewall

If Firewalld is running, allow Cockpit traffic.

```bash
sudo firewall-cmd --permanent --add-service=cockpit
sudo firewall-cmd --reload
```

Verify:

```bash
sudo firewall-cmd --list-services
```

Expected output should include:

```text
cockpit
```

---


# Step 8: Check SELinux Status

Verify SELinux.

```bash
getenforce
```

Expected output:

```text
Enforcing
```

No configuration changes are required.

---
# Step 9: Find the Server IP Address

Retrieve the IP address.

```bash
hostname -I
```

Example:

```text
192.168.1.100
```

---
# Step 10: Access Cockpit

Open a web browser and navigate to:

```
https://<server-ip>:9090
```

Example:

```
https://192.168.1.100:9090
```

If using the local machine:

```
https://localhost:9090
```

---

# Step 11: Log In

Authenticate using your Linux account.

Example:

```
Username: root
Password: <root-password>
```

or

```
Username: <linux-user>
Password: <user-password>
```

---
# Step 12: Ignore the HTTPS Warning

Cockpit uses a self-signed SSL certificate by default.

In the browser:

- Click **Advanced**
- Click **Proceed to the site**

---
# Step 13: Verify Cockpit from the Terminal

Check that Cockpit is responding.

```bash
curl -k https://localhost:9090
```

Or verify the service.

```bash
systemctl status cockpit.socket
```

---

# Useful Commands

## Restart Cockpit

```bash
sudo systemctl restart cockpit.socket
```

---

## Stop Cockpit

```bash
sudo systemctl stop cockpit.socket
```

---

## Start Cockpit

```bash
sudo systemctl start cockpit.socket
```

---

## Enable at Boot

```bash
sudo systemctl enable cockpit.socket
```

---

## Disable at Boot

```bash
sudo systemctl disable cockpit.socket
```

---

## View Logs

```bash
sudo journalctl -u cockpit.socket
```

---

## Check Listening Port

```bash
sudo ss -tulpn | grep 9090
```

---

## Verify Installed Version

```bash
rpm -q cockpit
```

---

# Troubleshooting

## Cockpit Does Not Start

Check service status.

```bash
systemctl status cockpit.socket
```

View logs.

```bash
journalctl -xeu cockpit.socket
```

---

## Port 9090 Not Reachable

Verify the listening port.

```bash
ss -tulpn | grep 9090
```

Open the firewall.

```bash
firewall-cmd --permanent --add-service=cockpit
firewall-cmd --reload
```

---

## Package Installation Fails

Clean DNF cache.

```bash
sudo dnf clean all
sudo dnf makecache
```

Retry installation.

```bash
sudo dnf install cockpit -y
```

---

# Verification Checklist

| Task | Status |
|------|--------|
| Cockpit installed | ✅ |
| Service enabled | ✅ |
| Service running | ✅ |
| Port 9090 listening | ✅ |
| Firewall configured | ✅ |
| Browser access successful | ✅ |
| Login successful | ✅ |

---

# Summary

Cockpit provides a browser-based interface for administering Linux servers. It enables administrators to monitor system performance, manage services, configure networking, inspect logs, manage storage, and perform routine administration tasks without relying solely on the command line. By default, Cockpit is accessible over HTTPS on port **9090** and integrates directly with the system's existing user accounts for authentication.
