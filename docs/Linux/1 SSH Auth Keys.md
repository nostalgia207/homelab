#### 1 - Generate keypair on client machine (not the VM)

```powershell
ssh-keygen -t ed25519 -C "gacemarques@homelab"
```

> **Ed25519 is a fast, modern elliptic-curve algorithm for SSH keys that gives strong security with a small key size and no known weaknesses.**

> [!NOTE] Runs on the client not the VM. Creates `id_ed25519` (private key) and `id_ed25519.pub` (public key) in `~/.ssh/`. Optional passphrase encrypts the private key file locally.

#### 2 - Copy public key to the VM

Copy your pub key from PowerShell to the host:

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh gacemarques@IPADDRESS "cat >> ~/.ssh/authorized_keys"
```

Prompts for the account password once - last password prompt needed.

#### 3 - Verify key was added

```bash
cat ~/.ssh/authorized_keys
```

#### 4 - Test key login before changing anything else

From a fresh terminal:

```powershell
ssh gacemarques@IPADDRESS
```

Should prompt for the key passphrase (if set), not the Linux account password.

#### 5 - Disable password auth and root login

```bash
sudo nano /etc/ssh/sshd_config
```

Set:

```bash
PasswordAuthentication no
PermitRootLogin no
```

#### 6 - Restart SSH service

```bash
sudo systemctl restart ssh
```

> [!WARNING] Do not close the current SSH session until a new connection from a separate terminal is confirmed working. If something's misconfigured, the open session is the only way back in.

#### 7 - Test from a new terminal

```powershell
ssh gacemarques@IPADDRESS
```

Should only ask for the key passphrase - no Linux password prompt. Only close the original session once this is confirmed.

---

Result: SSH access now requires a valid private key, and root cannot log in via SSH at all (even with a key) - only a regular user, then `sudo` from there.

> [!WARNING] If the private key is lost or `~/.ssh/authorized_keys` is wiped, SSH access is locked out entirely. Recovery requires console access (e.g. Proxmox console), not SSH. Keep a backup of the private key.